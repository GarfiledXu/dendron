---
id: le319kiozk7hktti7ezztfe
title: '9'
desc: ''
updated: 1778124532037
created: 1778124457814
---

# 规则检查(CRC) 模块设计文档

**文档编号**：NVS-ARCH-042  
**模块名称**：`nvs_pre_rule_check`  
**架构形态**：主动查询式预检机制

## 一、 模块概述和设计原则

### 1.1 功能定义

作为 **下位机** 的规则验证服务，负责接收来自 **上位机** 的查询请求，在实际执行设置或触发运行之前，先行对配置的合法性、系统资源的充裕度以及运行态前置条件进行计算评估。支持两种校验模式：
1. **运行态校验 (Status Check)**：基于当前设备真实环境、黑板数据及拓扑状态，验证是否满足执行条件。
2. **配置预校验 (Dry-Run)**：在不修改系统实际参数的前提下，通过透传拟下发的配置负载，先行评估其合法性。

### 1.2 价值体现

1. **校验逻辑集中维护**：应对固件差异，校验规则的划分与复杂判定收敛于下位机，上位机仅负责数据透传与结果显示，解决因硬编码导致的规则脱节问题。
2. **系统资源前置校验**：在执行保存文件、追加样本等动作前，预先比对真实的存储空间或内存剩余量，防止中途因资源耗尽引发写入失败。
3. **拓扑与依赖关系验证**：在触发运行前，自动完成工具间数据流闭环、依赖状态就绪（如模型是否已学习）等逻辑校验。
4. **动态配置预校验 (防呆)**：支持上位机在界面设定阶段透传参数负载。下位机返回精准的字段路径（`err_param_path`），上位机据此实现表单级的即时校验，无需自行维护繁杂的硬件能力边界。

### 1.3 核心设计原则

1. **无副作用**：不修改下位机内部数据，不触发执行动作。
2. **职责划分**：各层级实体（SYSTEM、PROG、NODE）负责维护和校验与其自身直接相关的配置项与状态，互不越权。

---

## 二、 角色定义和执行链路

### 2.1 角色定义

* **预检调度器 (Pre-check Dispatcher)**：分拣员。负责解析协议外层，根据目标寻址并透传负载，不感知具体的业务参数内容。
* **Prog/Node 实体 (程序与节点)**：收件人。实际的规则执行方，持有状态数据，复用现有的 JSON 解析能力对透传负载进行校验。

### 2.2 执行链路

1. 上位机下发 `Pre_Rule_Check` 业务查询指令，包含层级、目标、**负载类型**及**配置负载**。
2. 下位机 NDM 唤起 `nvs_pre_rule_check_run` 接口。
3. 预检调度器创建上下文，根据目标标识寻址到对应的具体回调函数。
4. 底层模块结合**传入的配置负载（若有）**、当前设备环境及全局能力集运行比对，将异常项压入上下文。
5. 预检调度器收集所有错误项，组装为包含请求回显的业务响应（ACK）返回。

---

## 三、 检查层级与严重程度

### 3.1 检查目标层级 (Scope)

* **SYSTEM (0)**：系统级。评估硬件能力、存储空间、授权基线等。
* **PROG (1)**：程序级。评估程序拓扑关系、工具配额、数据流连通性。
* **NODE (2)**：节点级。评估单一工具内部参数范围及独立状态机（如学习状态）。

### 3.2 严重等级 (Level)

* **WARN (1)**：警告。检测到风险但不阻断动作，上位机可选择性提示。
* **FATAL (2)**：致命。检测到严重违规或资源缺失，数据不可用，下位机拒绝执行并强制拦截。

---

## 四、 上下位机交互负载定义

为保证下位机 JSON 解析的稳定性，所有请求字段**必须始终存在**（不需要时传空字符串或空对象）。

### 4.1 请求字段 (Request Payload)

* `scope`: 整型 (0: System, 1: Prog, 2: Node)。
* `target_id`: 字符串。请求预检的目标 ID（系统级传 `""`）。
* `payload_type`: 字符串。指定 `payload` 的业务结构名（如 `"ParamConfig"`）。纯查状态时传 `""`。
* `payload`: JSON 对象。待校验的参数负载。纯查状态时传 `{}`。

### 4.2 应答字段 (ACK Payload)

* `scope`: 整型。回显请求层级，用于异步匹配。
* `target_id`: 字符串。回显请求目标 ID。
* `errors`: JSON 数组。若通过则返回空数组 `[]`。
    * `level`: 整型 (1: WARN, 2: FATAL)。
    * `err_code`: 整型。全局错误码，用于上位机多语言（i18n）映射。
    * `err_target_id`: 字符串。精准定位报错的底层实体 ID。
    * `err_param_path`: 字符串。**UI 路由核心字段**。
        * **非空**：代表表单参数错，前端据此标红对应的输入框。
        * **为空**：代表状态或拓扑错，前端据此高亮整个节点或弹出全局警告。
    * `err_msg`: 字符串。默认英文描述，作为多语言包缺失时的兜底显示。

### 4.3 负载数据示例

**场景：对 OCR 工具进行配置预校验（轨道 A）**
```json
// 请求
{
  "scope": 2, "target_id": "ocr_node_01", 
  "payload_type": "ParamConfig", "payload": {"threshold": 300}
}
// 应答 (参数越界)
{
  "scope": 2, "target_id": "ocr_node_01",
  "errors": [
    {
      "level": 2, "err_code": 1004, "err_target_id": "ocr_node_01", 
      "err_param_path": "threshold", "err_msg": "Exceeds max limit 255"
    }
  ]
}
```

---

## 五、 下位机内部实现与接口规范

### 5.1 核心数据结构

```c
// 预检异常数据传输对象
typedef struct {
    nvs_check_level_e    level;      
    int                  err_code;                 
    char                 err_target_id[64];  // 定位报错实体
    char                 err_param_path[64]; // 定位参数字段 (若为空则代表状态级错误)
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
int nvs_pre_rule_check_run(nvs_check_scope_e scope, const char* target_id, 
                           const char* payload_type, cJSON* payload, 
                           nvs_check_res_t* out_array, int max_capacity);

/**
 * @brief 错误项压栈辅助函数
 */
void nvs_chk_append_error(nvs_chk_ctx_t* ctx, nvs_check_level_e level, int code, 
                          const char* target_id, const char* param_path, const char* msg);
```

---

## 六、 具体场景与规则填充举例

本节结合系统的核心业务场景，详细说明在“双轨模式”（配置预校验 vs 运行态校验）下，各层级是如何通过底层的 C 代码落实规则拦截的。

### 6.1 场景一：系统资源与限额交叉检查 (SYSTEM 层)
**场景描述：** 在执行需要消耗存储的动作（如保存主控图、导出配置）前，评估物理资源。即便逻辑配额未超，物理环境不足时依然要产生致命拦截。

```c
// 运行态物理环境可用性检查
int required_space_kb = calculate_estimated_size(new_request_count);
int free_space_kb = system_get_flash_free_space();

if (free_space_kb < required_space_kb) {
    // 纯状态错误，无具体字段，param_path 传 ""
    // target_id 明确追责给 "system"
    nvs_chk_append_error(ctx, NVS_CHK_FATAL, 4010, "system", "", 
                         "Insufficient physical Flash space for this operation.");
}

6.2 场景二：程序拓扑与可执行状态验证 (PROG 层)
场景描述： 在触发相机运行前（轨道 B：纯查状态），验证整个程序画布内的数据流转与拓扑连线是否合规闭环。此时 payload_type 必为空。

// 预检程序拓扑：检查所有的分析工具，是否存在未连线的悬空输入
cJSON* nodes = cJSON_GetObjectItem(prog_cfg, "nodes");
cJSON* node = NULL;
cJSON_ArrayForEach(node, nodes) {
    if (strcmp(cJSON_GetStringValue(cJSON_GetObjectItem(node, "category")), "analysis") == 0) {
        cJSON* inputs = cJSON_GetObjectItem(node, "inputs");
        if (!cJSON_HasObjectItem(inputs, "image_source")) {
             // 提取出具体违规的子节点 ID
             const char* bad_node_id = cJSON_GetStringValue(cJSON_GetObjectItem(node, "id"));
             
             // 精准指认是哪个节点出了问题，让前端去高亮该节点
             nvs_chk_append_error(ctx, NVS_CHK_FATAL, 3010, bad_node_id, "", 
                                  "Program not executable: Mandatory input connection is floating.");
        }
    }
}

6.3 场景三：工具内部运行状态评估 (NODE 层 - 轨道 B)
场景描述： 同样在触发运行前，深入到具体的工具内部，检查其内部状态机是否满足运行前置条件（如模板匹配工具是否已经学习了基准图像）。

static void match_tool_check_rule(cJSON* blackboard, const char* payload_type, cJSON* payload, nvs_chk_ctx_t* ctx) {
    // 如果没有传入 payload，说明当前处于运行态前置状态检查
    if (payload == NULL) {
        // 检查黑板数据中的内部状态标志位
        if (tool_get_internal_status(blackboard) != STATUS_LEARNED) {
            // 这只是个警告，不阻断运行，但在前端提示用户该工具可能会返回 NG
            nvs_chk_append_error(ctx, NVS_CHK_WARN, 2001, "match_node_02", "", 
                                 "Tool is currently untrained. Execution may fail.");
        }
        return; 
    }
    // ... 其他逻辑 ...
}


6.4 场景四：动态配置预校验与防呆 (NODE 层 - 轨道 A)
场景描述： 用户在前端面板修改了 OCR 工具的参数（但还未点击保存）。上位机透传测试负载，底层根据 payload_type 找到对应的解析分支，结合硬件能力集进行合法性校验，并返回具体的 err_param_path 供前端标红表单。

static void ocr_tool_check_rule(cJSON* blackboard, const char* payload_type, cJSON* payload, nvs_chk_ctx_t* ctx) {
    // 必须有负载且类型匹配，才执行配置预校验
    if (payload != NULL && payload_type != NULL) {
        
        // 分支 1：上位机正在预校验常规参数面板
        if (strcmp(payload_type, "ParamConfig") == 0) {
            cJSON* threshold = cJSON_GetObjectItem(payload, "threshold");
            if (threshold) {
                int max_limit = 0;
                nvs_cap_get_int("ocr.threshold_max", &max_limit); // 从能力集获取当前固件上限
                if (threshold->valueint > max_limit) {
                    // 指出具体的 JSON 字段路径，前端收到后直接标红 "threshold" 输入框
                    nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1004, "ocr_node_01", "threshold", 
                                         "Threshold parameter exceeds hardware limits.");
                }
            }
        }
        
        // 分支 2：上位机正在预校验高级算法设定
        else if (strcmp(payload_type, "AdvancedConfig") == 0) {
            cJSON* mode = cJSON_GetObjectItem(payload, "mode");
            if (mode && !is_valid_enum(mode->valueint)) {
                nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1005, "ocr_node_01", "mode", 
                                     "Invalid enumeration value for running mode.");
            }
        }
    }
}



