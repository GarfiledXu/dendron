---
id: 5f7edlfecf6x0wyygr4oijg
title: '12'
desc: ''
updated: 1778134996892
created: 1778125794369
---

# 规则检查 (CRC) 模块设计文档

**文档编号**：NVS-ARCH-042  
**模块名称**：`nvs_pre_rule_check`  
**架构形态**：主动查询式预检机制

## 一、 模块概述与设计原则

### 1.1 功能定义

作为**下位机**的统一规则验证服务，本模块负责接收来自**上位机**的预检请求。在实际执行配置保存、运行触发、资源申请等动作之前，对目标对象进行合法性评估与运行条件验证。

根据请求中是否携带配置负载（Payload），分为两类校验场景：

#### 1）运行状态检查（不携带配置负载）

当请求负载为空（`"payload_type": ""` 且 `"payload": {}`）时，表示仅对目标对象**当前真实运行状态**进行评估。

* **检查依据**：当前设备资源环境、内部状态机、程序拓扑关系、Tool 运行状态、系统实时能力。
* **典型场景**：Flash/内存是否充足、Tool 是否已学习、Camera 是否 Ready、程序拓扑是否完整、当前状态是否允许执行。
* **问题定性**：此类通常属于**状态级、拓扑级或资源级错误**。
* **返回特征**：`err_param_path` 通常为空，因为错误并不对应某个具体的参数字段。

#### 2）配置负载检查（携带配置负载）

当请求中携带具体的 `"payload_type"` 和 `"payload"` 时，下位机会在上述“运行状态检查”的基础上，**额外对透传配置内容进行合法性验证**。

* **检查内容**：参数范围合法性、枚举值有效性、当前硬件是否支持、配置组合是否冲突、是否超出能力边界等。
* **问题定性**：本质属于**“配置是否合法”**的参数级错误。
* **返回特征**：返回具体的 `err_param_path`（如 `"threshold"`），用于精准指出问题字段。上位机借此可实现具体交互

### 1.2 价值体现

* **解释权转移**：将规则校验逻辑从上位机转移至下位机内部。实现“规则解释与固件绑定”，避免固件升级后规则漂移、多平台规则不一致或参数边界硬编码等问题
* **系统资源校验**：避免“逻辑允许但物理资源不足”导致的崩溃
* **减轻上位机的复杂性**：尤其在上位机和下位机一对多的场景下，减轻固件差异带来的交互和硬编码成本

### 1.3 设计原则

1. **避免副作用**：预检过程仅进行规则评估，绝对不修改内部数据、不保存配置、不触发执行、不改变状态机。
2. **职责划分**：避免跨层越权判断。
   * `SYSTEM` 层仅维护系统资源与能力规则；
   * `PROG` 层仅维护程序拓扑与工具关系；
   * `NODE` 层仅维护 Tool 内部参数与状态。
3. **解释权**：真正有效的规则以下位机返回结果为准
4. **交互模式优化**：
   * *传统模式（松散）*：上位机查询具体信息，进行校验判断，认为参数合法 $\rightarrow$ 直接下发 $\rightarrow$ 下位机执行失败
   * *当前模式（绑定）*：上位机请求检查 $\rightarrow$ 下位机基于真实环境评估并返回精准错误 $\rightarrow$ 上位机根据结果决策下轮行动

---

## 二、 角色定义与执行链路

### 2.1 角色定义

* **上位机 (Upper Machine)：**
  * **前端控制端：** 触发方。负责在执行特定交互前发起查询请求，并根据下位机透传的结果进行控件提示或全局警告。
* **下位机 (Lower Machine)：**
  * **NCM (通信模块)：** 负责接收网络数据，拆解定界符并提取指令与 JSON 负载，向下分发。
  * **NDM (设备管理模块)：** 接收业务指令，作为下位机的总控，调用预检引擎的统一入口。
  * **预检调度器 (Pre-check Dispatcher)：** 检查流程的统筹方。负责初始化错误收集上下文，解析请求层级，并寻址路由到对应的具体回调函数。
  * **Prog/Node 实体 (程序与节点)：** 实际的校验规则执行方。持有当前状态数据与内部限制条件，通过注册的 C 函数接口执行比对。

### 2.2 执行链路

1. 上位机下发 `Pre_Rule_Check` 查询请求，指定 `Scope`、`Target ID`、`Payload Type` 及 `Payload`。
2. 下位机 NDM 调用 `nvs_pre_rule_check_run` 入口函数。
3. Dispatcher 创建上下文并路由到对应的目标对象。
4. 底层模块结合当前真实环境、运行状态、透传配置负载以及全局能力集，进行规则评估。
5. 将检测到的异常项压入上下文。
6. Dispatcher 汇总错误列表并返回 ACK。

---

## 三、 检查层级与严重程度

### 3.1 检查目标层级 (Scope)

| Scope | 枚举值 | 含义 |
| :--- | :---: | :--- |
| **SYSTEM** | 0 | 系统级资源与能力验证 |
| **PROG** | 1 | 程序拓扑与依赖关系验证 |
| **NODE** | 2 | Tool 内部参数合法性与状态验证 |

### 3.2 严重等级 (Level)

| Level | 枚举值 | 含义 |
| :--- | :---: | :--- |
| **WARN** | 1 | 风险提示，不阻断执行流程 |
| **FATAL** | 2 | 致命错误，强制拦截并阻断执行 |

---

## 四、 上下位机交互报文定义

### 4.1 请求字段 (Request Payload)

所有字段必须始终存在。无配置负载时，传入空字符串和空对象。

| 字段 | 类型 | 说明 |
| :--- | :--- | :--- |
| `scope` | int | 检查层级（0: SYSTEM, 1: PROG, 2: NODE） |
| `target_id` | string | 目标实体 ID |
| `payload_type`| string | 配置结构类型（无负载时传 `""`） |
| `payload` | object | 待校验的具体配置负载（无负载时传 `{}`） |

### 4.2 应答字段 (ACK Payload)

| 字段 | 说明 |
| :--- | :--- |
| `scope` | 请求层级回显 |
| `target_id` | 请求目标回显 |
| `errors` | 错误项列表（Array）。若通过检查，则列表为空。 |

**`errors` 列表项结构：**

| 字段 | 说明 |
| :--- | :--- |
| `level` | 严重等级 (1: WARN, 2: FATAL) |
| `err_code` | 全局错误码 |
| `err_target_id` | 实际报错的实体 ID |
| `err_param_path`| 指明具体的参数路径。 |
| `err_msg` | 默认英文错误描述（供兜底展示或调试） |

> **关于 `err_param_path` 的核心意义**：
>
> * **非空（如 `"threshold"`）**：代表**参数级错误**。具体的使用就看上位机，是否与ui控件关联进行反馈
> * **为空（`""`）**：则表示错误不具体到对应参数

### 4.3 负载数据示例（以 OCR Tool 参数预校验为例）

**请求 (Request)：**

```json
{
  "scope": 2,
  "target_id": "ocr_node_01",
  "payload_type": "AdvancedConfig",
  "payload": {
    "mode": "adaptive",
    "algorithm": {
      "filter_size": 3,
      "binarization": {
        "enable": true,
        "threshold": 300  // 【错误1】不合法的深层参数
      }
    },
    "rois": [
      { "id": 1, "x": 0, "y": 0, "width": 5000, "height": 100 }, // 【错误2】数组内元素的参数越界
      { "id": 2, "x": 50, "y": 50, "width": 200, "height": 50 }
    ]
  }
}
```

**应答 (ACK)：**

```json
{
  "scope": 2,
  "target_id": "ocr_node_01",
  "errors": [
    {
      "level": 2,
      "err_code": 1004,
      "err_target_id": "ocr_node_01",
      "err_param_path": "algorithm.binarization.threshold", 
      "err_msg": "Threshold exceeds max limit 255"
    },
    {
      "level": 2,
      "err_code": 1012,
      "err_target_id": "ocr_node_01",
      "err_param_path": "rois[0].width", 
      "err_msg": "ROI width exceeds max camera resolution (4096)"
    }
  ]
}
```

---

## 五、 下位机内部实现与接口规范

### 5.1 数据结构

```c
// 预检异常数据传输对象
typedef struct {
    nvs_check_level_e    level;
    int                  err_code;
    char                 err_target_id[64];
    char                 err_param_path[64];
    char                 err_msg[128];
} nvs_check_res_t;

// 上下文管理器
typedef struct {
    nvs_check_res_t* results;
    int max_capacity;
    int current_count;
} nvs_chk_ctx_t;
```

### 5.2 核心 API

```c
/**
 * @brief 执行规则预检主入口
 */
int nvs_pre_rule_check_run(nvs_check_scope_e scope, 
                            const char* target_id, 
                            const char* payload_type, 
                            cJSON* payload, 
                            nvs_check_res_t* out_array, 
                            int max_capacity);

/**
 * @brief 错误项压栈辅助函数
 */
void nvs_chk_append_error(nvs_chk_ctx_t* ctx,
                          nvs_check_level_e level,
                          int code,
                          const char* target_id,
                          const char* param_path,
                          const char* msg);
```

模块回调接口定义

```c++
typedef struct {
    // ... 原有回调 (如 load, save) ...
    
    /**
     * @brief 程序级规则检查回调
     * @param prog_cfg     当前程序的内存配置快照
     * @param payload_type 对应请求中的负载类型
     * @param payload      对应请求中的 JSON 负载 (轨道B时为 NULL)
     */
    void (*check_rule)(cJSON* prog_cfg, const char* payload_type, cJSON* payload, nvs_chk_ctx_t* ctx);
} nvs_prog_ops_t;
```

```c++
typedef struct {
    const char* tool_type;
    // ... 原有回调 (如 process, set_param) ...

    /**
     * @brief 工具级规则检查回调
     * @param blackboard   当前工具在黑板中的私有数据
     * @param payload_type 对应请求中的负载类型
     * @param payload      对应请求中的 JSON 负载 (轨道B时为 NULL)
     */
    void (*check_rule)(cJSON* priv_data, const char* payload_type, cJSON* payload, nvs_chk_ctx_t* ctx);
} nvs_node_ops_t;
```

---

## 六、 具体场景与规则填充举例

### 6.1 场景一：系统资源与硬件配额检查 (SYSTEM 层)

**核心思想**：联动底层能力集获取物理限制（如样本存储空间），在操作前预判资源溢出。

```c
/**
 * System 级预检逻辑：检查系统资源是否满足承载当前程序/配置的需求
 */
void nvs_system_check_rule_impl(nvs_chk_ctx_t* ctx, cJSON* payload) {
    // 1. 获取物理资源实时值
    int free_flash_kb = system_get_flash_free_space();
    
    // 2. 联动能力集计算预期占用空间 (如：单工具最大样本数 20 * 样本单位大小)
    int max_samples = nvs_capability_get_int("atool.max_samples_per_tool"); 
    int estimated_size_kb = (max_samples * SAMPLE_UNIT_SIZE) / 1024;

    if (free_flash_kb < estimated_size_kb) {
        // 全局资源错误，err_param_path 为空
        nvs_chk_append_error(ctx, NVS_CHK_FATAL, 4010, "system", "", "Physical Flash space insufficient for new samples.");
    }

    // 3. 校验程序版本兼容性 (联动能力集 system.min_host_version)
    const char* min_version = nvs_capability_get_string("system.min_host_version");
    if (is_version_too_low(payload, min_version)) {
        nvs_chk_append_error(ctx, NVS_CHK_FATAL, 4012, "system", "", "Program version is incompatible with current firmware.");
    }
}
```

### 6.2 场景二：程序级配额与 Node 递归调度 (PROG 层)

**核心思想**：由 `nvs_prog_t` 的管理逻辑执行容器校验（如工具总数），并通过 `nvs_node_call_ops_xxx` 接口递归触发所属工具的校验。

```c
/**
 * Prog 级预检逻辑：执行程序级限制校验并下发 Node 校验指令
 */
int32_t nvs_prog_param_check_cb(nvs_prog_t* prog, const char* payload_type, cJSON* payload, nvs_chk_ctx_t* ctx) {
    
    // 1. 校验工具总数是否超出能力集限制 (atool.max_tools: 64)
    int max_tools = nvs_capability_get_int("atool.max_tools");
    if (nvs_pm_get_num_nodes(prog) > max_tools) {
        nvs_chk_append_error(ctx, NVS_CHK_FATAL, 3001, prog->prog_id_str, "", "Exceeds maximum supported tool count (64).");
    }

    // 2. 递归调度：通过 Ops 接口触发所属 Node 的内部校验
    // 遍历 atool_nodes 列表
    nvs_node_t* node = NULL;
    nvs_node_mng_list_foreach(node, &prog->atool_nodes) {
        // 调用 Node 封装层接口，最终进入各工具实现的 check_cfg_rule
        nvs_node_call_ops_check_cfg_rule(node, payload_type, payload, ctx);
    }
    
    return 0;
}
```

### 6.3 场景三：工具测试运行前的校验 (NODE 层)
>
> 运行前检查 Tool 内部状态机，例如模板是否学习、模型是否加载。

```c
//实现node的rule_check回调
static void match_tool_check_rule(cJSON* blackboard, const char* payload_type, cJSON* payload, nvs_chk_ctx_t* ctx) {
    if (payload == NULL) {
        if (tool_get_internal_status(blackboard) != STATUS_LEARNED) {
            nvs_chk_append_error(
                ctx, NVS_CHK_WARN, 2001, "match_node_02", "",
                "Tool is currently untrained. Execution may fail."
            );
        }
        return;
    }
}
```

### 6.4 场景四：工具的配置校验 (NODE 层)
>
> 用户修改参数尚未保存时，上位机实时透传负载，固件动态校验是否超出硬件极限或枚举是否合法。

```c
static void ocr_tool_check_rule(cJSON* blackboard, const char* payload_type, cJSON* payload, nvs_chk_ctx_t* ctx) {
    if (payload != NULL && payload_type != NULL) {
        
        if (strcmp(payload_type, "ParamConfig") == 0) {
            cJSON* threshold = cJSON_GetObjectItem(payload, "threshold");
            if (threshold) {
                int max_limit = 0;
                nvs_cap_get_int("ocr.threshold_max", &max_limit);
                if (threshold->valueint > max_limit) {
                    nvs_chk_append_error(
                        ctx, NVS_CHK_FATAL, 1004, "ocr_node_01", "threshold",
                        "Threshold parameter exceeds hardware limits."
                    );
                }
            }
        } 
        else if (strcmp(payload_type, "AdvancedConfig") == 0) {
            cJSON* mode = cJSON_GetObjectItem(payload, "mode");
            if (mode && !is_valid_enum(mode->valueint)) {
                nvs_chk_append_error(
                    ctx, NVS_CHK_FATAL, 1005, "ocr_node_01", "mode",
                    "Invalid enumeration value for running mode."
                );
            }
        }
    }
}
```

---

