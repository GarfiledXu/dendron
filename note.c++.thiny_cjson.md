# `thiny_cjson` 模块架构与设计规范

## 1. 核心设计思路与需求

### 1.1 背景与痛点分析

在 C++ 工程中直接集成 C 语言风格的 `cJSON` 库，通常会引入以下工程风险：

1. **内存泄漏与野指针风险**：原生 API 要求手动管理内存生命周期（`cJSON_Delete`）。在复杂的错误处理链路中，极易发生内存泄漏或重复释放（Double Free）。
2. **弱类型导致的未定义行为 (UB)**：`cJSON` 依赖统一的结构体存储不同类型的数据。如果业务层错误地将一个字符串节点按数字类型读取，原生接口会隐式返回 `0`。这种静默错误会掩盖数据异常，导致难以追踪的逻辑缺陷。
3. **结构污染**：当向非对象（Object）节点（如数组或字符串）强行调用键值对插入接口时，底层会发生越界附加，导致原始数据结构被破坏。

### 1.2 架构设计原则

为消除上述风险，本模块采用了以下设计策略：

* **RAII 内存所有权模型**：利用 C++ 对象的生命周期自动管理 C 裸指针。明确划分数据共享（视图）与所有权转移的边界，实现自动回收。
* **防御性编程 (Defensive Programming)**：在数据读取、遍历和修改等所有操作前，设置严格的类型检查与边界约束，隔离外部非法数据。
* **基于宏展开的静态反射**：通过变参宏技术，建立 C++ 原生结构体与 JSON 节点之间的映射，自动化生成序列化与反序列化代码，消除硬编码。

### 1.3 核心优势

* **稳定性**：通过完善的安全回退（Fallback）机制，确保系统在面对畸形报文或非法操作时，不会触发崩溃或异常抛出。
* **开发效率**：支持高达 100 个字段的自动化映射，显著减少样板代码（Boilerplate Code）的编写。
* **轻量级**：仅依赖单头文件与单一 C 文件，无额外第三方依赖，适用于对体积与性能敏感的嵌入式及跨平台环境。

### 1.4 接口调用对比

为了直观展现模块的封装效果，以下给出两类典型场景的调用对比。

#### 场景 A：数据提取与防御机制

目标：从网络负载中提取设备 `id`，若提取失败或类型不符，则回退为默认值 `999`。

**原生 `cJSON` 调用方式：**

```c
cJSON* root = cJSON_Parse(payload);
int id = 999; 
if (root != NULL) {
    cJSON* item = cJSON_GetObjectItem(root, "id");
    if (item != NULL && cJSON_IsNumber(item)) {
        id = item->valueint;
    }
    cJSON_Delete(root); // 必须在所有退出路径中手动释放
}
```

**`thiny_cjson` 调用方式：**

```cpp
Json j = Json::parse(payload);
// 包含存在性检测、类型校验与默认值回退机制。内存由析构函数自动回收。
int id = j["id"].as_int(999); 
```

#### 场景 B：结构体映射与序列化

目标：完成多层级 C++ 结构体与 JSON 的相互转换。

**自动化映射实现：**

```cpp
// 1. 定义原生结构体
struct DeviceConfig {
    int id;
    std::string model;
    bool is_active;
    std::vector<int> ports;
};

// 2. 使用宏建立映射关系
JSON_DEFINE(DeviceConfig, id, model, is_active, ports)

void process_network_msg(const std::string& msg) {
    // 反序列化：JSON 字符串至 C++ 对象
    Json j = Json::parse(msg);
    DeviceConfig config;
    if (from_json(j, config)) {
        // 配置解析成功，可进行业务处理
    }

    // 序列化：C++ 对象至 JSON 字符串
    config.is_active = false;
    std::string out_msg = to_json(config).dump(); 
}
```

---

## 2. 接口语义规范与详尽说明

本节详细说明 `Json` 核心类的接口声明及其设计语义。

### 2.1 生命周期管理

```cpp
explicit Json(cJSON *root, bool owner = true);
Json(Json &&o) noexcept;
Json &operator=(Json &&o) noexcept;
Json(const Json &o);
Json &operator=(const Json &o);
```

* **语义说明**：
  * **所有权标志 (`owner`)**：控制底层内存的释放权限。`owner = true` 表示当前对象负责在析构时销毁 `root_`；`owner = false` 表示当前对象仅作为视图（View）存在，不进行内存释放操作。
  * **移动操作 (Move)**：转移内存指针的所有权，并将源对象置为空壳，优化性能。
  * **拷贝操作 (Copy)**：执行深拷贝 (`cJSON_Duplicate`)，确保对象间内存隔离。

### 2.2 状态侦测

```cpp
bool valid() const;
bool is_array() const;
bool is_object() const;
bool is_null() const;
bool is_string() const;
bool is_number() const;
bool is_bool() const;
```

* **语义说明**：
  组合指针有效性检查与 C 底层 `type` 标志位校验，防止类型混淆。

### 2.3 数据提取 (安全回退)

```cpp
int as_int(int def = 0) const;
double as_double(double def = 0.0) const;
bool as_bool(bool def = false) const;
std::string as_string(const std::string &def = "") const;
```

* **语义说明**：
  执行严格的类型检查。如果节点不存在或底层类型不匹配，则放弃提取操作，安全返回 `def` 默认值。

### 2.4 结构访问与查询

```cpp
Json operator[](const char *key) const;
Json operator[](int index) const;
bool contains(const char *key) const;
int size() const;
```

* **语义说明**：
  * `operator[](const char*)` 与 `contains` 仅在当前节点为 `Object` 时生效。
  * `operator[](int)` 与 `size` 仅在当前节点为 `Array` 时生效。
  对于非法调用，均返回空节点或默认值。

### 2.5 数据构建

```cpp
void add(const char *key, int v);
void add(const char *key, double v);
void add(const char *key, bool v);
void add(const char *key, const std::string &v);
void add(const char *key, const Json &v);
void add(const char *key, Json &&v);
```

* **语义说明**：
  所有 `add` 接口强制校验当前节点必须为 `Object` 类型，拒绝向数组或标量节点插入键值对，防止破坏内部结构。

### 2.6 安全遍历

```cpp
std::vector<std::string> keys() const;
std::vector<Json> items() const;
```

* **语义说明**：
  分别针对 `Object` 的键名和 `Array` 的元素进行遍历提取。对于不匹配的数据类型，安全返回空容器，保障迭代器的稳定性。

---

## 3. 测试用例分类与映射矩阵

本模块的测试体系分为五大核心维度。在进行任何底层修改后，必须确保对应分类的 Doctest 测试用例全部通过，以维持系统的工程健壮性。

### 3.1 核心功能与内存验证 (Core & Memory)

**测试目标**：验证基本的序列化机制、移动语义以及内存隔离的有效性。

* **基础映射测试** (`TEST_CASE("basic serialize/deserialize")`)：验证普通结构体的字段解析。
* **移动语义测试** (`TEST_CASE("move constructor")`, `TEST_CASE("move assignment")`)：验证资源转移后源对象状态是否正确。
* **深拷贝隔离测试** (`TEST_CASE("copy constructor")`, `TEST_CASE("copy assignment")`)：验证复制后对象的修改是否互不干扰。
* **可选字段测试** (`TEST_CASE("optional null")`)：验证 `std::optional` 成员对于缺失字段的处理逻辑。

### 3.2 严格类型防御 (Type Defense)

**测试目标**：验证在底层弱类型环境下，C++ 接口能否正确拦截非法的类型转换请求。

* **基础类型校验** (`TEST_CASE("Strict type checking (is_xxx)")`)：验证 `is_number`、`is_string` 等接口的准确与互斥。
* **数据提取防御** (`TEST_CASE("Strict type extraction fallback (as_xxx)")`)：验证在使用 `as_int` 等接口读取类型错乱的节点时，能否准确触发保底机制（Fallback），避免提取非法默认值。

### 3.3 结构与边界约束 (Structural Boundaries)

**测试目标**：验证业务层对非法节点执行越界操作时的系统稳定性。

* **非法层级访问测试** (`TEST_CASE("Structural boundary defenses (operator[], add, contains)")`)：
  * 验证向非 Object 节点执行按 Key 查询。
  * 验证向非 Array 节点执行按 Index 访问。
  * 验证向非 Object 节点调用 `add` 函数是否会被安全拦截。
* **深层容错链测试** (`TEST_CASE("Deep nested missing fields safety chain")`)：验证 `json["layer1"]["layer2"].as_int()` 在上层节点不存在时，不触发野指针访问并顺利返回默认值。

### 3.4 动态迭代器保护 (Iterator Safety)

**测试目标**：验证在使用 `keys()` 和 `items()` 遍历未知 JSON 结构时的安全性。

* **正确遍历逻辑** (`TEST_CASE("dynamic object iteration")`, `TEST_CASE("array iteration using items()")`)：验证正常情况下的节点获取。
* **非法遍历防御** (`TEST_CASE("Iterator safety on wrong types")`)：验证当对纯数字或数组调用 `keys()` 时，是否能安全返回空向量。

### 3.5 宏系统压测与边缘情况 (Macro Stress & Edge Cases)

**测试目标**：验证变参宏的扩展能力以及对于非法报文的异常处理。

* **超大结构体映射** (`TEST_CASE("100 parameters serialize and deserialize")`)：验证宏在处理 100 个字段时的编译稳定性与数据无损还原。
* **解析异常捕获** (`TEST_CASE("parse error reporting")`, `TEST_CASE("invalid json")`)：验证对于残缺的 JSON 报文能否拒绝解析，并生成准确的错误偏移量。
* **空报文处理** (`TEST_CASE("empty json")`, `TEST_CASE("parse empty object")`, `TEST_CASE("parse empty array")`)：验证各类空状态节点的表现。

