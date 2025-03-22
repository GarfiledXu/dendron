---
id: uqjfo1fwbqt961sb25a6cue
title: Search
desc: ''
updated: 1741694015612
created: 1741693455591
---

### 搜索栏 file to include栏，如果通过路径表达式来搜索某个文件夹下所有符合通配符的文件内容，而不是仅仅当前单层文件夹下符合通配符的文件的内容

```bash
###首先在目标文件夹后面加 /**
### 然后在**/后面补充对应的通配符即可
### eg:查找/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/bulid/qt5base-5.14.2整个文件夹内所有文件名匹配通配符的文件内容
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/bulid/qt5base-5.14.2/**/*.cmake

```

### 如何匹配多个文件名?

```bash
### 使用花括号和逗号分隔即可支持，匹配两种
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/bulid/qt5base-5.14.2/**/*.{pro,pri,prl}

### 或者使用逗号分隔两个独立路径
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/bulid/qt5base-5.14.2/**/*.pro, /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/bulid/qt5base-5.14.2/**/*.pri
```
