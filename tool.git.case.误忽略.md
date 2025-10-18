---
id: oqu20rsdkgzom5xut4hhjrt
title: 误忽略
desc: ''
updated: 1760153483604
created: 1760153421581
---

## 此前由于引用asio和asio_net三方库，包含了git相关文件，导致在进行跟踪时 误操作整个文件夹被忽略，无法进行跟踪

```bash
# 1. 从父仓库 index 中删除子模块引用
git rm --cached src/global/mock/rk_bt_domain_rpc/asio_net1

# 2. 删除 .gitmodules 中对应条目（如果存在）
# 编辑 .gitmodules 文件，删除关于 asio_net1 的 section
# 然后提交
# 非必须
git add .gitmodules
git commit -m "Remove submodule entry for asio_net1"

# 3. 确保目录下的 .git 已经删除
rm -rf src/global/mock/rk_bt_domain_rpc/asio_net1/.git

# 4. 强制添加目录下所有文件
git add -f src/global/mock/rk_bt_domain_rpc/asio_net1/*
git commit -m "Add asio_net1 files as normal directory"

```
