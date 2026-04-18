---
id: oj3wzknkfkgiak9e7mroojs
title: 分支切换与文件状态处理指南
desc: ''
updated: 1776395521028
created: 1776395518472
---

# Git 分支切换与文件状态处理指南

---

# 一、核心概念

---

## 1. 三个区域

```text
工作区（Working Directory）
    ↓ git add
暂存区（Staging Area / Index）
    ↓ git commit
提交区（Repository / HEAD）
```

---

## 2. 文件类型定义

---

### 未跟踪（untracked）

- 从未被 `git add` + `git commit`
- Git 不管理

---

### 被跟踪（tracked）

- 至少被 `git commit` 过一次
- 或已经执行过 `git add`

---

👉 结论：

```text
git add = 开始被跟踪
git commit = 写入历史
```

---

## 3. 被跟踪文件的状态

| 状态 | 含义 |
|------|------|
| clean | 与 commit 一致 |
| modified | 已修改但未 add |
| staged | 已 add |
| committed | 已提交 |

---

## 4. 状态关系

```text
新文件 → untracked
        ↓ git add
tracked + staged
        ↓ git commit
tracked + committed
```

---

# 二、分支切换行为

---

## 1. 切换分支本质

```text
git checkout xxx
```

👉 本质：

```text
用目标分支的提交快照替换当前工作区（仅作用于 tracked 文件）
```

---

## 2. 不同文件类型的行为

| 文件类型 | 切换分支行为 |
|----------|-------------|
| tracked（已提交） | 按目标分支替换 |
| tracked（modified / staged） | 可能被拒绝或带过去 |
| untracked | 保留，不处理 |

---

## 3. 特殊情况

---

### 未提交修改（modified / staged）

- 如果切换会覆盖 → ❌ 拒绝切换
- 如果不冲突 → ✔ 带到新分支

---

### 未跟踪文件（untracked）

- ✔ 不会删除
- ✔ 会保留在新分支
- ❗ 可能污染其他分支

---

### 路径冲突

如果：

```text
当前分支：a.cpp（untracked）
目标分支：a.cpp（tracked）
```

👉 切换时：

```text
❌ 报错（避免覆盖未跟踪文件）
```

---

# 三、问题场景

---

当前状态：

```text
分支：feature/xxx

未暂存修改（tracked）：
  修改 .h / .c 文件

未跟踪文件（untracked）：
  .tours/
  .vscode/
  临时文件若干
```

---

需求：

```text
保留：
  .tours/
  .vscode/

删除：
  其他所有修改和临时文件

然后切换到 dev 分支继续开发
```

---

# 四、操作流程

---

## Step 1：丢弃所有已跟踪文件修改

```bash
git restore .
```

---

## Step 2：删除未跟踪文件（排除指定目录）

```bash
git clean -fd -e .tours/ -e .vscode/
```

---

## Step 3：确认状态

```bash
git status
```

期望结果：

```text
working tree clean
```

或仅剩：

```text
Untracked:
  .tours/
  .vscode/
```

---

## Step 4：切换分支

```bash
git checkout dev
```

---

## Step 5：从 dev 创建新分支

```bash
git checkout -b feature/xxx
```

---

# 五、关键原则

---

## 1. 切换分支只处理 tracked 文件

```text
tracked → 被切换（按目标分支）
untracked → 保留不变
```

---

## 2. git add 的意义

```text
untracked → tracked
```

---

## 3. git commit 的意义

```text
将暂存区写入历史（生成快照）
```

---

## 4. 分支切换的安全前提

```text
working tree clean
```

---

# 六、推荐习惯

---

## 1. 切换分支前

```bash
git status
```

---

## 2. 丢弃改动

```bash
git restore .
git clean -fd
```

---

## 3. 预览删除内容

```bash
git clean -fdn
```

---

## 4. 忽略本地文件

```bash
echo ".vscode/" >> .gitignore
echo ".tours/" >> .gitignore
```

---

# 七、一句话总结

---

```text
Git 只管理被跟踪文件，分支切换本质是切换提交快照；
未跟踪文件不会被处理，需要手动清理或忽略。
```

---