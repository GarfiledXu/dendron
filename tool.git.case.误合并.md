---
id: 2zm1ofzg8dnanhi9ewk8654
title: 误合并
desc: ''
updated: 1758779743978
created: 1758724888980
---

## 误合并

将本地分支与develop分支最新提交合并，结果发现合并的提交太靠前，是未经过测试的，需要合并低几个版本的提交

## 方案

1. 这个分支10之后的提交我不要了，提交10对应新分支develop_smart_1106_saas，那我就把develop上的3.11重新合并上去，但是这样困难的地方，10以后的除了合并的部分还有我各种修改的记录，只能手动拷上去了
2. 使用cherry-pick，将提交里挑选改动，应用到分支上，但是改动内容有可能包含merge的冲突内容，所以需要判断待pick的提交中，哪些是非merge提交，只将非merge提交改动拷贝过去

```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git log --oneline --no-merges develop_smart_1106_cur
8db2e6a [high][code] fix the update fireware logic error
8c1bbb6 多盖预警增加盖印时间与定位信息
b4ca028 [high][code] add bluez process rpc call version
f59ebfc 兼容旧蓝牙BT24-S模块连接-2
b45db9c 兼容旧蓝牙BT24-S模块连接
d9de92d 修复SN低概率写入操作失败问题&wpa配置异常wifi不可用问题&老设备(2.6之前不带wifi)不能联网MQ服务
2ec2c10 (tag: SaaS_2.5.8) 增加对小程序控制端因参数不足导致的应用崩溃的认证协议处理；
f30426e 调整打点视频上传地址增加端口指定；变更打包脚本内容；
90a90a6 变更saas打点分割视频传输地址；
d431b80 关闭影像开关不开启拍照程序
04f9b2b 修复关闭影像管理， 在线特权模式，设备还会拍照问题&认真一审一印等待中会导致设备自动关机
2da6cef 打点管道信息增加视频录制中判断；
225647a  修复设备日志上传失败情况但本地日志被误删除的问题&升级固件时设备屏幕图标与印章名重叠
d795d87 调整连续用印按键伸出印章；
a1012a2 天玺取消假关机功能新需求&连接状态下的关机优先级新需求&优化镜像下载显示逻辑
c44df07 调整因控制的异常退出、异常连接导致的视频起止时间记录异常问题；调整风险打点认证模式下的打点判断；
0a6392a 修复设备处于快捷/特权登录情况下，使用app登录设备回导致设备退登和跳转到流程盖印中问题
6b57049 增加认证协议打点判断参数；
6685f53 [high][code] fixed wifi connected prompt the not work
f626bea [middle][code] supplement the daemon_service first check ydn-rk process way, instead of pidfile
66c002d [high][code] fixed daemon_service pid extract, instead of pidfile
544abfc 增加支持无IOT配置运行功能&其他问题
e5367a9 (develop_smart_1106_saas) [middle][code] Fix the delay of the command main thread caused by the voice playback command not being executed in the background
da24977 增加连续用印上解锁语音提示；增加有线连接标志；固定BLE蓝牙mac地址；增加BLE蓝牙mac地址上传；
1f06731 [low][code] modify the low power level -100
fa41d95 [high][code] fix aplus add new 4g info extract and

# git log --no-merges 的作用是 排除 merge commit 本身，也就是说它不会显示 git merge 产生的那条 merge commit。
# 但是 merge 带来的其他 commit（被 merge 进来的历史 commit）仍然会显示，只要这些 commit 在你当前分支的历史中。
# 所以，还需要继续过滤，得到本分支
```

!!! merge操作和改动混为一个提交导致的问题，如果merge存在问题，则不好进行回滚，此时merge commit中包含了非冲突内容的改动, 最新提交就是merge的分支，包含了非冲突的改动

在10-14之前的提交进行pick，然后15部分手动把非冲突内容手动添加，然后再合并3.11
cherry-pick 10~14 的提交
通过patch机制处理第15提交的非冲突内容

```bash
# 对比 15 与 14 的差异，只生成修改部分
git diff <commit-14> <commit-15> > change15.patch
生成文件太大了
说明提交 15 本身就包含大量内容，尤其可能是二进制文件或者 merge 时手动加的大量改动
patch不适用

改用git restore，直接进行文件恢复
```

使用restore进行提取

```bash
git checkout develop_smart_1106_saas

git restore --source=c2106295c43a0a3 --worktree --staged \
  src/global/bt_bluez \
  src/global/mock \
  src/global/wifi_wpa \
  src/global/global_smart.pri \
  src/global/global.pri \
  packages/packages.sh \
  src/global/DeviceIo/NetLinkWrapper.h \
  src/global/blewifi.c \
  src/global/blewifi.h

```

## 同步完成后如何清理 develop_smart_1106_cur 多余的提交

```bash
## 使用checkout备份和tag备份的区别，checkout还是存在分支线，只是名称不同，但tag是固定提交
git checkout develop_smart_1106_cur
git tag develop_smart_1106_cur_15_bak HEAD

git fetch origin
git checkout develop_smart_1106_cur
git reset --hard origin/develop_smart_1106_cur


这里会丢掉 11~15 的提交，所以之前先打了 cur_backup_15 标签。

git reset --hard origin/develop_smart_1106_cur
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git reflog
ce3d0f9 (HEAD -> develop_smart_1106_cur, origin/develop_smart_1106_cur) HEAD@{0}: reset: moving to origin/develop_smart_1106_cur
c210629 (tag: develop_smart_1106_cur_15_bak, backup_develop_smart_1106_cur_before_split) HEAD@{1}: checkout: moving from develop_smart_1106_saas to develop_smart_1106_cur
bdde739 (develop_smart_1106_saas) HEAD@{2}: commit (merge): Merge commit '2ec2c10bbb0e47f742a45' into develop_smart_1106_saas

直接reset --hard后发现，分支图只剩下一个最新的远程提交，是一个点了,需要先撤回操作 c2106295c43a0a39e061c 再reset到共同祖先 e5367a915f938
git reset --hard develop_smart_1106_cur_15_bak
git reset --hard develop_smart_1106_cur_15_bak

完成reset，同步远程，从此不再干涉此分支
git fetch origin
git checkout develop_smart_1106_cur
# git reset --hard origin/develop_smart_1106_cur
git pull

最后清理一下工作区中未跟踪文件
vscode 插件的discard all charge或者
git reset --hard
git clean -fd


发现有文件丢失
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git log -- src/global/mock/rk_bt_domain_rpc/asio_net
commit 907af0d5c4ca8587407f42c21fd70839e2fe43d5
Author: xujiafei <xujiafei@ydn.cn>
Date:   Wed Sep 17 20:16:13 2025 +0800

    [high][code] add bluez process rpc call version
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git log --all src/global/mock/rk_bt_domain_rpc/asio_net/rpc_core.pri
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git log --all -- src/global/mock/rk_bt_domain_rpc/asio_net/rpc_core.pri
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git log --all -- src/global/mock/rk_bt_domain_rpc/asio_net/rpc_core.pri
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git rev-list -n 1 --all -- src/global/mock/rk_bt_domain_rpc/asio_net/rpc_core.pri

怀疑是内容没有同步，需要查看其他分支上该文件内容
查看某个文件在其他分支的内容
git show <branch_name>:<path/to/file>
git show backup_develop_smart_1106_cur_before_split:src/global/mock/rk_bt_domain_rpc/rk_bt_domain_rpc_client.pri

```
