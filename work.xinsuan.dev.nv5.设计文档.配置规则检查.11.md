---
id: 0rn8s0861d3yp3r1xwrgl5b
title: '11'
desc: ''
updated: 1778125562878
created: 1778125351166
---

# 规则检查(CRC) 模块设计文档

**文档编号**：NVS-ARCH-042  
**模块名称**：`nvs_pre_rule_check`  
**架构形态**：主动查询式预检机制

## 一、 模块概述和设计原则

### 1.1 功能定义

作为 **下位机** 的统一规则验证服务，负责接收来自 **上位机** 的预检请求，在实际执行配置保存、运行触发、资源申请等动作之前，对目标对象进行合法性评估与运行条件验证。

模块根据请求中是否携带配置负载，分为两类校验内容：

#### 1）运行状态检查（不携带配置负载）

当：

```json
"payload_type": ""
"payload": {}
```

时，表示仅对目标对象当前真实运行状态进行评估。

此时下位机会基于：

- 当前设备资源环境
- 内部状态机
- 程序拓扑关系
- Tool 运行状态
- 系统实时能力

进行检查。

典型场景包括：

- Flash 是否充足
- Tool 是否已学习
- Camera 是否 Ready
- 程序拓扑是否完整
- 当前状态是否允许执行

该类问题通常属于：

- 状态级错误
- 拓扑级错误
- 资源级错误

因此：

```text
err_param_path 通常为空
```

因为错误并不对应某个具体参数字段。

---

#### 2）配置负载检查（携带配置负载）

当请求中携带：

```json
"payload_type"
"payload"
```

时，下位机会在运行状态检查的基础上，额外对透传配置内容进行合法性验证。

包括：

- 参数范围是否合法
- 枚举值是否有效
- 当前硬件是否支持
- 配置组合是否冲突
- 是否超出能力边界
- 是否违反工具内部规则

该类检查本质属于：

```text
“配置是否合法”
```

下位机会返回：

```text
err_param_path
```

用于精准指出问题字段。

例如：

```json
{
  "err_param_path": "threshold"
}
```

上位机即可：

- 标红输入框
- 自动定位表单项
- 展示对应错误提示

实现：

```text
真正与固件能力绑定的动态防呆
```

---

### 1.2 价值体现

#### 1）规则解释权收敛至下位机

本模块最核心的价值之一，是将：

```text
规则校验逻辑
```

从：

```text
上位机 UI / SDK
```

转移至：

```text
下位机固件内部
```

实现：

```text
规则与固件能力绑定
```

而不是：

```text
规则与上位机版本绑定
```

避免：

- 固件升级后规则漂移
- 多平台规则不一致
- 参数边界硬编码
- UI 与真实能力脱节

最终由：

```text
由执行方进行规则的检查解释
```

---

#### 2）系统资源前置校验

在执行：

- 文件保存
- 主图缓存
- 样本追加
- AI 模型加载

等资源消耗型动作之前，由下位机基于真实物理环境进行预检查。

包括：

- Flash 剩余空间
- 内存占用
- DMA Buffer
- Runtime 状态
- 模型加载情况

避免：

```text
逻辑允许
但物理资源不足
```

导致的执行失败。

---

#### 3）拓扑与依赖关系统一验证

在程序运行前，由下位机统一验证：

- Tool 输入是否悬空
- 数据流是否闭环
- 必要节点是否存在
- Tool 是否满足依赖状态

避免：

```text
程序“存在”
≠
程序“可执行”
```

---

#### 4）动态配置预校验（防呆）

上位机在用户修改配置但尚未保存时，可实时透传参数负载至下位机。

由固件基于：

- 当前型号
- 当前能力集
- 当前 License
- 当前资源状态

动态返回精准错误。

包括：

- 错误码
- 问题字段路径
- 默认错误描述

实现：

```text
真正与固件同步的动态防呆
```

而不是：

```text
上位机维护静态规则表
```

---

#### 5）上位机轻量化与解耦

上位机不再需要维护：

- 参数上下限
- Tool 能力边界
- 拓扑规则
- 固件差异逻辑
- 复杂组合限制

仅负责：

```text
数据透传 + 结果展示
```

实现：

```text
UI 与业务规则彻底解耦
```

---

### 1.3 核心设计原则

#### 1）无副作用

预检过程：

- 不修改内部数据
- 不保存配置
- 不触发执行
- 不改变状态机

仅进行规则评估。

---

#### 2）职责划分明确

各层级实体仅维护自身规则：

| 层级 | 职责 |
|---|---|
| SYSTEM | 系统资源与能力 |
| PROG | 程序拓扑与工具关系 |
| NODE | Tool 内部参数与状态 |

避免：

```text
跨层越权判断
```

---

#### 3）下位机拥有最终解释权

真正有效的规则：

```text
以下位机返回结果为准
```

上位机不保证规则正确性。

---

#### 4）从“松散交互”转向“固件绑定式交互”

传统模式：

```text
上位机：
“我认为这个参数合法”
↓
直接下发
↓
下位机执行失败
```

属于：

```text
松散式交互
```

问题本质在于：

```text
上位机与真实执行环境脱节
```

当前方案：

```text
上位机：
“请帮我检查”
↓
下位机基于真实环境评估
↓
返回精准错误
↓
上位机仅负责展示
```

属于：

```text
固件绑定式交互
```

本质上实现：

```text
规则解释权与执行权统一
```

---

## 二、 角色定义和执行链路

### 2.1 角色定义

#### 1）预检调度器 (Pre-check Dispatcher)

负责：

- 解析协议外层
- 创建上下文
- 路由目标对象
- 分发检查请求

不感知具体业务参数内容。

属于：

```text
规则分拣层
```

---

#### 2）Prog/Node 实体 (程序与节点)

负责：

- 实际规则执行
- 参数合法性验证
- 状态机检查
- 能力集匹配
- 拓扑关系验证

属于：

```text
规则拥有者
```

---

### 2.2 执行链路

1. 上位机下发 `Pre_Rule_Check` 查询请求。
2. 指定：
   - Scope
   - Target ID
   - Payload Type
   - Payload
3. 下位机 NDM 调用 `nvs_pre_rule_check_run`。
4. Dispatcher 创建上下文并路由目标对象。
5. 底层模块结合：
   - 当前真实环境
   - 当前运行状态
   - 透传配置负载（若有）
   - 全局能力集
   进行规则评估。
6. 异常项压入上下文。
7. Dispatcher 汇总错误并返回 ACK。

---

## 三、 检查层级与严重程度

### 3.1 检查目标层级 (Scope)

| Scope | 含义 |
|---|---|
| SYSTEM (0) | 系统级资源与能力 |
| PROG (1) | 程序拓扑与依赖 |
| NODE (2) | Tool 参数与状态 |

---

### 3.2 严重等级 (Level)

| Level | 含义 |
|---|---|
| WARN (1) | 风险提示，不阻断执行 |
| FATAL (2) | 致命错误，强制拦截 |

---

## 四、 上下位机交互负载定义

### 4.1 请求字段 (Request Payload)

| 字段 | 说明 |
|---|---|
| scope | 检查层级 |
| target_id | 目标 ID |
| payload_type | 配置结构类型 |
| payload | 待校验负载 |

所有字段必须始终存在。

无配置负载时：

```json
{
  "payload_type": "",
  "payload": {}
}
```

---

### 4.2 应答字段 (ACK Payload)

| 字段 | 说明 |
|---|---|
| scope | 请求层级回显 |
| target_id | 请求目标回显 |
| errors | 错误列表 |

错误项结构：

| 字段 | 说明 |
|---|---|
| level | 严重等级 |
| err_code | 全局错误码 |
| err_target_id | 报错实体 ID |
| err_param_path | 参数路径 |
| err_msg | 默认错误描述 |

---

### 4.3 `err_param_path` 的核心意义

该字段是：

```text
UI 路由核心字段
```

---

#### 非空

表示：

```text
参数级错误
```

前端可：

- 标红输入框
- 自动聚焦
- 定位表单项

例如：

```json
"threshold"
```

---

#### 为空

表示：

```text
状态级 / 拓扑级 / 资源级错误
```

前端可：

- 高亮节点
- 弹出全局警告
- 阻断运行

---

### 4.4 负载数据示例

#### 场景：OCR Tool 参数预校验

```json
// 请求
{
  "scope": 2,
  "target_id": "ocr_node_01",
  "payload_type": "ParamConfig",
  "payload": {
    "threshold": 300
  }
}
```

```json
// 应答
{
  "scope": 2,
  "target_id": "ocr_node_01",
  "errors": [
    {
      "level": 2,
      "err_code": 1004,
      "err_target_id": "ocr_node_01",
      "err_param_path": "threshold",
      "err_msg": "Exceeds max limit 255"
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

---

### 5.2 核心 API

```c
/**
 * @brief 执行规则预检主入口
 */
int nvs_pre_rule_check_run(
    nvs_check_scope_e scope,
    const char* target_id,
    const char* payload_type,
    cJSON* payload,
    nvs_check_res_t* out_array,
    int max_capacity
);

/**
 * @brief 错误项压栈辅助函数
 */
void nvs_chk_append_error(
    nvs_chk_ctx_t* ctx,
    nvs_check_level_e level,
    int code,
    const char* target_id,
    const char* param_path,
    const char* msg
);
```

---

## 六、 具体场景与规则填充举例

### 6.1 场景一：系统资源与限额交叉检查 (SYSTEM 层)

场景：

在执行：

- 保存主图
- 导出配置
- AI 模型加载

等动作前，检查真实资源环境。

核心思想：

```text
逻辑允许
≠
物理环境允许
```

```c
int required_space_kb = calculate_estimated_size(new_request_count);
int free_space_kb = system_get_flash_free_space();

if (free_space_kb < required_space_kb) {
    nvs_chk_append_error(
        ctx,
        NVS_CHK_FATAL,
        4010,
        "system",
        "",
        "Insufficient physical Flash space for this operation."
    );
}
```

---

### 6.2 场景二：程序拓扑与可执行状态验证 (PROG 层)

场景：

运行前检查程序数据流与依赖关系。

核心思想：

```text
程序存在
≠
程序可运行
```

```c
cJSON* nodes = cJSON_GetObjectItem(prog_cfg, "nodes");
cJSON* node = NULL;

cJSON_ArrayForEach(node, nodes) {

    if (strcmp(
            cJSON_GetStringValue(
                cJSON_GetObjectItem(node, "category")
            ),
            "analysis"
        ) == 0) {

        cJSON* inputs = cJSON_GetObjectItem(node, "inputs");

        if (!cJSON_HasObjectItem(inputs, "image_source")) {

            const char* bad_node_id =
                cJSON_GetStringValue(
                    cJSON_GetObjectItem(node, "id")
                );

            nvs_chk_append_error(
                ctx,
                NVS_CHK_FATAL,
                3010,
                bad_node_id,
                "",
                "Program not executable: Mandatory input connection is floating."
            );
        }
    }
}
```

---

### 6.3 场景三：工具内部运行状态评估 (NODE 层)

场景：

运行前检查 Tool 内部状态机。

例如：

- 模板是否学习
- 模型是否加载
- Tool 是否初始化

核心思想：

```text
配置正确
≠
运行状态正确
```

```c
static void match_tool_check_rule(
    cJSON* blackboard,
    const char* payload_type,
    cJSON* payload,
    nvs_chk_ctx_t* ctx
) {

    if (payload == NULL) {

        if (tool_get_internal_status(blackboard) != STATUS_LEARNED) {

            nvs_chk_append_error(
                ctx,
                NVS_CHK_WARN,
                2001,
                "match_node_02",
                "",
                "Tool is currently untrained. Execution may fail."
            );
        }

        return;
    }
}
```

---

### 6.4 场景四：动态配置预校验与防呆 (NODE 层)

场景：

用户修改 Tool 参数但尚未保存。

上位机实时透传负载，由固件动态校验。

核心思想：

```text
未保存
≠
无法验证
```

```c
static void ocr_tool_check_rule(
    cJSON* blackboard,
    const char* payload_type,
    cJSON* payload,
    nvs_chk_ctx_t* ctx
) {

    if (payload != NULL && payload_type != NULL) {

        if (strcmp(payload_type, "ParamConfig") == 0) {

            cJSON* threshold =
                cJSON_GetObjectItem(payload, "threshold");

            if (threshold) {

                int max_limit = 0;

                nvs_cap_get_int(
                    "ocr.threshold_max",
                    &max_limit
                );

                if (threshold->valueint > max_limit) {

                    nvs_chk_append_error(
                        ctx,
                        NVS_CHK_FATAL,
                        1004,
                        "ocr_node_01",
                        "threshold",
                        "Threshold parameter exceeds hardware limits."
                    );
                }
            }
        }

        else if (strcmp(payload_type, "AdvancedConfig") == 0) {

            cJSON* mode =
                cJSON_GetObjectItem(payload, "mode");

            if (mode && !is_valid_enum(mode->valueint)) {

                nvs_chk_append_error(
                    ctx,
                    NVS_CHK_FATAL,
                    1005,
                    "ocr_node_01",
                    "mode",
                    "Invalid enumeration value for running mode."
                );
            }
        }
    }
}
```

---

## 七、 总结

`nvs_pre_rule_check` 的核心并不仅仅是：

```text
“规则检查”
```

更本质的是：

```text
规则归属权的重新划分
```

即：

```text
从“上位机维护规则”
转向
“下位机定义规则”
```

最终实现：

- 固件能力统一收口
- UI 与规则彻底解耦
- 多平台行为一致
- 动态能力自适应
- 真实运行环境校验
- 参数级精准防呆
- 拓扑与状态统一验证

本模块本质上是：

```text
系统运行规则的统一解释层
```

也是：

```text
上位机与下位机之间
从“配置下发”
转向
“能力协商”
的重要基础设施
```