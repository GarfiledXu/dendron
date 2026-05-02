---
id: s8a5mln5dyjieov735gynua
title: 备份导入导出指令扩展流程
desc: ''
updated: 1775540890480
created: 1775529990429
---


## 1. 定义 NCM 请求处理回调接口原型

* **文件位置:** `nv/app/nvs/inc/nvs_ncm.h`
* **命名格式:** `nvs_ncm_ret_t (*on_<xx>_request)(...);`
* **携带二进制 payload 格式:** `nvs_ncm_ret_t (*on_<xxx>_request)(nvs_ncm_session_t *session, <xxx_bin_payload>, int32_t resp_body_type, void *user_arg, const void *req_jobj, void *resp_jobj);`

```c
typedef struct {
    // --- 4.1.1 设备管理 ---
    nvs_ncm_ret_t (*on_search_device_request)(nvs_ncm_session_t *session, int32_t resp_body_type, void *user_arg, const void *req_jobj, void *resp_jobj);
    nvs_ncm_ret_t (*on_set_alias_request)(nvs_ncm_session_t *session, int32_t resp_body_type, void *user_arg, const void *req_jobj, void *resp_jobj);
    nvs_ncm_ret_t (*on_set_network_request)(nvs_ncm_session_t *session, int32_t resp_body_type, void *user_arg, const void *req_jobj, void *resp_jobj);
} nvs_ncm_callback_t;
```

---

## 2. 定义 NCM 响应处理回调接口原型

* **文件位置:** `nv/app/nvs/inc/nvs_ncm.h`
* **纯 JSON 格式:** `void (*do_<xx>_respond)(nvs_ncm_session_t *session, const void *req_jobj, const void *resp_jobj);`
* **携带二进制 payload 格式:** `void (*do_<xx>_respond)(nvs_ncm_session_t *session, <xxxx_bin_payload_param>, const void *req_jobj, const void *resp_jobj);`

```c
typedef struct {
    // --- 4.1.1 设备管理 ---
    void (*do_search_device_respond)(nvs_ncm_session_t *session, const void *req_jobj, const void *resp_jobj);
    void (*do_set_alias_respond)(nvs_ncm_session_t *session, const void *req_jobj, const void *resp_jobj);
    void (*do_set_network_respond)(nvs_ncm_session_t *session, const void *req_jobj, const void *resp_jobj);

    void (*do_get_prog_thumb_respond)(nvs_ncm_session_t *session, const uint8_t* img_data, size_t img_size, const void *req_jobj, const void *resp_jobj);
} nvs_ncm_ops_t; 
```

---

## 3. 实现 NCM 请求处理回调

* **文件位置:** `nv/app/nvs/src/core/nvs_device_mgr_ncm_cb_impl.c`
* **命名格式:** `nvs_ncm_ret_t on_<xxx>_request(...)`

```c
// --- 4.1.1 设备管理 ---
static nvs_ncm_ret_t on_search_device_request(nvs_ncm_session_t *session, int32_t resp_body_type, void *user_arg, const void *req_jobj, void *resp_jobj)
{
    nvs_device_mgr_t *device_mgr = (nvs_device_mgr_t *)user_arg;

    /* 异步模式：发送消息到主线程处理 */
    send_ndm_ncm_request_to_main_thread(device_mgr, NVS_NDM_NCM_REQ_SEARCH_DEVICE,
                                        session, resp_body_type, req_jobj, resp_jobj,
                                        NULL, 0);

    // TODO: 解析 req_jobj 获取搜索参数，执行搜索逻辑，构建 resp_jobj 返回结果
    // 注意：实际处理逻辑应在主线程的消息处理函数中实现

    return 0;
}
```

---

## 4. 绑定请求处理回调

* **文件位置:** `nv/app/nvs/src/core/nvs_device_mgr_ncm_cb_impl.c`
* **绑定格式:** `.on_<xxx>_request = on_<xxx>_request`

```c
static nvs_ncm_callback_t s_ncm_cb = {
    .on_search_device_request = on_search_device_request,
    .on_set_alias_request = on_set_alias_request,
    .on_set_network_request = on_set_network_request,
};
```

---

## 5. 实现 NCM 请求响应回调

* **文件位置:** `nv/app/nvs/src/ncm/nvs_ncm_tcp_client.c`
* **命名格式:** `nvs_ncm_do_<xxx>_respond`

```c
// --- 4.1.1 ---
static void nvs_ncm_do_search_device_respond(nvs_ncm_session_t *session, const void *req_jobj, const void *resp_jobj) {
    NVS_LOG_DEBUG(ncm_mod, "Enter\n");
    (void)req_jobj;
    char pc_ip[32];

    if (!session || session->fd <= 0) {
        NVS_LOG_ERROR(ncm_mod, "Error: Invalid session or FD in Search respond\n");
        return;
    }

    if (resp_jobj) {
        char* json_str = cJSON_PrintUnformatted((cJSON*)resp_jobj);
        if (!json_str) {
            json_str = strdup("{}"); 
        }

        inet_ntop(AF_INET, &session->udp_client_addr.sin_addr, pc_ip, sizeof(pc_ip));

        char buf[4096];
        int len = snprintf(buf, sizeof(buf), "Search_Device-#%s\r", json_str);
        nvs_modify_host_route(pc_ip, "eth0", 1);

        NVS_LOG_INFO(ncm_mod, "Sending UDP: %s\n", buf);

        sys_session_send(session, buf, len);
        free(json_str);

        route_cleanup_args_t *cleanup_args = malloc(sizeof(route_cleanup_args_t));
        if (cleanup_args) {
            strncpy(cleanup_args->pc_ip, pc_ip, sizeof(cleanup_args->pc_ip) - 1);
            cleanup_args->pc_ip[sizeof(cleanup_args->pc_ip) - 1] = '\0';

            pthread_t tid;
            pthread_attr_t attr;
            pthread_attr_init(&attr);
            pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);
            if (pthread_create(&tid, &attr, nvs_route_cleanup_task, cleanup_args) != 0) {
                NVS_LOG_ERROR(ncm_mod, "Failed to create cleanup thread, fallback to immediate delete.\n");
                nvs_modify_host_route(pc_ip, "eth0", 0);
                free(cleanup_args);
            }
            pthread_attr_destroy(&attr);
        } else {
            nvs_modify_host_route(pc_ip, "eth0", 0);
        }
    } else {
        nvs_ncm_send_json_resp_ex(session, "Search_Device", resp_jobj);
    }
    NVS_LOG_DEBUG(ncm_mod, "Exit (success)\n");
}
```

---

## 6. 绑定请求响应处理回调

* **文件位置:** `nv/app/nvs/src/ncm/nvs_ncm_tcp_client.c`
* **绑定格式:** `.do_<xx>_respond = nvs_ncm_do_<xxx>_respond`

```c
static nvs_ncm_ops_t g_ncm_impl = {
    .do_error_respond = nvs_ncm_do_error_respond,
    .do_search_device_respond = nvs_ncm_do_search_device_respond,
};
```

---

## 7. 定义指令的路由处理函数

* **文件位置:** `nv/app/nvs/src/ncm/nvs_ncm_tcp_client.c`
* **命名格式:** `nvs_ncm_<xxx>(nvs_ncm_session_t* session)`
* **说明:** 内部调用回调为 NDM `s_ncm_cb` 中定义的回调

```c
static void nvs_ncm_get_dev_cap(nvs_ncm_session_t* session) {
    INVOKE_NDM_CB(session, g_ndm->on_get_dev_cap_request, NVS_NCM_RESP_BODY_TYPE_JSON, session->request_jobj, session->respond_jobj);
}
```

---

## 8. 定义指令名称和绑定路由处理函数

* **文件位置:** `nv/app/nvs/src/ncm/nvs_ncm_tcp_client.c`
* **绑定格式:** `{"<cmd_name>", nvs_ncm_<xxx>}`

```c
static const nvs_dispatch_entry_t g_dispatch_table[] = {
    // 4.1.2 连接
    {"Get_DevCap", nvs_ncm_get_dev_cap},
    {"Get_Serial", nvs_ncm_get_serial},
    {"Connect", nvs_ncm_connect},
};
```

---

## 9. 定义 NDM 请求处理类型

* **文件位置:** `nv/app/nvs/inc/nvs_device_mgr.h`
* **命名格式:** `NVS_NDM_NCM_REQ_<XXX>`

```c
typedef enum {
    /* 输出逻辑 (Output Logic) */
    NVS_NDM_NCM_REQ_SET_OUT_LOGIC = 58,         /* 设置输出逻辑 */
    NVS_NDM_NCM_REQ_SET_OUT_FMT_MODE = 59,      /* 设置输出格式模式 */
    NVS_NDM_NCM_REQ_SET_CUSTOM_FMT = 60,        /* 设置自定义格式 */
} nvs_ndm_ncm_request_type_e;
```

---

## 10. 在事件循环中实现对应的 NDM 处理逻辑，组合响应

* **文件位置:** `nv/app/nvs/src/core/nvs_device_mgr.c`
* **分支格式:** `case NVS_NDM_NCM_REQ_<XXXX>:`

```c
static void process_ndm_ncm_request(nvs_device_mgr_t *dev_mgr, nvs_ndm_ncm_request_data_t *req_data)
{
    //.....
    switch (req_data->request_type) {
        case NVS_NDM_NCM_REQ_SW_PROG: {
            NVS_LOG_INFO(ndm_mod, "Processing NVS_NDM_NCM_REQ_SW_PROG in main thread\n");
            
            cJSON *id_json = cJSON_GetObjectItem(req_data->req_jobj, "id");
            int32_t id = cJSON_GetNumberValue(id_json);
            
            int ret = nvs_pm_switch_cur_prog(id);
            if (!ret) {
                ret = nvs_ndm_ncm_sw_prog_postprocess(dev_mgr);
                if (ret != 0) {
                    ncm_ops->do_error_respond(req_data->session, ret);
                } else {
                    cJSON_AddNumberToObject(resp, "result", ret);
                    if (ncm_ops->do_sw_prog_respond) {
                        ncm_ops->do_sw_prog_respond(req_data->session, req_data->req_jobj, resp);
                    }
                }
            } else {
                ncm_ops->do_error_respond(req_data->session, ret);
            }
            break;
        }
        //...
    }
}
```
