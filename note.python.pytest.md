---
id: crabp1us0ux2rivdpnfbugo
title: Pytest
desc: ''
updated: 1775720556994
created: 1775706650275
---

# Pytest 集成指南

本文档旨在说明如何在 Python 项目中引入 `pytest` 自动化测试框架，并配合 `loguru` 实现工业级的日志追踪与测试管理。

---

## 1. 框架核心依赖包

在 Python 环境中，通过以下包构建测试脚手架：

```bash
pip install pytest       # 核心测试驱动框架
pip install loguru       # 增强型日志系统（支持彩色输出与异步记录）
pip install pytest-html  # 可选：用于生成可视化 HTML 测试报告
```

## 2. 自动化测试工程结构（标准推荐）

为了使 `pytest` 能够自动识别配置并实现逻辑解耦，建议采用以下目录布局：

```text
project_root/
├── pytest.ini          # 框架全局配置文件（控制运行参数、实时日志开关）
├── conftest.py        # 测试上下文/钩子文件（管理连接初始化、全局变量、Fixture）
├── src/               # 业务代码模块（如协议封装、API调用）
└── tests/             # 测试用例存放目录
    ├── test_module_a.py
    └── test_module_b.py
```

## 3. 关键配置文件解析

### 3.1 pytest.ini：定义运行行为
该文件是 `pytest` 的大脑，通过它可以避免在命令行重复输入繁琐参数。

```ini
[pytest]
# 常用参数说明：
# -s: 允许在控制台显示 print 和 logger 的输出内容
# -v: 增加输出的详细程度
# --color=yes: 在任何环境下强制开启彩色显示（对 VS Code 极其重要）
addopts = -s -v --color=yes

# 开启实时日志（Live Logging）：在用例执行过程中即时看到日志，而不是等结束
log_cli = true
log_cli_level = INFO
log_cli_format = %(asctime)s [%(levelname)8s] %(message)s
log_cli_date_format=%H:%M:%S
```

### 3.2 conftest.py：管理环境生命周期
该文件通过 `Fixture`（固件）机制，自动处理测试前后的 Setup（如建立连接）和 Teardown（如断开连接）。

```python
import pytest
import os, sys
from loguru import logger

# 环境变量设置：强制色彩渲染，确保 GUI 面板兼容性
os.environ["FORCE_COLOR"] = "1"

@pytest.fixture(scope="session")
def session_resource():
    """
    scope="session" 表示整个测试过程中只运行一次
    yield 之前是初始化（Setup），yield 之后是清理（Teardown）
    """
    logger.info("正在初始化测试会话...")
    resource = {"status": "connected"} # 示例：此处可替换为真实的设备连接对象
    yield resource
    logger.info("正在关闭测试会话...")
```

## 4. 常见使用方式与指令

### 4.1 命令行启动执行
在工程根目录下使用终端进行操作：

```bash
# 执行全部测试用例
pytest

# 执行特定目录或文件
pytest tests/test_backup.py

# 【调试神器】通过关键字运行特定用例
# 例如只运行函数名包含 "manifest" 的用例
pytest -k "manifest" -vs

# 生成单文件 HTML 报告
pytest --html=report.html --self-contained-html
```

### 4.2 编写测试用例的规范
1.  **文件名**：必须以 `test_` 开头（如 `test_api.py`）。
2.  **函数名**：必须以 `test_` 开头。
3.  **断言**：直接使用标准 `assert` 表达式，无需特殊 API。

```python
def test_example_logic(session_resource):
    # session_resource 会自动获取 conftest.py 中定义的 fixture
    assert session_resource["status"] == "connected"
```

## 5. 示例：针对下位机测试的脚手架集成

作为实战示例，我们可以将协议栈封装在 `src/` 中，并在 `tests/` 中调用：

1.  **协议封装**：在 `NVSClient` 中处理 TCP 粘包与报文拼接。
2.  **业务封装**：在 `FileIOTester` 中实现 MD5 校验与上传下载循环。
3.  **日志显示**：利用 `loguru` 的自定义级别（如 `SEND`/`RECV`）实时监控报文流动，配合 `pytest -vs` 参数实现可视化调试。

---

## 6. 常见问题 (FAQ)

- **为什么 VS Code 看不到彩色日志？**
  需确保 `pytest.ini` 中配置了 `--color=yes`，且 `conftest.py` 中 `logger.add` 时开启了 `colorize=True`。
- **如何处理测试失败？**
  `pytest` 默认会停留在失败位置。使用 `pytest --lf`（last failed）可以仅重新运行上次失败的用例。