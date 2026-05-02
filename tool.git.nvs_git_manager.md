---
id: 8o5muhetnn3vvjvq2kfc409
title: Nvs_git_manager
desc: ''
updated: 1776499397250
created: 1776499395650
---

# 一、当前团队的 Git 工程管理方案（基于 repo + git）

## 1. 整体架构

当前团队采用的是 **repo + 多 git 仓库** 的管理模式，典型结构如下：

```
repo（统一调度）
│
├── bsp      （板级支持）
├── hal      （硬件抽象层）
├── app      （应用层）
│   ├── core     （子仓库）
│   ├── algos    （子仓库）
│   └── ...
└── product  （产品层）
```

其中：

- 每一个目录通常对应一个独立的 git 仓库
- repo 通过 manifest 文件统一管理所有仓库的版本（commit）

---

## 2. repo 的核心作用

repo 的本质是：

> **对多仓库进行统一版本编排（snapshot 管理）**

### 关键能力：

- 统一管理多个仓库的 commit
- 保证整体工程版本一致
- 一次性同步所有依赖

```bash
repo sync
```

效果：

- 所有仓库同步到 manifest 指定版本
- 保证 bsp / hal / app / algos / core 之间版本匹配

---

## 3. git 的作用（单仓库开发）

在 repo 管理下，每个子目录仍然是独立 git 仓库：

```bash
cd app/algos
git checkout
git commit
git pull
```

👉 git 负责：

- 单仓库开发
- 分支管理
- 提交与合并

---

## 4. 当前开发流程（典型）

### 场景1：同步工程（标准操作）

```bash
repo sync
```

👉 获取一套完整可编译的工程版本

---

### 场景2：开发某个模块（如 app）

```bash
cd app
git checkout -b feature_xxx
```

👉 在单仓库内开发

---

## 5. 当前痛点（核心问题）

### ❗ 问题本质

repo 管理的是：

```text
“全局一致版本（快照）”
```

但开发时：

```bash
cd app
git pull
```

👉 会导致：

```text
app 更新 ✔
algos / core 未更新 ❌
```

---

### ❗ 结果

- 依赖版本不一致
- 编译失败或运行异常
- 难以排查问题

---

## 6. 问题总结

当前方案的特点：

### ✅ 优点

- 多仓库统一版本（强一致性）
- 适合大型工程（嵌入式/系统级）

### ❌ 缺点

- 开发态依赖不同步
- 手动 git pull 容易破坏一致性
- 缺乏依赖约束机制

---

## 7. 推荐工程规范（基于现状优化）

### 1）禁止随意 git pull 子仓库

👉 避免破坏 manifest 一致性

---

### 2）版本升级通过 manifest

```xml
<project name="app" revision="xxx"/>
<project name="algos" revision="xxx"/>
```

```bash
repo sync
```

---

### 3）区分“开发态”和“发布态”

| 状态 | 行为 |
|------|------|
| 发布态 | repo sync（统一版本） |
| 开发态 | git checkout（局部修改） |

---

### 4）建议补充机制（关键）

👉 增加依赖版本管理，例如：

- app 内声明依赖版本（json / 配置）
- 自动同步脚本（对齐 algos / core 版本）

---

# 二、repo 与 git submodule 机制说明

---

## 1. git submodule 是什么？

一句话定义：

> **submodule 是 git 提供的“仓库依赖机制”，用于在一个仓库中引用另一个仓库的特定 commit**

---

## 2. submodule 的核心原理

父仓库记录：

```text
子仓库路径 → 某个 commit
```

例如：

```
app
├── algos（submodule）
├── core（submodule）
```

---

### 关键点：

- submodule 不是复制代码
- 而是“引用 + commit 指针”

---

## 3. submodule 的结构

### `.gitmodules`

```ini
[submodule "algos"]
    path = algos
    url = git@xxx/algos.git
```

---

### 实际版本记录

```bash
git ls-tree HEAD
```

```text
160000 commit abc123 algos
```

👉 表示：

```text
algos = commit abc123
```

---

## 4. submodule 的使用流程

### 1）添加子模块

```bash
git submodule add git@xxx/algos.git algos
```

---

### 2）初始化

```bash
git submodule update --init --recursive
```

---

### 3）切换子模块版本

```bash
cd algos
git checkout xxx_commit
cd ..
git add algos
git commit
```

---

### 4）克隆（带子模块）

```bash
git clone --recursive
```

---

## 5. submodule 的“自动化”说明

### ❗ 默认不会自动拉子仓库

```bash
git clone
```

👉 不会下载 submodule

---

### 需要显式操作：

```bash
git clone --recursive
```

或：

```bash
git submodule update --init
```

---

## 6. submodule 的 .git 机制

子模块目录中的 `.git` 实际是指针：

```bash
cat algos/.git
```

```text
gitdir: ../.git/modules/algos
```

---

真实数据在：

```
.git/modules/algos/
```

---

## 7. submodule 的管理方式

### 双层管理模型：

#### 1）子仓库（独立维护）

```bash
cd algos
git commit
git push
```

---

#### 2）父仓库（记录版本）

```bash
git add algos
git commit
```

👉 记录 commit 指针变化

---

## 8. 是否支持“自动最新版本”？

理论支持：

```bash
git submodule update --remote
```

但：

### ❌ 不推荐原因：

- 不可控
- 不可复现
- 工程不稳定

---

👉 标准做法：

```text
始终固定 commit
```

---

## 9. repo vs submodule（核心区别）

| 维度 | repo | submodule |
|------|------|----------|
| 层级 | 工程级 | 仓库级 |
| 控制方式 | manifest（集中） | 父仓库（分散） |
| 管理范围 | 多仓库 | 单仓库依赖 |
| 结构 | 平铺 | 嵌套 |
| 一致性 | 强 | 弱 |

---

## 10. 是否可以混用？

### ✔ 可以（技术上）

但必须遵守：

> **同一依赖只能由一个系统管理**

---

### ✔ 推荐混用方式

```
repo 管：
  app / algos / core

app 内 submodule：
  第三方库（repo 不管理）
```

---

### ❌ 禁止方式

```
repo 管 algos
app 又 submodule algos
```

👉 会导致版本冲突

---

## 11. 最终结论

### repo：

> 多仓库“版本编排系统”（工程级）

---

### submodule：

> 单仓库“依赖声明机制”（代码级）

---

### 本质关系：

```text
repo：解决“整体版本一致性”
submodule：解决“仓库之间依赖关系”
```

---

### 选择建议：

- 大型工程：优先 repo
- 小型依赖：可用 submodule
- 禁止重复管理同一仓库

---