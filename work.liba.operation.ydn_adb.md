---
id: jl6ondpcmrynfz4i18vknec
title: Ydn_adb
desc: ''
updated: 1742890647481
created: 1742808681219
---

### windows adb prepare

```bash
adb kill-server
adb -a -P 5037 nodaemon server

adb forward --list
adb forward tcp:6001 tcp:6001
# adb forward --remove-all
```

### wsl adb env

```bash
export ADB_SERVER_SOCKET=tcp:172.19.192.1:5037

```

### wsl gdb server

```bash

```

### run jpeg test

```bash
adb push  /home/xjf1127/codespace/git_down/new_aplus/test/takephoto/src/target/build-indep_call_takephoto-rk1126_arm_linux-Debug/indep_takephoto /oem/xjf1127/

rm indep_takephoto

cd /oem/xjf1127/ && chmod 777 * && ./indep_takephoto --save_pic_dir_path=/userdata/save_pic --run_case_id=3 --takephoto_interval_ms=1000

cd /userdata/save_pic/
ls -l
rm -rf ./*

adb pull /userdata/save_pic/* /home/xjf1127/codespace/git_down/new_aplus/test/takephoto/data/save_pic_
```

### run rkmedia_muxer_test

```bash
[root@RV1126_RV1109:/oem/xjf1127]# ./rkmedia_muxer_test -a /etc/iqfiles -e h265 -w 3840 -h 2160 -d rkispp_scale0
```
