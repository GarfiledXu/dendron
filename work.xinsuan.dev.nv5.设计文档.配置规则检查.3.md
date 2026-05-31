---
id: edskih8qn2rt2ixyig7z8ue
title: '3'
desc: ''
updated: 1778057797878
created: 1778057260871
---

# 智能相机 Pre-Rule Check 模块设计与技术规格说明书 (V3.1)

**文档编号**：NVS-ARCH-042
**模块名称**：`nvs_pre_rule_check`
**架构形态**：外挂式独立预检

## 1. 模块定位与架构总览

`nvs_pre_rule_check` 模块定位于下位机系统的独立状态预判与规则检查层。
本模块提供无副作用的“实体状态预检服务”。上位机通过专用的预检指令主动发起同步询问，下位机执行计算并以结构化数据 (Structured DTO) 返回校验结果。

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

上位机发送 `CMD_PRE_CHECK` 指令发起预检。请求参数包含检查层级、目标标识和测试载荷。

### 3.1 预检请求报文 (Request)

**参数定义：**

* `scope`: 整型，对应 `nvs_check_scope_e`。
* `target_id`: 字符串，对应的程序 ID 或节点 ID（System 层级可传 `null`）。
* `payload`: JSON 对象。若下发具体参数，则执行**“参数预检”**；若传 `null`，则执行基于黑板数据的**“就绪状态检查”**。

**报文示例 (节点级参数预检)：**

```json
{
  "cmd": "pre_check",
  "scope": 2,
  "target_id": "ocr_node_01",
  "payload": {
    "threshold": 300,
    "mode": 1
  }
}
```

### 3.2 预检应答报文 (ACK)

下位机检查完毕后，将所有 `level > 0` 的异常项封装为数组返回。若数组为空，则代表检查全量通过。

**报文示例 (存在异常拦截)：**

```json
{
  "cmd": "pre_check_ack",
  "status": 0,
  "errors": [
    {
      "level": 2,
      "err_code": 1004,
      "err_path": "threshold",
      "err_msg": "Value exceeds capability limit."
    },
    {
      "level": 1,
      "err_code": 2001,
      "err_path": "status",
      "err_msg": "Tool is not trained. Inference may fail."
    }
  ]
}
```

## 4. 实体职责划分与校验链路

| 层级 (Scope) | 执行实体 | 检查职责描述 | 典型场景示例 |
| :--- | :--- | :--- | :--- |
| **SYSTEM (0)** | NDM 引擎核心 | 负责全系统的宏观软硬件防线。 | 1. 计算 Flash 余量是否能支撑新模板导入。<br>2. 校验请求的新工具类型是否存在于 `atool.supported_types`。 |
| **PROG (1)** | Prog 管理器 | 负责程序内部结构与拓扑完整性。 | 1. 检查节点连线是否悬空或存在死循环。<br>2. 检查程序内总工具数是否超越 `atool.max_tools`。 |
| **NODE (2)** | Node 工具回调 | 负责单工具的配置闭环与运行态前置条件。 | 1. 验证 OCR `mode` 等内部句柄生成强依赖参数。<br>2. 判断工具是否已完成 `Learn` 状态切换。 |

## 5. 底层 API 与引擎实现规范

### 5.1 引擎对外调度接口

由 NDM 模块或下位机内部状态机调用。

```c
/**
 * @brief 执行规则校验引擎主入口
 * @param scope         检查层级 (System/Prog/Node)
 * @param target_id     目标实体 ID
 * @param payload       测试参数载荷 (状态检查可传 NULL)
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
// 各算法模块维护者必须实现此回调以守护本模块数据边界
void (*check_rule)(cJSON* blackboard, cJSON* payload, nvs_chk_ctx_t* ctx);

// 2. Prog 级操作集扩展
void (*check_topology)(cJSON* prog_cfg, nvs_chk_ctx_t* ctx);
```

## 6. 能力集 (nvs_capability) 联动规范

预检引擎在执行各级规则比对时，必须依赖系统全局能力集进行边界确认。

**交互原则：**

1. **只读性：** 预检代码块中仅允许调用 `nvs_cap_get_*` 接口，严禁修改能力集结构。
2. **缺省防范：** 当 `nvs_cap_get` 返回 `NVS_CAP_ERR_NOT_FOUND` 时，预检逻辑必须提供 Fallback（降级）安全限制或直接抛出 `FATAL` 拦截，绝不能允许无限制执行。

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
                // 调用引擎工具函数统一拼装报警信息
                nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1004, "exposure_time", 
                                     "Exposure time exceeds capability limits.");
            }
        } else {
            // 能力集获取失败的安全防御机制
            nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1000, "exposure_time", 
                                 "Capability baseline missing, check denied.");
        }
    }
}
```

## 思考

当存在多个检查项（连续执行多个检查回调），一旦其中一个生成了错误结果，是否继续呢?