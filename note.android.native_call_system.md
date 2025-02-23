---
id: vbn7sj6avwe4y0ot101g8xe
title: Native_call_system
desc: ''
updated: 1715671709653
created: 1715671279080
---

#### 通过native程序调用system命令执行 apk 相关的操作
```bash 

### 安装 apk
pm install <pkg_name>
### 卸载
pm uninstall <pkg_name>
### 启动
am start -n <pkg_name>/<entry_activity_name>
### 强制停止
am force-stop <pkg_name>

```