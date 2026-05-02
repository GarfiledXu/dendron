---
id: gl5q5iuz61079yb4a533z2w
title: 新增指令协议在代码中的实现顺序以及框架规范
desc: ''
updated: 1775111639874
created: 1775095880399
---

## nvs指令扩展流程

| 序 | 文件                 | 操作类型 | 描述 |
| ---- | :--------------------- | :--------- | ------ |
| 1  | nvs_ncm_tcp_client.c |          |      |
|    |                      |          |      |
|    |                      |          |      |
|    |                      |          |      |
|    |                      |          |      |
|    |                      |          |      |
|    |                      |          |      |
|    |                      |          |      |

1. 确定协议类型后找到映射表 添加指令和对应处理函数  将指令进行路由处理
   1. test/nvs/src/ncm/nvs_ncm_tcp_client.c

        ```c++
        //指令名称注册 以及定义的指令路由处理函数注册
        static const nvs_dispatch_entry_t g_dispatch_table[] = {
        // 4.1.2 连接
        {"Get_DevCap", nvs_ncm_get_dev_cap},
        {"my_test_cmd", nvs_ncm_my_test_cmd},//示例
        ...
        //在当前文件，定义指令路由处理函数
        static void nvs_ncm_my_test_cmd(nvs_ncm_session_t* session) {
            //绑定指令对应的处理回调(在nvs/src/core/nvs_device_mgr_ncm_cb_impl.c中定义)和负载类型 负载类型取决于指令的设定逻辑
            INVOKE_NDM_CB(session, g_ndm->on_get_dev_cap_request, NVS_NCM_RESP_BODY_TYPE_JSON, session->request_jobj, session->respond_jobj);
        }
        ```

2. 定义在指令处理函数中使用到的request回调接口原型，定义实现，绑定到函数表 将请求进行转发，交由ndm处理
   1. 定义接口原型
      1. nvs/inc/nvs_ncm.h
      2. sample: `nvs_ncm_ret_t (*on_search_device_request)(nvs_ncm_session_t *session, int32_t resp_body_type, void *user_arg, const void *req_jobj, void *resp_jobj);`
   2. 回调实现
      1. nvs/src/core/nvs_device_mgr_ncm_cb_impl.c
      2. sample

            ```c++
            //主要将请求发送到ndm主线程处理
            static nvs_ncm_ret_t on_get_serial_request(nvs_ncm_session_t *session, int32_t resp_body_type, void *user_arg, const void *req_jobj, void *resp_jobj)
            {
                nvs_device_mgr_t *device_mgr = (nvs_device_mgr_t *)user_arg;

                /* 异步模式：发送消息到主线程处理 */
                send_ndm_ncm_request_to_main_thread(device_mgr, NVS_NDM_NCM_REQ_GET_SERIAL,
                                                    session, resp_body_type, req_jobj, resp_jobj,
                                                    NULL, 0);

                return 0;
            }
            ```

      3. 回调绑定
         1. nvs/src/core/nvs_device_mgr_ncm_cb_impl.c
         2. sample

            ```c++
            static nvs_ncm_callback_t s_ncm_cb = {
            .on_search_device_request = on_search_device_request,
            .on_set_alias_request = on_set_alias_request,
            .on_set_network_request = on_set_network_request,
            .on_sys_reboot_request = on_sys_reboot_request,
            ```

3. 定义请求在ndm中的处理实现，定义处理类型，以及实现处理代码块
   1. nvs/src/core/nvs_device_mgr.c
   2. 定义ndm的处理类型，存在异步请求处理的需求时，作为send_ndm_ncm_request_to_main_thread的参数带入

        ```c++
            NVS_NDM_NCM_REQ_READ_RESULT = 127,          /* 读取结果 */
            NVS_NDM_NCM_REQ_STOP_AUTO_TUNE = 128,       /* 停止自动调节 */



            /* user tcp 用的 */
            NVS_NDM_NCM_REQ_USER_CONNECT = 501,         /* user连接设备 */
            NVS_NDM_NCM_REQ_USER_DISCONNECT = 502,      /* user断开连接 */

            NVS_NDM_NCM_REQ_READ_PROG_ID = 503,          /* 读取程序ID */
            NVS_NDM_NCM_REQ_SWITCH_PROG = 504,           /* 切换程序 */
            NVS_NDM_NCM_REQ_READ_PROG_NAME = 505,        /* 读取程序名称 */

        } nvs_ndm_ncm_request_type_e;
        ```

   3. 实现请求处理的代码块，位于nvs/src/core/nvs_device_mgr.c:process_ndm_ncm_request

        ```c++
        switch (req_data->request_type) {
        case NVS_NDM_NCM_REQ_SEARCH_DEVICE: {
            NVS_LOG_INFO(ndm_mod, "Processing NVS_NDM_NCM_REQ_SEARCH_DEVICE in main thread\n");

            // 从配置获取设备信息（alias、产品名、SN、版本号）
            cJSON *root_obj = (cJSON*)nvs_cfg_mgr_get_obj();
            cJSON *system_obj = cJSON_GetObjectItem(root_obj, "system");
            cJSON *device_info = cJSON_GetObjectItem(system_obj, "device_info");

            // 硬编码产品名、SN、版本号
            const char *alias_str = "Unknown_Camera";
            if (dev_mgr && dev_mgr->device_info.alias && strlen(dev_mgr->device_info.alias) > 0) {
                alias_str = dev_mgr->device_info.alias;
            }
            const char *product_name = "NV5";
            const char *sn = "NV5-20260101-001";
            const char *version = "V1.0.0";

            // 从网络接口获取实际运行的网络参数
            char ip_addr[32] = {0};
            char mac_addr[32] = {0};
            char netmask[32] = {0};
            char gateway[32] = {0};

            const char *ifname = "eth0";  // TODO: 从配置获取实际网卡名
            get_ip_address(ifname, ip_addr, sizeof(ip_addr));
            get_mac_address(ifname, mac_addr, sizeof(mac_addr));
            get_netmask(ifname, netmask, sizeof(netmask));
            get_gateway(ifname, gateway, sizeof(gateway));

            // 构建响应 JSON
            cJSON_AddStringToObject(resp, "alias", dev_mgr->device_info.alias);
            cJSON_AddStringToObject(resp, "product_name", product_name);
            cJSON_AddStringToObject(resp, "sn", sn);
            cJSON_AddStringToObject(resp, "version", version);
            cJSON_AddStringToObject(resp, "ip", ip_addr);
            cJSON_AddStringToObject(resp, "mask", netmask);
            cJSON_AddStringToObject(resp, "gw", gateway);
            cJSON_AddStringToObject(resp, "mac", mac_addr);

            // 调用响应接口
            if (ncm_ops->do_search_device_respond) {
                ncm_ops->do_search_device_respond(req_data->session, req_data->req_jobj, resp);
            }

            break;
        }
        ```

   4. 请求响应的 接口原型+实现+绑定
      1. 接口原型声明
         1. nvs/inc/nvs_ncm.h

             ```c++
                void (*do_user_read_prog_id_respond)(nvs_ncm_session_t *session, const char *raw_str);
                void (*do_user_switch_prog_respond)(nvs_ncm_session_t *session, const char *raw_str);
                void (*do_user_read_prog_name_respond)(nvs_ncm_session_t *session, const char *raw_str);

            } nvs_ncm_ops_t;
             ```

      2. 实现定义
         1. nvs/src/ncm/nvs_ncm_tcp_client.c

            ```c++
            static void nvs_ncm_do_user_read_prog_id_respond(nvs_ncm_session_t* s, const char* result) {
                nvs_ncm_send_simple_user_tcp_resp(s, "PR", result);
            }
            ```

      3. 接口实现绑定
         1. nvs/src/ncm/nvs_ncm_tcp_client.c

             ```c++
             static nvs_ncm_ops_t g_ncm_impl = {
            .do_error_respond = nvs_ncm_do_error_respond,
            .do_user_read_prog_id_respond = nvs_ncm_do_user_read_prog_id_respond,
            ...
             ```

## ncm ndm 模块间分工理解

1. static nvs_ncm_ops_t g_ncm_impl
   1. nvs/src/ncm/nvs_ncm_tcp_client.c
   2. ncm模块定义的操作集如：各种响应的封装，可供外部模块如ndm引用

## gpt question

目前团队的上下位机通信使用的基于udp和tcp的私有协议，
主要格式为 `<cmd>-#<paylod>\n`，其中`-#`为连接符，payload分了几个类
typedef enum {
    NVS_NCM_RESP_BODY_TYPE_JSON = 0,
    NVS_NCM_RESP_BODY_TYPE_RAW_STR = 1,
    NVS_NCM_RESP_BODY_TYPE_BLOB = 2,
    NVS_NCM_RESP_BODY_TYPE_BUTT,
} nvs_ncm_resp_body_type_e;
// 发送简单字符串响应（用于纯 OK 响应）
static void nvs_ncm_send_simple_resp(nvs_ncm_session_t* s, const char* cmd, const char* result) {
    NVS_LOG_DEBUG(ncm_mod, "Enter\n");
    char buf[256];
    int len = snprintf(buf, sizeof(buf), "%s-#%s\r", cmd, result);
    sys_session_send(s, buf, len);
    NVS_LOG_DEBUG(ncm_mod, "Exit (success)\n");
}
// 发送user tcp简单字符串响应
static void nvs_ncm_send_simple_user_tcp_resp(nvs_ncm_session_t* s, const char* cmd, const char* result) {
    NVS_LOG_DEBUG(ncm_mod, "Enter\n");
    char buf[1024];
    int len = 0;
    if (result != NULL) {
        len = snprintf(buf, sizeof(buf), "%s,%s\r", cmd, (char *)result);
    } else {
        len = snprintf(buf, sizeof(buf), "%s\r", cmd);
    }
    sys_session_send(s, buf, len);
    NVS_LOG_DEBUG(ncm_mod, "Exit (success)\n");
}
// 发送混合响应（JSON + 二进制数据）
static void nvs_ncm_send_hybrid(nvs_ncm_session_t* s, const char* cmd, const uint8_t* bin, size_t bin_size) {
    NVS_LOG_DEBUG(ncm_mod, "Enter\n");
    char* json_str = cJSON_PrintUnformatted(s->respond_jobj);
    if (!json_str) json_str = strdup("{}");

    char head[1024];
    int head_len = snprintf(head, sizeof(head), "%s-#%s-#", cmd, json_str);
    sys_session_send(s, head, head_len);

    free(json_str);

    if(bin && bin_size > 0) sys_session_send(s, bin, bin_size);
    sys_session_send(s, "\r", 1);
    NVS_LOG_DEBUG(ncm_mod, "Exit (success)\n");
}

目前开发备份导入导出的功能，敲定的方案是，先定义一些简单的文件服务指令接口，可以用于传输文件，查询文件状态等
然后通过定义业务协议，获取一份完整的备份文件列表，然后上位机依次根据列表文件和绝对路径，通过传输接口一个个的将文件读取到本地，最后打包，导入就是一个逆向过程，将之前存下来的清单读取，将文件一个个传输回去恢复即可
由于团队目前的主架构是c，所以可能模块底层我用cpp写，然后暴露c接口接入，为什么需要一个模块呢，在进行文件读写的过程，下位机本质上要打开文件fd，然后通过协议获取到bin的分片，校验后一点点写入，或者读取，那么一个文件的
过程就是类似一个fd_session了，有状态的，目前的限制就是仅允许一个时刻一个文件进行读写，涉及到状态管理和fd管理，指令的功能实现基于cpp 设计一个单例类，然后再往上封装成指令和request respond处理就按框架的c来

至于相关概念命名，io_session io_session_id_是否合理
需要配置一些访问限制，比如会有接口可以设置内部哪些路径目录或者文件是可以通过这个ncm_fs来读写的，如果上位机请求了非法的路径，那么返回对应的错误，首先有这个机制，即设置接口，以及查询返回接口
具体的指令功能还需要讨论
1. 查询-支持功能:查询设备文件服务能力，支持的块大小、压缩方式及支持访问的根路径
2. 查询-单文件状态：查询单个文件详细状态，返回路径，返回文件大小、MD5等信息
3. 查询-文件列表：遍历指定目录下的文件，{"path":"/prog","recursive":1,"max_count":1000,"cursor":0}返回文件列表：返回文件数量，文件大小，属性是否为文件夹等
4. io-读取文件打开：返回分配的 Session ID 及文件总大小，以及md5等
5. io-文件读取：按偏移量，请求文件数据，返回 JSON 描述及紧跟的二进制数据