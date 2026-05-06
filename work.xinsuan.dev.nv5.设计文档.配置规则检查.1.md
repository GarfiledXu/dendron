---
id: zdlhbiammrgio7rj3eo0zq9
title: '1'
desc: ''
updated: 1778032107734
created: 1778032105739
---

# 智能相机 Pre-Rule Check 模块双方案与技术规格说明书 (V1.0)

## 第一部分：双方案架构说明

本说明针对智能相机下位机（包含 NCM、NDM、Prog、Node）与上位机的交互架构，提供隐式拦截（下位机控制）与显式触发（上位机控制）两种实现方案的技术对比。

### 方案一：下位机隐式拦截架构 (Implicit Interception)

#### 1. 核心机制
保持现有上位机与下位机的通讯指令集（如 `CMD_SETPARAM`, `CMD_TRIGGER`）不变。在下位机 NDM 模块解析指令后、实际写入黑板内存或触发 Prog 执行前，插入预检中间层。校验与执行在下位机内部合并为一次请求处理。

#### 2. 执行链路
1. **指令接收：** 上位机发送 `CMD_SETPARAM` (携带目标节点 ID 及参数 JSON)。
2. **预检触发：** NDM 接收指令，提取参数 JSON，调用预检层接口 `nvs_pre_check_param(...)`。
3. **节点校验：** 预检层根据节点 ID 查找对应 Node 的验证回调，返回校验结果。
4. **决策分流：**
   * **校验通过 (PASS)：** NDM 将参数写入节点对应的黑板区，并向 NCM 返回标准 `SUCCESS_ACK`。
   * **校验失败 (FAIL)：** NDM 中断执行（不写入黑板），将带有错误码和具体错误路径 (`err_path`) 的结果封装入 `ERROR_ACK`，通过 NCM 返回给上位机。

#### 3. 优缺点概括
* **优点：** 闭环安全性高；防止绕过校验直接下发异常参数；通讯效率高（单次 RTT）。
* **缺点：** 增加了 NDM 核心模块的耦合度。

### 方案二：上位机显式触发架构 (Explicit Trigger API)

#### 1. 核心机制
新增专用的校验通讯指令（如 `CMD_PRE_CHECK`）。上位机在执行任何设定修改或运行触发前，主动调用校验指令获取判定。

#### 2. 执行链路
1. **校验请求：** 上位机发送新增指令 `CMD_PRE_CHECK`。NDM 接收后转交预检层处理，并将结果返回上位机。下位机状态不发生改变。
2. **执行请求：** 上位机收到 `PASS` 后，发送实际的 `CMD_SETPARAM` 或 `CMD_TRIGGER`。NDM 接收到执行指令直接写入黑板或调度 Prog 执行。

#### 3. 优缺点概括
* **优点：** 下位机架构解耦；符合单一职责原则。
* **缺点：** 存在并发状态竞争风险（TOCTOU）；增加一次网络通讯开销；上位机状态机重构成本高。

---

## 第二部分：模块技术规格说明（基于隐式拦截方案）

### 1. 核心数据结构定义

```c
// 校验结果严重等级
typedef enum {
    NVS_CHK_PASS  = 0,  // 校验通过，允许写入或执行
    NVS_CHK_WARN  = 1,  // 警告，允许写入或执行，但存在潜在风险
    NVS_CHK_FATAL = 2   // 致命错误，拒绝写入黑板，拒绝执行指令
} nvs_check_level_e;

// 校验触发动作类型枚举
typedef enum {
    NVS_ACTION_TRIGGER_READ = 1,
    NVS_ACTION_LEARN        = 2,
    NVS_ACTION_ADD_SAMPLE   = 3
} nvs_check_action_e;

// 标准校验结果透传结构体
typedef struct {
    nvs_check_level_e level;      // 严重等级
    int err_code;                 // 全局统一分配的错误码 (如 ERR_OUT_OF_RANGE)
    char err_path[128];           // 失效字段的具体点号路径 (如 "programs[0].nodes[2].threshold")
    char err_msg[128];            // 透传描述信息
} nvs_check_res_t;
```

### 2. 协议与交互指令扩展说明

基于隐式拦截机制，上位机与 NCM 之间的原有指令（CMD）保持不变，仅在下位机返回的应答包（ACK）协议中扩展错误信息的 JSON 载荷定义。

#### 2.1 参数下发协议 (`setparam`)

**上位机请求 (Request):**
```json
{
  "cmd": "setparam",
  "target_id": "ocr_node_01",
  "params": {
    "threshold": 300,
    "mode": 1
  }
}
```

**下位机应答 (ACK) - 校验失败 (拦截):**
NDM 将 `nvs_check_res_t` 结构体序列化后，固定挂载于 `_check_error` 字段。
```json
{
  "cmd": "setparam_ack",
  "status": -1,
  "msg": "pre-rule check failed",
  "_check_error": {
    "level": 2,
    "err_code": 1004,
    "err_path": "params.threshold",
    "err_msg": "Value 300 exceeds maximum capability limit 255."
  }
}
```

#### 2.2 动作触发协议 (`trigger read` / `learn`)

**下位机应答 (ACK) - 状态不满足 (拦截):**
```json
{
  "cmd": "trigger_read_ack",
  "status": -1,
  "msg": "pre-rule check failed",
  "_check_error": {
    "level": 2,
    "err_code": 2001,
    "err_path": "nodes[1].match_tool",
    "err_msg": "Tool requires training before execution."
  }
}
```

### 3. 核心 API 接口定义

#### 3.1 引擎对外接口 (供 NDM 调用)

```c
/**
 * @brief 校验写入参数的合法性 (拦截 SetParam)
 * @param target_node_id 目标节点 ID
 * @param param_json     即将写入的参数 JSON (cJSON指针)
 * @param out_res        输出的校验结果
 * @return 0: 允许写入黑板, -1: 拦截写入
 */
int nvs_pre_rule_check_param(const char* target_node_id, cJSON* param_json, nvs_check_res_t* out_res);

/**
 * @brief 校验当前状态是否满足动作前提条件 (拦截 Trigger/Learn)
 * @param target_id      Prog ID 或 Node ID
 * @param action_type    动作枚举值 (nvs_check_action_e)
 * @param out_res        输出的校验结果
 * @return 0: 允许拉起底层算法执行, -1: 拦截执行
 */
int nvs_pre_rule_check_action(const char* target_id, nvs_check_action_e action_type, nvs_check_res_t* out_res);
```

#### 3.2 底层工具回调接口 (供 Node 维护者填充)

在现有的节点工厂操作集 `nvs_node_ops_t` 结构体中新增两项预检回调函数指针。

```c
typedef struct {
    const char* tool_type;
    
    // 1. 参数静态校验：比对传入的 JSON 节点与静态限制或系统能力集
    int (*validate_param)(cJSON* new_param, nvs_check_res_t* out_res);
    
    // 2. 运行时状态校验：比对请求动作与当前工具在黑板/内存中的就绪状态
    int (*validate_action)(nvs_check_action_e action_type, nvs_check_res_t* out_res);
    
    // [现有接口] init, execute, deinit...
} nvs_node_ops_t;
```

### 4. 执行时序流转总结

1. **写参数控制流 (`setparam`)**：
   NCM 接收数据 -> 传递至 NDM -> NDM 调用 `nvs_pre_rule_check_param` -> 引擎路由至目标 Node 的 `validate_param` 回调 -> 检查静态规格 -> 返回 PASS -> NDM 将数据并入黑板内存。
2. **动作控制流 (`trigger` / `learn`)**：
   NCM 接收指令 -> 传递至 NDM -> NDM 调用 `nvs_pre_rule_check_action` -> 引擎向下级联调用受影响 Node 的 `validate_action` 回调 -> 检查运行状态机 -> 返回 PASS -> NDM 调度 Prog 拉起底层算法执行。