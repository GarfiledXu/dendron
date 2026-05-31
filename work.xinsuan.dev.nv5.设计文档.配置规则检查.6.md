---
id: in7ynvcq149zkew27znjrtj
title: '6'
desc: ''
updated: 1778062522031
created: 1778062287531
---

# 配置检查模块设计文档

**文档编号**：NVS-ARCH-042
**模块名称**：`nvs_pre_rule_check`
**架构形态**：主动查询式独立预检机制 (Active-Query Independent Pre-check)

---

## 一、 模块概述和设计原则

### 1.1 功能定义

本模块在系统执行参数修改（SetParam）或控制触发（Trigger/Learn）之前，先行对系统级限制、程序内节点依赖以及工具节点的参数合法性与状态就绪度进行全面检查与验证。在架构上，它作为上下位机交互之间或下位机内部执行前置校验的独立预检服务存在。

### 1.2 价值体现

1. **防崩溃保护：** 在脏数据或残缺配置进入底层算法引擎前予以拦截，防止句柄生成失败、内存越界或状态机死锁。
2. **状态预判：** 提前评估硬件资源水位与逻辑拓扑状态。
3. **UI 协同：** 通过精准透传 JSON 定位路径 (`err_path`)，直接驱动上位机前端界面实现控件级的高亮标红与提示反馈。

### 1.3 核心设计原则

1. **状态隔离：** 预检过程是纯粹的只读计算，绝对不修改底层黑板（Blackboard）配置区数据，也不触发设备真实动作。
2. **分层路由：** 检查逻辑不集中堆砌，而是依附于现有的系统层级，由预检引擎按需分发。
3. **高扩展性：** 通过统一的错误压栈上下文 API，降低各业务模块接入预检逻辑的开发成本。

---

## 二、 角色定义和执行链路

### 2.1 角色定义

系统参与方从宏观到微观划分为两层及具体的执行模块：

* **上位机 (Upper Machine)：**
  * **前端客户端/Web：** 触发方。负责发起诊断指令，并根据下位机返回的结构化异常数组更新 UI 交互状态。
* **下位机 (Lower Machine)：**
  * **NCM (通信模块)：** 负责拆解上位机定界符协议，提取核心指令与 JSON 负载，并向后路由。
  * **NDM (设备管理模块)：** 接收 NCM 分发的数据，作为下位机的总调控者，调用预检引擎主入口。
  * **Pre-Rule Check Engine (预检引擎)：** 诊断调度的核心控制器。负责初始化上下文、解析作用域 (Scope)，并寻址到对应的底层实体。
  * **Prog/Node 实体 (程序与节点)：** 实际的规则校验执行方。持有当前状态数据与内部限制条件，通过注册的回调函数完成验证逻辑。

### 2.2 执行链路

1. 上位机发送 `Pre_Rule_Check` 指令。
2. NCM 接收包体，完成校验与去格式化后移交 NDM。
3. NDM 唤起 `nvs_pre_rule_check_run` 接口，传入层级信息与测试载荷。
4. 预检引擎创建校验上下文 (`nvs_chk_ctx_t`)，并根据 Scope 定位到对应的 Prog 管理器或 Node 工具注册的回调函数。
5. 底层模块结合测试载荷、黑板数据及全局能力集 (`nvs_capability`) 运行规则比对，将产生的错误项压入校验上下文。
6. 预检引擎收集所有错误项，交由 NDM 与 NCM 封装为标准的应答报文 (ACK) 返回给上位机。

---

## 三、 检查目标和结果的层级划分（概念定义）

### 3.1 检查目标层级 (Scope)

明确检查的实体对象级别，分为三层：

* **SYSTEM (0)：** 系统级。评估设备全局硬上限与软上限（如系统总可用内存、支持的算法库类型清单）。
* **PROG (1)：** 程序级。评估程序内部的拓扑关系、工具节点总数、输入输出流的连通性。
* **NODE (2)：** 节点级。评估具体单一工具的内部参数合法性、枚举范围、以及独立状态机的就绪情况。

### 3.2 结果严重等级与动作定义 (Level)

对产出的错误项进行分级，指导上位机后续的调度决策：

* **INFO / PASS (0)：** 检查通过，允许放行。
* **WARN (1)：** 警告。检测到逻辑风险（如“工具连线正确但未加载学习模型”），但不强制阻断动作指令的下发。
* **FATAL (2)：** 致命。检测到严重越界或缺失核心前置参数。此状态意味着如果强行执行下发或触发指令，系统必然发生故障。

---

## 四、 上下位机交互时序和协议

### 4.1 交互时序

预检指令属于同步的 Request/Response 模型。上位机发起查询后，必须等待下位机返回 ACK 报文后，再根据结果决定是否继续下发真正的配置写入（`SetParam`）或调度指令（`Trigger`）。

### 4.2 报文格式

通信严格遵循定界符协议格式：`指令-#JSON负载[CR]`。

* **分隔符**：`-#`
* **结束符**：`[CR]`

### 4.3 报文示例

**预检请求 (Request)：**

```text
Pre_Rule_Check-#{"scope":2,"target_id":"ocr_node_01","payload":{"threshold":300,"mode":1}}
```

**正常应答 (ACK - 无异常通过)：**

```text
Pre_Rule_Check-#{"status":0,"errors":[]}[CR]
```

**正常应答 (ACK - 存在异常拦截)：**

```text
Pre_Rule_Check-#{"status":0,"errors":[{"level":2,"err_code":1004,"err_path":"payload.threshold","err_msg":"Exceeds cap limits"},{"level":1,"err_code":2001,"err_path":"blackboard.status","err_msg":"Untrained"}]}[CR]
```

**系统级失败应答 (ER - 指令解析或系统故障)：**

```text
ER-#Pre_Rule_Check-#33[CR]
```

---

## 五、 下位机内部实现

### 5.1 核心数据结构

```c
// 校验层级作用域
typedef enum {
    NVS_CHK_SCOPE_SYSTEM = 0, 
    NVS_CHK_SCOPE_PROG   = 1, 
    NVS_CHK_SCOPE_NODE   = 2  
} nvs_check_scope_e;

// 严重等级
typedef enum {
    NVS_CHK_PASS  = 0,  
    NVS_CHK_WARN  = 1,  
    NVS_CHK_FATAL = 2   
} nvs_check_level_e;

// 数据传输对象 (DTO)
typedef struct {
    nvs_check_level_e level;      
    int err_code;                 
    char err_path[128];           
    char err_msg[128];            
} nvs_check_res_t;

// 引擎上下文管理器 (用于在回调流转中安全记录错误)
typedef struct {
    nvs_check_res_t* results;     
    int max_capacity;             
    int current_count;            
    int global_status;            
} nvs_chk_ctx_t;
```

### 5.2 接口规范

#### 5.2.1 供其他模块使用的工具 API (引擎入口)

NDM 或下位机内部状态机在需要发起检查时调用的统一接口。

```c
/**
 * @brief 执行规则预检主入口
 * @param scope         目标层级 (System/Prog/Node)
 * @param target_id     目标实体标识符
 * @param payload       测试参数载荷 (执行状态检查时可传 NULL)
 * @param out_array     外部传入的内存池指针，用于接收输出
 * @param max_capacity  内存池容量
 * @return 实际产出的错误项总数
 */
int nvs_pre_rule_check_run(nvs_check_scope_e scope, const char* target_id, cJSON* payload, nvs_check_res_t* out_array, int max_capacity);
```

#### 5.2.2 回调实现使用的 API (错误压栈)

暴露给具体业务模块使用的内置工具函数，隔离底层的数组维护与越界检查逻辑。

```c
/**
 * @brief 将预检发现的错误压入上下文
 * @param ctx   校验上下文指针
 * @param level 严重级别
 * @param code  错误归类码
 * @param path  具体的 JSON 定位路径
 * @param msg   描述文本
 */
void nvs_chk_append_error(nvs_chk_ctx_t* ctx, nvs_check_level_e level, int code, const char* path, const char* msg);
```

#### 5.2.3 回调函数的签名定义

系统现有实体操作集 (`Ops`) 需补充的扩展坑位，由模块维护者填充具体规则。

```c
// Prog 级程序管理器回调集扩展
typedef struct {
    // ... 原有回调 ...
    void (*check_topology)(cJSON* prog_cfg, nvs_chk_ctx_t* ctx);
} nvs_prog_mgr_ops_t;

// Node 级工具实体回调集扩展
typedef struct {
    const char* tool_type;
    // ... 原有回调 ...
    void (*check_rule)(cJSON* blackboard_data, cJSON* test_payload, nvs_chk_ctx_t* ctx);
} nvs_node_ops_t;
```

---

## 六、 具体场景与规则填充举例

### 6.1 场景一：能力集静态联动校验

针对测试参数是否超越硬件极限进行的只读比对。

```c
// 示例：校验曝光时间是否在能力集支持的范围内
cJSON* expo_item = cJSON_GetObjectItem(payload, "exposure_time");
if (expo_item) {
    int max_expo = 0;
    // 读取能力集
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

在 SYSTEM 或 PROG 层，结合下发意图与设备实时状态进行安全水位运算。

```c
// 示例：检查新建工具后的总数是否超出安全容量
int current_tool_count = prog_mgr_get_total_tools();
int added_tools_count = cJSON_GetArraySize(cJSON_GetObjectItem(payload, "new_nodes"));
int max_allowed_tools = 0;

nvs_cap_get_int("atool.max_tools", &max_allowed_tools);

if ((current_tool_count + added_tools_count) > max_allowed_tools) {
    nvs_chk_append_error(ctx, NVS_CHK_FATAL, 3001, "new_nodes", "Exceeds system maximum tool allowance.");
}
```

### 6.3 场景三：拓扑连线与业务逻辑合法性

在 PROG 层检查模块间数据流的连通性。

```c
// 示例：检查当前程序的分析工具是否成功绑定了输入图像
cJSON* nodes = cJSON_GetObjectItem(prog_cfg, "nodes");
cJSON* node = NULL;
cJSON_ArrayForEach(node, nodes) {
    if (strcmp(cJSON_GetStringValue(cJSON_GetObjectItem(node, "category")), "analysis") == 0) {
        cJSON* inputs = cJSON_GetObjectItem(node, "inputs");
        if (!cJSON_HasObjectItem(inputs, "image_source")) {
             char path_buffer[64];
             snprintf(path_buffer, sizeof(path_buffer), "nodes[%d].inputs.image_source", get_node_index(node));
             nvs_chk_append_error(ctx, NVS_CHK_FATAL, 3010, path_buffer, "Mandatory input image connection is floating.");
        }
    }
}
```

### 6.4 场景四：算法核心句柄生成的强依赖拦截

在 NODE 层强制检查算法内部运行必须的前提参数。

```c
// 示例：检查 OCR 工具必须存在的 mode 字段，防止内部句柄初始化异常
cJSON* mode_item = cJSON_GetObjectItem(payload ? payload : blackboard_data, "mode");
if (mode_item == NULL) {
    nvs_chk_append_error(ctx, NVS_CHK_FATAL, 1020, "mode", "Missing mandatory parameter 'mode' for internal handle generation.");
}
```


功能定义还是有问题，你的描述没有明确上下危机，笼统的用系统是什么意思
上下位机交互时序和协议是字段解释，不是所谓的报文格式，这个是都早就定义的，然后具体场景
与规则填充举例怎么都没有了，变少掉了，诊断部分的内容还是先不要给我了