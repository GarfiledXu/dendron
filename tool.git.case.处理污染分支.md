---
id: jghlvmmu43525c6f1jqyy5h
title: 处理污染分支
desc: ''
updated: 1758723840555
created: 1758722724065
---

### 处理污染分支

```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git branch
  develop
  develop_smart_1106
  develop_smart_1106_bak
* develop_smart_1106_cur
  develop_smart_1106_saas
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git branch backup_develop_smart_1106_cur_before_split
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git fetch origin
BASE=$(git merge-base develop_smart_1106_cur origin/develop_smart_1106_cur)
echo "base commit: $BASE"
git log -1 --oneline $BASE
remote: Enumerating objects: 24, done.
remote: Counting objects: 100% (24/24), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 13 (delta 11), reused 0 (delta 0), pack-reused 0
展开对象中: 100% (13/13), 10.52 KiB | 633.00 KiB/s, 完成.
来自 https://git.libawall.com/zhouhb/aplus_2
   9d82e07..8230b32  develop    -> origin/develop
base commit: e5367a915f938a136053a0820ca48f01da1921e5
e5367a9 (develop_smart_1106_saas) [middle][code] Fix the delay of the command main thread caused by the voice playback command not being executed in the background
```

## cherry-pick 和 rebase的区别

1. cherry-pick 是复制，rebase是搬运，迁移，会改变历史提交
2. cherry-pick+rebase会保险一些
3. 目前遇到的问题，rebase需要重新解决冲突