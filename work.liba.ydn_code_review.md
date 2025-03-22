---
id: 3982pwlvq3rqqmsq87px66h
title: Ydn_code_review
desc: ''
updated: 1742356244278
created: 1742117916481
---

### state machine



### project code module analysis

### 数据传递的链路和执行线程

### keyword
sysInfo.phtotgraphicId

verifyOk ?
1. 决定了设备上锁与用户绑定? 还是单单是指纹验证通过?


### 视频上传
相关数据结构

## Network 模块
4gworker [thread]

### connect relationship
**蓝牙数据的处理** 诡异之处，模块关系是自顶向下的，多层信号转递，将底层回调数据传递出去，仍到rk_pollevent中的蓝牙数据处理, 阅读代码时就非常诡异，必须从下向上梳理才能明白信号传递规则
[wlanworker rk_wifi_bt_callback thread] receive_data -> emit WlanWorker::recvBtData(cmd, len);
[wlanworker event loop] &WlanWorker::recvBtData-> this, &NetworkModule::recvBtData
[rk_poll thread] &NetworkModule::recvBtData->&rk_pollevent::BtHandleMsg ->anlasyBTmsg

**mqtt协议的处理**


### wlanwork
RK_BLEWifiInit -> 注入 callbackRecvBleData,callbackRecvBleState, callbackRecvWifiStatus

在callbackRecvBleData [中pthis->handleBtDownloadAppDatas(recvData, len);]或者pthis->handleBTCommand(recvData, len);[[处理命令，而命令中包含了`WIFI`相关设置，比如WIFILIST，WIFISETTING，进行wifi的add/ del/ switch]] [[emit recvBtData(cmd, len)(&NetworkModule::recvBtData);->&rk_pollevent::BtHandleMsg]]
**networkmodule中的处理包含了wifi的配置，以及发送除wifi配置意外的蓝牙命令到rk_pollevent**

在callbackRecvBleState 中会进行ui界面网络状态的蓝牙状态更新 [[emit pthis->updateNetworkStatus(STA_BT, pthis->isBtConfig);]]

[wlanworker rk_wifi_bt_callback thread] receive_data -> emit WlanWorker::recvBtData(cmd, len);
[wlanworker event loop] &WlanWorker::recvBtData-> this, &NetworkModule::recvBtData
[rk_poll thread] &NetworkModule::recvBtData->&rk_pollevent::BtHandleMsg ->anlasyBTmsg

callbackRecvWifiStatus 中会进行ui界面网络状态的蓝牙状态更新 [[emit pthis->updateNetworkStatus(STA_WIFI, false);]]
#### 构造
1. wpa的服务配置和重启
2. 


### debug command
环境变量设置
```bash

### windows
adb forward tcp:6001 tcp:6001
adb forward --list
adb forward --remove-all

adb kill-server
adb -a -P 5037 nodaemon server

### adb shell
killall daemon_service
killall ydn-rk
killall gdbserver
chmod 777 * && cd /oem/xjf1127/ && ./gdbserver :6001 ./ydn-rk

### vscode wsl2
export QT_SELECT=qt5_rv2116
export ADB_SERVER_SOCKET=tcp:172.19.192.1:5037
# adb forward tcp:6001 tcp:6001
# adb forward --list
# adb forward --remove-all

### wsl2 gdb
export PATH=$PATH:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin

arm-linux-gnueabihf-gdb /home/xjf1127/codespace/git_down/build-yda_aplus-rk1126_arm_linux-Debug/ydn-rk

### gdb command
target remote 17.19.192.1:6001

```
vscode cppconfig
```bash
{
  "configurations": [
    {
      "name": "linux-gcc-x64",
      "includePath": [
        "${workspaceFolder}/**",
        "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler/**",
        "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/**"
      ],
      "defines": [
          "QT_CORE_LIB",
          "QT_GUI_LIB",
          "QT_WIDGETS_LIB"
      ],
      "compilerPath": "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-g++",
      "cStandard": "${default}",
      "cppStandard": "${default}",
      "intelliSenseMode": "linux-gcc-x64"
    }
  ],
  "version": 4
}
```

vscode launch.json
```bash

{
    "version": "0.2.0",
    
    // "search.exclude": {
    //     // "/usr/lib/**;/oem/usr/lib/**": true //没有生效
    // },
    // "search.include":{
    // },
    // "files.exclude": {
    //     "/usr/lib/**;/oem/usr/lib/**": true //没有生效
    // },
    // "files.include":{
    //     "${workspaceFolder}/**": true
    // },

    "configurations": [
        {
            "name": "rv1126_linux_arm_remote_debug_ydn",
            "type": "cppdbg",
            "request": "launch",
            "program": "/home/xjf1127/codespace/git_down/build-yda_aplus-rk1126_arm_linux-Debug/ydn-rk",
            // "symbolSearchPath": "",
            // "sourceFileMap": {
            //     "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/target/usr/lib": "/usr/lib",
            //     "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/target/lib": "/oem/usr/lib"
            // },
            
            "cwd": "${workspaceFolder}",
            "stopAtEntry": true,
            "linux": {
                "MIMode": "gdb",
                "miDebuggerPath": "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-gdb",
                // "debugServerPath": "/oem/xjf1127/gdbserver",
                "debugServerArgs": "",
                "miDebuggerServerAddress": "172.19.192.1:6001"
            },
            "additionalSOLibSearchPath": "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib;home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/**;/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/**",
            "showDisplayString": true,
            "customLaunchSetupCommands": [
                // ,
                // {
                //     "description": "Load debug symbols",
                //     "text": "add-symbol-file /home/xjf1127/codespace/git_down/build-yda_aplus-rk1126_arm_linux-Profile/ydn-rk.debug",
                //     "ignoreFailures": false
                // }
                // {
                //     "description": "connect to target remote",
                //     "text": "target remote 172.19.192.1:6001",
                //     "ignoreFailures": false
                // },
               
                // {
                //     "description": "Load sysroot",
                //     "text": "set sysroot /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot",
                //     "ignoreFailures": false
                // },
                // {
                //     "description": "Load sysroot",
                //     "text": "set sysroot /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf",
                //     "ignoreFailures": false
                // },
                // { 
                //     "description": "Load library symbol",
                //     "text": "set solib-search-path /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib",
                //     "ignoreFailures": false
                // }
            ],
            "logging": {
                "engineLogging": false,
                "trace": false, 
                "traceResponse": false 
            },
            "targetArchitecture": "arm64"
        }
    ]
}
```


### 量化的思路

sequence uml + 组件图

### 阅读思路

先分析足够独立的模块，梳理所有存在线程关系，包括模块相关的事件循环, 组件图，sequence
功能分析完毕再梳理本模块的信号与槽映射关系
再进行从下往上的信号映射关系
然后关心在主逻辑中的调用
最终关心所有的符号调用关系

