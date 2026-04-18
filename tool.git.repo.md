---
id: jllcjc5ant1fpbbiattuium
title: Repo
desc: ''
updated: 1775529399898
created: 1774418589169
---

### 常用命令

`repo list` 查看所有项目

```bash
. : products/reader_product_base
app/nvs/cmake : common/cmake_base
app/nvs/third_party : app/third_party
app/nvs/third_party/libjpeg-turbo/libjpeg-turbo : common/libjpeg-turbo
app/nvs_recovery/cmake : common/cmake_base
app/nvs_recovery/third_party : app/third_party

```

`jfxu@xssw-ubuntu:~/test$ repo sync app/nvs` sync模块 

### `git repo sync` 执行成功，但进度为0

输入账户密码成功，但就是进度为0
问题原因：中途曾经输入密码错误，有概率导致后续输入成功，但无法正常进行sync

解决方案：git 账户密码免密设置，规避密码错误情况，通过设置`ssh`或者`git config --global credential.helper store`

## 已经同步代码以后如何更新

### 1. 同步最新代码

在你的工程根目录下执行以下命令，这会更新 `.repo/manifests` 并拉取所有子仓库的最新改动：

```bash
repo sync -j8

### `repo sync` 与 `git pull` 的关系

简单来说：**`repo sync` = `git fetch` + `git rebase` (或 `git merge`)**。

#### 1. 它会重新 Pull 吗？
是的。当你运行 `repo sync` 时，它会执行以下逻辑：
* **如果本地没有代码**：相当于 `git clone`。
* **如果本地已有代码**：
    1. 它会检查远程仓库的更新（`git fetch`）。
    2. 它会尝试将你当前的本地分支更新到远程的最新状态。
    3. 默认情况下，它使用 `rebase` 策略，将你的本地提交“移动”到远程提交之后，以保持提交历史的线性。

#### 2. 为什么不建议手动使用 `git pull`？
在大型工程中（如你目前处理的系统级 C++ 项目），手动在数百个子仓库里执行 `git pull` 是不现实的。
* **全局同步**：`repo sync` 会根据 `manifest.xml` 文件确保**所有**仓库都对齐到特定的版本。如果你只在某一个目录 `git pull`，可能会导致该模块与其他模块的接口/版本不匹配，从而引发 C++ 编译时的链接错误。
* **自动切换**：如果远程 Manifest 修改了某个仓库的路径或分支，`repo sync` 会自动处理这些变更，而 `git pull` 做不到。

#### 3. 特殊情况：本地有改动时
如果你在过去两周内修改了代码且没有提交：
* `repo sync` 可能会因为冲突而跳过某些仓库的更新。
* 此时你需要进入对应的子目录，手动处理：
  ```bash
  git stash        # 暂存改动
  repo sync .      # 仅同步当前目录
  git stash pop    # 恢复改动并解决冲突