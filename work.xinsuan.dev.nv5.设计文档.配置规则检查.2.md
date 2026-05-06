---
id: psl4kerzkip841y25r7znyh
title: '2'
desc: ''
updated: 1778056906545
created: 1778056904552
---

# 智能相机 Pre-Rule Check 模块核心代码实现与扩展规范 (V3.0)

针对“如何具体实现回调”、“集中化组装报文工具”以及“下位机自用”的问题，本说明提供底层的纯 C 语言工程代码结构及实现范例。

## 1. 关于“下位机自用”的定性说明

**可以且建议自用。** Pre-Rule Check 本质上是一个纯函数式、无状态的数据校验服务。下位机内部的定时任务、内部状态机自动切换（如从空闲态自动切换到运行态），在触发真实动作前，均可以直接调用 `nvs_pre_rule_check_run` 获取当前状态的合法性，防止内部逻辑死锁或崩溃。

## 2. 集中化诊断引擎设计 (Utility API)

为了避免各个模块在实现回调时重复编写组装错误数组的底层逻辑，预检引擎需提供一套统一的**上下文管理器 (Check Context)** 和**组装 API**。各模块仅需调用该 API 压入错误项。

### 2.1 引擎核心结构与 API

```c
// 校验结果数组管理上下文
typedef struct {
    nvs_check_res_t* results;     // 外部传入的数组指针
    int max_capacity;             // 数组最大容量
    int current_count;            // 当前已记录的错误数量
    int global_status;            // 0: PASS, -1: FAIL (存在 FATAL)
} nvs_chk_ctx_t;

/**
 * @brief 引擎提供的通用错误压栈接口 (供各模块回调使用)
 * @param ctx   校验上下文
 * @param level 严重级别
 * @param code  错误码
 * @param path  错误路径
 * @param msg   描述信息
 */
void nvs_chk_append_error(nvs_chk_ctx_t* ctx, nvs_check_level_e level, int code, const char* path, const char* msg) {
    if (ctx->current_count >= ctx->max_capacity) return; // 防止越界
    
    nvs_check_res_t* item = &ctx->results[ctx->current_count];
    item->level = level;
    item->err_code = code;
    strncpy(item->err_path, path, sizeof(item->err_path) - 1);
    strncpy(item->err_msg, msg, sizeof(item->err_msg) - 1);
    
    if (level == NVS_CHK_FATAL) {
        ctx->global_status = -1;
    }
    ctx->current_count++;
}
```

## 3. 回调表结构注入设计

将预检接口标准地嵌入到现有的各层级实体操作集 (Ops) 中。

```c
// Node 层级回调定义
typedef struct {
    const char* tool_type;
    // ... 原有 init, execute ...
    // 新增诊断回调
    void (*diagnose_rule)(cJSON* blackboard, cJSON* payload, nvs_chk_ctx_t* ctx);
} nvs_node_ops_t;

// Prog 管理器层级回调定义
typedef struct {
    // ... 原有 load, start, stop ...
    void (*diagnose_topology)(cJSON* prog_cfg, nvs_chk_ctx_t* ctx);
} nvs_prog_mgr_ops_t;

// NDM 系统层级检查接口
void ndm_diagnose_system(nvs_chk_ctx_t* ctx);
```

## 4. 核心场景代码实现示例

以下为三个典型层级的落地实现代码，展示如何利用引擎工具函数和能力集进行快速扩展。

### 4.1 场景一：Node 层级 (OCR 工具) 规则与状态验证

**校验目标：** 1. 强制校验用于底层句柄生成的 `mode` 参数。
2. 校验参数是否超出能力集限制。

```c
// ocr_tool.c
static void ocr_diagnose_rule(cJSON* blackboard, cJSON* payload, nvs_chk_ctx_t* ctx) {
    cJSON* target = (payload != NULL) ? payload : blackboard;
    if (!target) return;

    // 1. 强依赖校验：mode 参数绝不能缺省，否则内部句柄无法生成
    cJSON* mode_item = cJSON_GetObjectItem(target, "mode");
    if (mode_item == NULL) {
        nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1020, "mode", 
                             "Mandatory parameter 'mode' missing for handle generation.");
        // 若 mode 缺失，后续校验无意义，直接返回
        return; 
    }

    // 2. 能力集联动校验：阈值上限
    cJSON* thresh_item = cJSON_GetObjectItem(target, "threshold");
    if (thresh_item != NULL) {
        int max_thresh = 0;
        // 假设能力集中定义了 ocr.threshold_max
        if (nvs_cap_get_int("atool.ocr.threshold_max", &max_thresh) == NVS_CAP_OK) {
            if (thresh_item->valueint > max_thresh) {
                nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1004, "threshold", 
                                     "Threshold exceeds capability limit.");
            }
        }
    }
    
    // 3. 状态预判 (仅当 payload 为空，即触发执行前检查黑板状态)
    if (payload == NULL) {
        // 伪代码：检查底层库是否已加载字库
        if (!ocr_lib_is_dict_loaded()) {
            nvs_chk_append_error(ctx, NVS_CHK_WARN, 2001, "status", 
                                 "Dictionary not loaded, execution may fail.");
        }
    }
}

// 注册回调
nvs_node_ops_t ocr_ops = {
    .tool_type = "OCR_TOOL",
    .diagnose_rule = ocr_diagnose_rule
};
```

### 4.2 场景二：Prog 层级拓扑检查

**校验目标：** 检查程序中的工具数量是否超出能力集硬上限。

```c
// prog_mgr.c
static void prog_diagnose_topology(cJSON* prog_cfg, nvs_chk_ctx_t* ctx) {
    if (!prog_cfg) return;

    // 1. 能力集联动：获取系统允许的最大工具数
    int max_tools_allowed = 0;
    if (nvs_cap_get_int("atool.max_tools", &max_tools_allowed) != NVS_CAP_OK) {
        max_tools_allowed = 64; // 缺省安全值
    }

    cJSON* nodes_array = cJSON_GetObjectItem(prog_cfg, "nodes");
    if (nodes_array) {
        int current_nodes_count = cJSON_GetArraySize(nodes_array);
        if (current_nodes_count > max_tools_allowed) {
            // 注意 err_path 填入能指向整个 nodes 数组的路径
            nvs_chk_append_error(ctx, NVS_CHK_FATAL, 3001, "nodes", 
                                 "Total node count exceeds system capability limits.");
        }
        
        // 2. 遍历节点级联调用下层诊断 (引擎调度逻辑)
        // ...
    }
}
```

### 4.3 场景三：System 层级 (NDM) 资源与清单检查

**校验目标：**
拦截上位机下发的非法工具类型。

```c
// ndm_system_check.c
void ndm_diagnose_system_node_creation(const char* requested_tool_type, nvs_chk_ctx_t* ctx) {
    // 1. 能力集联动：查询支持的工具列表
    // nvs_cap_array_contains 接口返回 1 表示存在，0 表示不存在
    int is_supported = nvs_cap_array_contains("atool.supported_types", requested_tool_type);
    
    if (is_supported == 0) {
        char err_msg[128];
        snprintf(err_msg, sizeof(err_msg), "Tool type '%s' is not supported by current capability.", requested_tool_type);
        nvs_chk_append_error(ctx, NVS_CHK_FATAL, 4005, "tool_type", err_msg);
    }
}
```

## 5. 引擎总控接口实现范例

展示外部如何调用，以及引擎内部的封装过程。

```c
// nvs_pre_rule_check.c

int nvs_pre_rule_check_run(nvs_check_scope_e scope, const char* target_id, cJSON* payload, nvs_check_res_t* out_array, int max_capacity) {
    // 1. 初始化上下文
    nvs_chk_ctx_t ctx;
    ctx.results = out_array;
    ctx.max_capacity = max_capacity;
    ctx.current_count = 0;
    ctx.global_status = 0; // 默认通过

    // 2. 根据 Scope 路由分发
    if (scope == NVS_CHK_SCOPE_NODE) {
        // 查找到对应的 Node Ops
        nvs_node_ops_t* ops = factory_get_node_ops_by_id(target_id);
        if (ops && ops->diagnose_rule) {
            cJSON* blackboard = get_blackboard_by_id(target_id);
            // 将上下文指针传入，业务回调中只需调 append 即可
            ops->diagnose_rule(blackboard, payload, &ctx);
        }
    } else if (scope == NVS_CHK_SCOPE_PROG) {
        nvs_prog_mgr_ops_t* prog_ops = get_prog_mgr_ops();
        if (prog_ops && prog_ops->diagnose_topology) {
            cJSON* prog_cfg = get_prog_cfg_by_id(target_id);
            prog_ops->diagnose_topology(prog_cfg, &ctx);
        }
    }

    // 3. 返回实际产出的错误数量
    return ctx.current_count;
}
```

**工程扩展性总结：**
采用 `nvs_chk_ctx_t` 上下文机制后，Node 层和 Prog 层的开发者在编写校验逻辑时，完全不需要关心错误数组的内存越界问题和最终报文的打包过程。只需要专心写 `if (xxx)`，命中条件则调用 `nvs_chk_append_error`。这使得系统的校验规则能够随着业务的增长而实现低成本的高速扩充。