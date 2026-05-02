---
id: hztc7z0o9w08mu9gwnrcyup
title: 架构与功能设计
desc: ''
updated: 1777034939975
created: 1776436364412
---

# Debug Client 工程架构设计与选型文档

## 1. 架构核心原则

采用单向依赖分层架构（Clean Architecture）。Core层为绝对核心，CLI与GUI为平级的调用方，Pytest作为独立的工程质量保障层，剥离所有业务控制流。

## 2. 工程目录树 (src-layout)

```text
debug_client/
├── pyproject.toml          # 现代化工程与依赖配置
├── src/
│    ├── __init__.py
│    ├── core/           # 核心业务与协议层
│    │   ├── config.py
│    │   ├── logger.py
│    │   ├── network/
│    │   └── protocol/
│    ├── cli/            # 命令行交互层
│    │   └── main.py
│    └── gui/            # 图形交互层
│           └── main.py
└── tests/                  # 测试层
    ├── conftest.py
    ├── test_core/
    └── test_cli/
```

## 3. 模块划分与第三方库选型

### 3.1 核心库 (Core)

**职责**: 封装所有 TCP 通信、自定义协议解析、全局配置加载与日志输出。绝不包含任何 `print` 或 UI 阻塞逻辑。
**推荐第三方库**:

* **`loguru`**: 替代标准库 `logging`。开箱即用的彩色终端输出与异步文件轮转，适合调试工具。
* **`pydantic`**: 强大的数据验证与配置管理。用于定义协议数据类（Data Model）和加载外部 YAML/JSON 配置文件，自带类型断言。
* **`construct`**: （可选）如果自定义协议涉及复杂的 C-struct 字节对齐、位域操作，使用此库进行声明式二进制组包/解包，比原生的 `struct` 模块更直观。
* **`PyYAML`**: 配合 Pydantic 读取本地配置文件。

### 3.2 命令行模块 (CLI)

**职责**: 提供轻量级的纯终端测试入口。解析参数，实例化 Core 层组件并触发网络请求，将结果格式化输出。
**推荐第三方库**:

* **`typer`**: 基于 Python 类型提示 (Type Hints) 的现代 CLI 框架。代码侵入性极小，自动生成 Help 文档，比 `argparse` 更易维护，底层基于 `Click`。
* **`rich`**: 终端富文本输出库。用于在 CLI 模式下打印漂亮的表格（如协议字段解析结果）和状态面板。

### 3.3 图形模块 (GUI)

**职责**: 提供可视化操作面板。作为 Core 层的视图（View），维护自身的事件循环，将耗时的网络请求丢入后台线程。
**推荐第三方库**:

* **`dearpygui`**: 当前主力。C++ 编写的 GPU 渲染立即模式 GUI，适合快速构建高性能的工程调试界面。
* **注意**: 架构上应预留接口，保证后续向 `PyQt6` / `PySide6` 迁移时，只需替换 `src/debug_client/gui/` 目录下的内容，不影响 Core 层。

### 3.4 测试模块 (Tests)

**职责**: 开发者维度的白盒/黑盒测试。验证 Core 层的数据解析逻辑，以及 CLI 层的命令路由逻辑。
**推荐第三方库**:

* **`pytest`**: 核心测试框架。
* **`pytest-mock`**: 提供 `mocker` fixture，用于在断网或无下位机时，Mock (模拟) TCP 的收发返回值。
* **`pytest-asyncio`**: 如果你的网络层最终决定采用 `asyncio` 实现，必须引入此库来运行异步测试用例。

## 4. 构建与环境管理选型

**推荐第三方库**:

* **`poetry`** 或 **`uv`**: 摒弃传统的 `requirements.txt`。使用 `pyproject.toml` 统一管理依赖版本、虚拟环境以及项目入口点。
  * *配置示例*: 可以直接将 CLI 挂载为系统命令。在终端输入 `debug-cli send 0x01` 即可运行，彻底摆脱 `python -m src.debug_client.cli.main` 的繁琐调用。

# Core 核心模块详细设计与选型补充

将 Core 层拆分为 `logger`、`config`、`event`、`network`、`protocol` 是非常标准的底层抽象。针对你的思考，以下是精炼的边界划分与库选型建议：

## 1. Logger (日志管理)

**功能边界划分**：

* **内聚在 Core 的功能**：日志级别的统一定义（Debug/Info/Warning/Error）、日志格式化（时间戳、模块名、行号）、本地文件的轮转与压缩保存、终端 (stdout) 的色彩输出控制。
* **属于 GUI/CLI 的功能**：界面上的“日志显示面板”、按关键字搜索日志的输入框、界面上的级别筛选下拉菜单。
**架构解耦设计**：
Core 层只负责产生数据流，不负责“怎么画在屏幕上”。通过暴露 `add_sink(callback)` 接口，让 GUI 主动向 Core 注册。
**库选型**：
* **`loguru`**：绝对首选。它天然支持多 Sink 模型。
  * 默认配置：输出到 `sys.stderr` 和 `logs/` 目录下的滚动文件。
  * GUI 接入：在 GUI 模块初始化时，执行 `logger.add(gui_append_text_callback, level="INFO")`，将日志直接推入 GUI 的文本框组件。

## 2. Event (事件总线/消息分发)

**核心解答**：你完全需要这个机制。在 Qt 中，这被称为**信号与槽 (Signals and Slots)**。由于 Core 层必须对 GUI 绝对无感知（不能 `import gui`），当底层网络收到数据时，必须通过 Event 机制通知上层。
**架构解耦设计**：
采用 **Pub/Sub（发布/订阅）模式**。

* **Network 模块** 收到心跳包，触发 `EventBus.emit('tcp_received', data)`。
* **Protocol 模块** 监听 `tcp_received`，进行解包。解包成功后触发 `EventBus.emit('protocol_parsed', packet)`。
* **GUI 模块** 监听 `protocol_parsed`，更新界面仪表盘或状态灯。
**库选型**：
* **纯手写 Observer**：维护一个 `dict[str, list[Callable]]` 即可满足初期需求，代码不到 50 行，最轻量。
* **`blinker`**：轻量级、高度优化的 Python 信号/事件派发库（Flask 底层使用）。若后续全面迁移到 PyQt，GUI 层的交互可换用 PyQt 自带的 `pyqtSignal`，但 Core 层仍建议保留纯 Python 的事件总线以维持框架中立。

## 3. Config (配置管理)

**核心解答**：要实现 GUI 的直接映射和编辑生效，配置类必须具备**模式定义 (Schema)** 和 **类型校验** 的能力。
**架构解耦设计**：

* 不要使用松散的 `dict`。构建强类型的配置数据类。
* 修改流向：GUI 读取 Config 对象的属性生成界面 -> 用户在 GUI 修改 -> GUI 将新值回写给 Config 对象 -> 调用 `Config.save()` 刷入本地文件 -> 触发 `EventBus.emit('config_reloaded')` 让网络等模块感知变化。
**库选型**：
* **`pydantic`**：极度契合此需求。它可以通过定义类属性（如 `ip: str`，`port: int`）自动处理数据校验，并且可以无缝序列化/反序列化为 JSON 或 dict。配合 `PyYAML` 可实现 YAML 文件的持久化。GUI 层可以直接反射 Pydantic 模型的 `model_fields` 来自动生成对应的输入框。

## 4. Network (网络通信)

**设计考量**：
作为上位机工具，TCP 的断线重连、心跳保活、异步非阻塞是关键。
**库选型与实现策略**：

* **标准库 `socket` + `threading` + `queue`**：最稳妥的方案。开启一个独立的后台接收线程，将收到的字节流压入线程安全的 `queue.Queue`，再由主循环或事件池去消费。这种方式与 C/C++ 嵌入式开发的思维最一致，便于你后续用 C++/Qt 重构。
* 不建议初期直接上 `asyncio`，它具有传染性，会让整个架构的函数都变成 `async/await`，增加与 Dear PyGui 这种同步 UI 框架融合的复杂度。

## 5. Protocol (协议解析)

**设计考量**：
TCP 是流式协议（Stream），存在**粘包**和**半包**问题。协议模块的首要职责是“分帧 (Framing)”。
**边界划分**：

* 实现一个环形缓冲区 (Ring Buffer) 或累加字节数组。
* 每次从 Network 接收到碎片数据，都丢入缓冲区。
* Protocol 模块循环检查缓冲区，根据你自定义协议的**包头标识 (如 0xAA 0x55)** 和 **长度字段**，切割出完整的帧，再进行 CRC 校验和业务数据解包。
**库选型**：
* **标准库 `struct`**：适用于固定长度的简单协议结构。
* **`construct`**：如果你的底层通信包含复杂的位域 (Bitfields)、变长数组或条件字段，这个库可以用声明式的方式完美定义 C-struct 级别的协议，自动完成 bytes 到 Python dict 的双向转换。
