---
id: 1juy4rj4kwnu1fkbvyzy6k5
title: '11'
desc: ''
updated: 1710320250798
created: 1710120754162
---

#### must
- [x] 采集工具代码上传
- [x] 向陈老师发送 websocket
- [ ] 验证笔记本英伟达硬件编码
- [x] cs11 调试验证

#### this week plan
1. byd two 项目上平台
2. 采集工具英伟达笔记本硬件编码验证调试
3. cs11 工具调试运行完毕，交付
4. 瑞驰曼

#### cs11 操作命令
##### cmd
```bash
adb root

adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\out\out_700\cs11_gui_tool\cs11_gu
i_tool /share


adb shell sync exit
```
```bash
#基础库
adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\src\module\geely_cs11\lib\libarcs
oft_avm_sdk.so /share

adb push \\wsl$\Ubuntu\home\xjf2613\code-space\qnx\pe-testapp_avm_geely_cs11\src\module\geely_cs11\lib\libArcs
oftAVM.so /share

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
cp -f /var/share/libArcsoftAVM.so /var/avm/
cp -f /var/share/libarcsoft_avm_sdk.so  /var/avm/

## screen script
sh /var/avm/winmgr.sh

### run
cd /var/avm/ && chmod 777 * && ./cs11_gui_tool
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



##### pragress
version: 1.0.1 
90%
- [x] 车模透明度返回错误码 > 解决，只支持0 和 1，中间值报错
- [ ] 车模颜色没有返回错误码，但似乎看不到效果
- [ ] lightinfo设置了以后没有看到效果
- [x] 最后是起了一个线程保持脉冲差值输入，轮胎没有动