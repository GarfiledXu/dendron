---
id: a81ijgp6t37wsfj0rvnftxc
title: '4'
desc: ''
updated: 1778060771552
created: 1778058397887
---

模块概述和设计原则

1. 功能定义
2. 价值体现
3. 核心设计原则

角色定义和执行链路

检查目标和结果的层级划分（概念定义）

上下位机交互时序和协议

1. 交互时序
2. 报文格式
3. 报文示例

下位机内部实现

1. 核心数据结构
2. 接口规范

具体场景举例

1. 能力集联动
2. 设备运行时状态
3. ...待补充

# 智能相机 Pre-Rule Check 模块设计与技术规格说明书 (V4.0)

**文档编号**：NVS-ARCH-042
**模块名称**：`nvs_pre_rule_check`
**架构形态**：主动查询式独立预检机制 (Active-Query Independent Pre-check)

## 1. 模块定位与架构总览

`nvs_pre_rule_check` 模块定位于下位机系统的独立状态预判与规则检查层。
本模块提供无副作用的“实体状态预检服务”。上位机通过专用的预检指令主动发起同步询问，下位机执行计算并以结构化数据返回校验结果。

**核心设计原则：**

* **状态隔离：** 检查过程仅执行只读计算，绝不修改下位机黑板 (Blackboard) 内存或触发底层算法运行。
* **层级路由：** 检查请求基于“层级作用域 (Scope)”严格路由至对应的实体（系统管理层、程序调度层、具体节点层）进行处理。
* **数据透传：** 检查结果采用精准的点号路径 (`err_path`) 透传，直接驱动上位机 UI 视图的状态更新与错误标红。

## 2. 核心数据结构定义

模块内部及上下位机交互的基础数据模型定义如下：

```c
// 1. 校验层级作用域 (Scope)
typedef enum {
    NVS_CHK_SCOPE_SYSTEM = 0, // 系统级：全局硬件/软件资源与能力基线限制
    NVS_CHK_SCOPE_PROG   = 1, // 程序级：程序内拓扑连线、组件容量配额
    NVS_CHK_SCOPE_NODE   = 2  // 节点级：单工具内部参数合法性及状态机就绪度
} nvs_check_scope_e;

// 2. 校验结果严重等级 (Level)
typedef enum {
    NVS_CHK_PASS  = 0,  // 通过
    NVS_CHK_WARN  = 1,  // 警告：状态存在潜在风险，但不绝对阻断执行逻辑
    NVS_CHK_FATAL = 2   // 致命：缺失核心前置条件或严重越界，若强行执行必致崩溃
} nvs_check_level_e;

// 3. 单项校验结果实体 (DTO)
typedef struct {
    nvs_check_level_e level;      
    int err_code;                 // 错误分类码 (参考全局错误码表)
    char err_path[128];           // 错误发生的 JSON 字段定位路径 (如 "nodes[0].mode")
    char err_msg[128];            // 面向开发者/调试的透传描述文本
} nvs_check_res_t;

// 4. 引擎集中化上下文管理器 (供底层回调使用，防止数组越界)
typedef struct {
    nvs_check_res_t* results;     // 外部传入的结果数组首地址
    int max_capacity;             // 数组最大容量
    int current_count;            // 当前已写入的错误项数量
    int global_status;            // 全局状态评估 (0: 允许放行, -1: 存在 FATAL 必须拦截)
} nvs_chk_ctx_t;
```

## 3. 交互通信协议规范

通信采用定界符格式：`指令-#JSON负载[CR]`。

**定界符规范：**

* 分隔符：`-#` (分割指令名与负载)
* 结束符：`[CR]` (标识单帧报文结束)

### 3.1 预检请求报文 (Request)

**报文格式：**
`Pre_Rule_Check-#{JSON_Payload}`

**Payload 字段定义：**

* `scope`: 整数 (0:System, 1:Prog, 2:Node)
* `target_id`: 字符串 (目标实体 ID，System 级传空字符串 `""`)
* `payload`: JSON 对象 (用于参数预检，若为状态检查传 `null` 或空字典 `{}`)

**报文示例 (节点级参数预检)：**

```text
Pre_Rule_Check-#{"scope":2,"target_id":"ocr_node_01","payload":{"threshold":300,"mode":1}}
```

**报文示例 (程序级状态预检)：**

```text
Pre_Rule_Check-#{"scope":1,"target_id":"prog_01","payload":{}}
```

### 3.2 预检应答报文 (ACK - 通信与处理成功)

只要下位机成功执行了预检计算，无论规则是否通过，均通过此格式返回。上位机根据 `errors` 数组长度和 `level` 决定界面的拦截与放行。

**报文格式：**
`Pre_Rule_Check-#{JSON_Payload}[CR]`

**报文示例 (检查全量通过，允许执行)：**

```text
Pre_Rule_Check-#{"status":0,"errors":[]}[CR]
```

**报文示例 (存在规则冲突或状态异常，需拦截)：**

```text
Pre_Rule_Check-#{"status":0,"errors":[{"level":2,"err_code":1004,"err_path":"threshold","err_msg":"Value exceeds limit"},{"level":1,"err_code":2001,"err_path":"status","err_msg":"Untrained"}]}[CR]
```

### 3.3 系统异常应答报文 (ER - 通信或解析失败)

当发生协议解析失败、内存不足或指令未注册等底层异常时，抛出标准的 ER 报文，不返回规则校验数据。

**报文格式：**
`ER-#[CommandName]-#[ErrorCode][CR]`

**报文示例：**

```text
ER-#Pre_Rule_Check-#33[CR]
```

## 4. 实体职责划分与校验链路

| 层级 (Scope) | 执行实体 | 检查职责描述 | 典型场景示例 |
| :--- | :--- | :--- | :--- |
| **SYSTEM (0)** | NDM 引擎核心 | 负责全系统的宏观软硬件防线。 | 1. 计算 Flash 余量是否能支撑新模板导入。<br>2. 校验请求的新工具类型是否存在于 `atool.supported_types`。 |
| **PROG (1)** | Prog 管理器 | 负责程序内部结构与拓扑完整性。 | 1. 检查节点连线是否悬空或存在死循环。<br>2. 检查程序内总工具数是否超越 `atool.max_tools`。 |
| **NODE (2)** | Node 工具回调 | 负责单工具的配置闭环与运行态前置条件。 | 1. 验证 OCR `mode` 等内部句柄生成强依赖参数。<br>2. 判断工具是否已完成训练状态。 |

## 5. 底层 API 与引擎实现规范

### 5.1 引擎对外调度接口

由 NDM 模块处理网络请求后调用。

```c
/**
 * @brief 执行规则预检引擎主入口
 * @param scope         检查层级 (System/Prog/Node)
 * @param target_id     目标实体 ID
 * @param payload       测试参数载荷 (状态检查传 NULL)
 * @param out_array     外部传入的内存数组，用于接收校验结果
 * @param max_capacity  数组容量
 * @return 错误项总数 (0 表示全部校验通过)
 */
int nvs_pre_rule_check_run(nvs_check_scope_e scope, const char* target_id, cJSON* payload, nvs_check_res_t* out_array, int max_capacity);
```

### 5.2 引擎集中化工具 API

供各层级回调函数使用，标准化错误项的拼接与压栈过程。

```c
/**
 * @brief 错误项压栈工具函数
 * @param ctx   由引擎创建并传递的校验上下文
 * @param level 严重等级
 * @param code  错误码
 * @param path  触发错误的定位路径
 * @param msg   详细预检描述
 */
void nvs_chk_append_error(nvs_chk_ctx_t* ctx, nvs_check_level_e level, int code, const char* path, const char* msg);
```

### 5.3 回调函数签名注入

系统现有结构体需新增对应检查接口，供引擎底层路由调用。

```c
// 1. Node 级工具操作集 (nvs_node_ops_t) 扩展
void (*check_rule)(cJSON* blackboard, cJSON* payload, nvs_chk_ctx_t* ctx);

// 2. Prog 级操作集扩展
void (*check_topology)(cJSON* prog_cfg, nvs_chk_ctx_t* ctx);
```

## 6. 能力集 (nvs_capability) 联动规范

预检引擎在执行各级规则比对时，强依赖系统全局能力集进行边界确认。

**交互原则：**

1. **只读访问：** 预检代码块中仅允许调用 `nvs_cap_get_*` 接口，严禁调用写接口。
2. **缺省防范：** 当能力集读取失败 (`NVS_CAP_ERR_NOT_FOUND`) 时，预检逻辑必须提供缺省安全限制或抛出 `FATAL` 拦截。

**能力联动代码示例 (Node 层校验回调)：**

```c
static void vi_tool_check_rule(cJSON* blackboard, cJSON* payload, nvs_chk_ctx_t* ctx) {
    cJSON* target = (payload != NULL) ? payload : blackboard;
    if (!target) return;

    cJSON* expo_item = cJSON_GetObjectItem(target, "exposure_time");
    if (expo_item != NULL) {
        int req_expo = expo_item->valueint;
        int max_expo = 0;
        
        // 联动能力集获取硬件上限
        if (nvs_cap_get_int("vi.hframe.exposure_max", &max_expo) == NVS_CAP_OK) {
            if (req_expo > max_expo) {
                nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1004, "exposure_time", 
                                     "Exposure time exceeds capability limits.");
            }
        } else {
            // 能力集读取失败的安全拦截
            nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1000, "exposure_time", 
                                 "Capability baseline missing.");
        }
    }
}
```
