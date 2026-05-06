---
id: r4mx6st917lvqdup8zf8mib
title: '7'
desc: ''
updated: 1778068406972
created: 1778062630535
---

# 智能相机 Pre-Rule Check 模块设计与技术规格说明书 (V6.0)

**文档编号**：NVS-ARCH-042
**模块名称**：`nvs_pre_rule_check`
**架构形态**：主动查询式独立预检机制

---

## 一、 模块概述和设计原则

### 1.1 功能定义

本模块定位于**下位机**的独立规则验证服务。它负责接收来自**上位机**的查询请求，在下位机实际执行设置或触发运行之前，先行对配置的合法性、系统资源的充裕度以及运行态前置条件进行计算评估，并向上位机返回预定义结构化的数据

### 1.2 价值体现

本机制的核心在于将检查逻辑实现在下位机，并通过事前查询拦截无效动作。具体体现在以下三个方面及有无此机制的差异：

1. **校验规则实现在下位机**
   * **机制特点：** 具体的参数限制、范围比对代码均写在下位机，上位机只负责发送测试数据并显示返回的错误文本。
   * **无此机制：** 上位机软件中需要硬编码相机的各项物理限制和规则。当下位机固件升级、能力集变更或接入不同型号相机时，上位机的规则极易与设备实际情况脱节。
   * **有此机制：** 校验规则由下位机根据自身当前的固件和能力集直接计算，上位机无需维护校验规则，避免了版本不一致导致的规则错误。

2. **执行前检查系统资源**
   * **机制特点：** 在执行保存主控图、添加学习样本等消耗硬件资源的动作前，通过接口直接查询下位机当前的 Flash 剩余空间或内存状态。
   * **无此机制：** 上位机直接下发执行指令，若执行中途设备存储空间耗尽，会导致文件写入一半损坏或引发系统级异常。
   * **有此机制：** 在动作实际发生前对比资源需求与剩余量，若资源不足则直接拦截请求并报错。

3. **检查工具连线与依赖条件**
   * **机制特点：** 检查程序中各个工具的数据输入/输出端口是否已连接，以及必须的参数是否缺失。
   * **无此机制：** 上位机只能将包含断线或缺参数的配置直接保存给下位机，直到产线触发相机运行时，才会产生执行错误。
   * **有此机制：** 在上位机设定阶段即可发现未连接的数据流或缺失的依赖，阻止无效配置存入下位机。

### 1.3 核心设计原则

1. **状态只读：** 检查过程无副作用，不修改下位机内部数据和触发执行动作，仅获取检查结果
2. **职责划分边界：** 各层级实体（系统管理层、prog层、工具node节点层）仅负责维护和校验与其自身直接相关的配置项与状态数据，模块间互不越权

---

## 二、 角色定义和执行链路

### 2.1 角色定义

* **上位机 (Upper Machine)：**
  * **前端控制端：** 触发方。负责在用户点击保存或运行前主动发起诊断请求，并根据下位机返回的字段进行界面渲染。
* **下位机 (Lower Machine)：**
  * **NCM (通信模块)：** 负责接收网络数据，拆解定界符并提取指令与 JSON 负载，向下分发。
  * **NDM (设备管理模块)：** 接收业务指令，作为下位机的总控，负责调用预检引擎的统一入口。
  * **预检调度器 (Pre-check Dispatcher)：** 检查流程的统筹方。负责初始化错误收集上下文，解析请求层级，并寻址路由到对应的具体回调函数。
  * **Prog/Node 实体 (程序与节点)：** 实际的校验规则执行方。持有当前状态数据与内部限制条件，通过注册的 C 函数接口执行比对。

### 2.2 执行链路

1. 上位机下发 `Pre_Rule_Check` 业务查询指令。
2. 下位机 NCM 完成底层协议解析，将 JSON 负载移交 NDM。
3. NDM 唤起 `nvs_pre_rule_check_run` 接口，传入层级信息与测试载荷。
4. 预检调度器创建上下文，根据层级路由到 Prog 管理器或具体的 Node 工具注册的检查回调。
5. 底层模块结合测试载荷、黑板数据及全局能力集 (`nvs_capability`) 运行比对，将产生的错误项压入校验上下文。
6. 预检调度器收集所有错误项，交由 NDM 组装为标准业务响应（ACK）返回给上位机。

---

## 三、 检查目标和结果的层级划分

### 3.1 检查目标层级 (Scope)

* **SYSTEM (0)：** 系统级。评估设备全局硬件能力与软件授权基线。
* **PROG (1)：** 程序级。评估程序内部的拓扑关系、工具节点配额上限、组件数据流的连通性。
* **NODE (2)：** 节点级。评估单一工具内部参数的具体枚举范围、格式，以及独立状态机的就绪情况（如模板是否已学习）。

### 3.2 结果严重等级与动作定义 (Level)

* **PASS (0)：** 检查通过，数据合规，允许放行。
* **WARN (1)：** 警告。检测到逻辑风险，不强制阻断动作指令的下发，上位机可选择性提示用户。
* **FATAL (2)：** 致命。检测到严重越界或缺失核心前置参数。此状态意味着数据不可用，下位机拒绝并强制拦截后续的写入或执行动作。

---

## 四、 上下位机交互时序与负载定义

### 4.1 交互时序

上位机在提交配置保存或主动触发运行前发起预检请求，阻塞等待下位机返回 ACK 报文。确认返回结果中无 `FATAL` 级别错误后，方可继续下发真实的写入或运行指令。

### 4.2 业务负载 (Payload) 字段定义

基于系统已确立的 `指令-#JSON负载[CR]` 底层通信协议，本模块重点对 JSON Payload 的业务字段进行定义。

**请求字段 (Request Payload)：**

* `scope`: 整型 (0: System, 1: Prog, 2: Node)。
* `target_id`: 字符串。对应程序 ID 或节点 ID（检查 System 级时为空）。
* `payload`: JSON 对象。下位机基于此字段执行参数预检；若为空，则执行黑板就绪状态预检。

**应答字段 (ACK Payload)：**

* `status`: 整型 (0 表示计算成功，不代表规则通过)。
* `errors`: JSON 数组。包含所有 `level > 0` 的异常项。
  * `level`: 整型 (1: WARN, 2: FATAL)。
  * `err_code`: 整型，全局错误码。
  * `err_path`: 字符串，错误定位路径 (如 `payload.threshold`)。
  * `err_msg`: 字符串，描述文本。

### 4.3 负载数据示例

**上位机请求负载示例：**

```json
{"scope": 2, "target_id": "ocr_node_01", "payload": {"threshold": 300, "mode": 1}}
```

**下位机响应负载示例 (校验拦截)：**

```json
{
  "status": 0,
  "errors": [
    {"level": 2, "err_code": 1004, "err_path": "payload.threshold", "err_msg": "Exceeds limits"},
    {"level": 1, "err_code": 2001, "err_path": "blackboard.status", "err_msg": "Untrained"}
  ]
}
```

---

## 五、 下位机内部实现与接口规范

### 5.1 核心数据结构

```c
typedef enum {
    NVS_CHK_SCOPE_SYSTEM = 0, 
    NVS_CHK_SCOPE_PROG   = 1, 
    NVS_CHK_SCOPE_NODE   = 2  
} nvs_check_scope_e;

typedef enum {
    NVS_CHK_PASS  = 0,  
    NVS_CHK_WARN  = 1,  
    NVS_CHK_FATAL = 2   
} nvs_check_level_e;

// 预检异常数据传输对象 (DTO)
typedef struct {
    nvs_check_level_e level;      
    int err_code;                 
    char err_path[128];           
    char err_msg[128];            
} nvs_check_res_t;

// 预检收集上下文管理器 (避免数组越界，集中管控状态)
typedef struct {
    nvs_check_res_t* results;     
    int max_capacity;             
    int current_count;            
    int global_status;            
} nvs_chk_ctx_t;
```

### 5.2 接口规范

#### 5.2.1 供系统内部使用的调度 API (引擎入口与工具函数)

```c
/**
 * @brief 执行规则预检主入口 (供 NDM 接收网络请求后调用)
 */
int nvs_pre_rule_check_run(nvs_check_scope_e scope, const char* target_id, cJSON* payload, nvs_check_res_t* out_array, int max_capacity);

/**
 * @brief 错误项压栈辅助函数 (供各层级回调内部调用)
 */
void nvs_chk_append_error(nvs_chk_ctx_t* ctx, nvs_check_level_e level, int code, const char* path, const char* msg);
```

#### 5.2.2 回调实现函数的签名定义

系统现有的操作集 (`Ops`) 结构体需补充标准化检查接口，由对应的实体维护者实现。

```c
// Prog 级程序管理器操作集扩展
typedef struct {
    // ... 原有回调 ...
    void (*check_topology)(cJSON* prog_cfg, nvs_chk_ctx_t* ctx);
} nvs_prog_mgr_ops_t;

// Node 级工具实体操作集扩展
typedef struct {
    const char* tool_type;
    // ... 原有回调 ...
    void (*check_rule)(cJSON* blackboard_data, cJSON* test_payload, nvs_chk_ctx_t* ctx);
} nvs_node_ops_t;
```

---

## 六、 具体场景与规则填充举例

### 6.1 场景一：能力集静态联动校验

在 Node 层，当尝试设定某些物理参数时，下位机预检逻辑调取全局能力集进行边界比对。

```c
// 示例：校验曝光时间是否在能力集支持的范围内
cJSON* expo_item = cJSON_GetObjectItem(test_payload, "exposure_time");
if (expo_item) {
    int max_expo = 0;
    // 从能力集读取硬上限
    if (nvs_cap_get_int("vi.hframe.exposure_max", &max_expo) == NVS_CAP_OK) {
        if (expo_item->valueint > max_expo) {
            nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1004, "exposure_time", "Exceeds capability limit.");
        }
    } else {
        // 能力集缺省时的安全阻断
        nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1000, "exposure_time", "Capability definition missing.");
    }
}
```

### 6.2 场景二：设备运行态软资源评估

在 Prog 或 System 层，结合下发指令与设备实时状态进行容量预估。

```c
// 示例：检查新增工具后，总数是否超出系统允许的最大容量
int current_tool_count = prog_mgr_get_total_tools();
int added_tools_count = cJSON_GetArraySize(cJSON_GetObjectItem(test_payload, "new_nodes"));
int max_allowed_tools = 0;

nvs_cap_get_int("atool.max_tools", &max_allowed_tools);

if ((current_tool_count + added_tools_count) > max_allowed_tools) {
    nvs_chk_append_error(ctx, NVS_CHK_FATAL, 3001, "new_nodes", "Exceeds system maximum tool allowance.");
}
```

### 6.3 场景三：拓扑连线与业务逻辑合法性

在 Prog 层检查工具间的数据流向是否合理。

```c
// 示例：检查程序内所有分析类工具，是否均已正确绑定了输入的图像源
cJSON* nodes = cJSON_GetObjectItem(prog_cfg, "nodes");
cJSON* node = NULL;
cJSON_ArrayForEach(node, nodes) {
    if (strcmp(cJSON_GetStringValue(cJSON_GetObjectItem(node, "category")), "analysis") == 0) {
        cJSON* inputs = cJSON_GetObjectItem(node, "inputs");
        if (!cJSON_HasObjectItem(inputs, "image_source")) {
             char path_buffer[64];
             // 拼装精准路径供上位机高亮 UI
             snprintf(path_buffer, sizeof(path_buffer), "nodes[%d].inputs.image_source", get_node_index(node));
             nvs_chk_append_error(ctx, NVS_CHK_FATAL, 3010, path_buffer, "Mandatory input image connection is floating.");
        }
    }
}
```

### 6.4 场景四：算法核心句柄生成的强依赖拦截

在 Node 层强制检查必须的初始化字段，防御底层空指针崩溃。

```c
// 示例：检查决定内部算法逻辑分支的 'mode' 参数
cJSON* target_data = test_payload ? test_payload : blackboard_data;
cJSON* mode_item = cJSON_GetObjectItem(target_data, "mode");

if (mode_item == NULL) {
    nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1020, "mode", "Missing mandatory parameter 'mode' for internal handle generation.");
}
```
