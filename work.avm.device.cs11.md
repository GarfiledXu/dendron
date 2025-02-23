---
id: 2gow7rn2r4gt4upqesxpywe
title: Cs11
desc: ''
updated: 1727313944227
created: 1727242061651
---

#### cs11 操作命令
kx11
```bash


adb root && adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\out\out_700\cs11_gui_tool\cs11_gui_tool /share && adb shell sync && adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\src\module\geely_KX11-A3-L1\lib\libarcsoft_avm_algo.so /share && adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\src\module\geely_KX11-A3-L1\lib\libarcsoft_avm_sdk.so /share && adb shell sync exit


# cp -f /var/share/cs11_gui_tool /var/avm/ && cp -f /var/share/libarcsoft_avm_algo.so /var/avm/ && cp -f /var/share/libarcsoft_avm_sdk.so /var/avm/


mount -uw /; slay sva.upgrade; slay sva.adapter; slay sva.avmadapter; hamctrl -stop; slay sva.winmgr; slay sva.doip; export LD_LIBRARY_PATH=/var/avm/:$LD_LIBRARY_PATH; cp -f /var/share/cs11_gui_tool /var/avm/ && cp -f /var/share/libarcsoft_avm_algo.so /var/avm/ && cp -f /var/share/libarcsoft_avm_sdk.so /var/avm/

cd /var/avm/ && chmod 777 * && ./cs11_gui_tool gui

cd /var/avm/ && chmod 777 * && ./cs11_gui_tool render_test

```
##### cmd
```bash
adb root

adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\out\out_700\cs11_gui_tool\cs11_gui_tool /share


adb shell sync exit
```
```bash
#基础库
adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\src\module\geely_cs11\lib\libarcsoft_avm_sdk.so /share
adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\src\module\geely_cs11\lib\libArcsoftAVM.so /share

adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\src\module\geely_KX11-A3-L1\lib\libarcsoft_avm_algo.so /share
adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\src\module\geely_KX11-A3-L1\lib\libarcsoft_avm_sdk.so /share

adb shell sync exit

断电重启

断电后拔掉板子上的ADB线，然后上电后电流上去的时候再把ADB接上
否则屏幕可能亮不了，一直处于黑屏背光状态
```
##### qnx
```bash
## 运行程序
##cp exe
cp  -f /var/share/cs11_gui_tool /var/avm/
##cp lib
cp -f /var/share/libarcsoft_avm_algo.so /var/avm/
cp -f /var/share/libarcsoft_avm_sdk.so  /var/avm/

## screen script
sh /var/avm/winmgr.sh

### run
cd /var/avm/ && chmod 777 * && ./cs11_gui_tool gui
```
##### coredump
```bash
/dev/shmem//cs11_gui_tool.core
```



#### winmgr.sh content
```bash
#!/bin/sh
## 1
hamctrl -stop
slay sva.winmgr
```
```bash
#!/bin/sh
##２
slay sva.upgrade
y
y
hamctrl -stop
slay sva.winmgr

```

```bash
#kx11
mount -uw /
slay sva.upgrade
yy
slay sva.adapter
slay sva.avmadapter
hamctrl -stop
slay sva.winmgr
slay sva.doip
cd  /var/share/
cp avm_test.out    /var/avm/
cp libarcsoft_avm_sdk.so   /var/avm/
cp libarcsoft_avm_algo.so /var/avm/
cd  /var/avm/
export LD_LIBRARY_PATH=/var/avm/:$LD_LIBRARY_PATH
chmod 777 ./avm_test.out
./avm_test.out & slog2info -w | grep ArcSoft

```