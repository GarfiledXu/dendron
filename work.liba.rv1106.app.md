---
id: ce65fpwpo4boansgep5mg6x
title: App
desc: ''
updated: 1755159214553
created: 1750063099745
---

## env

1. base tool
   1. qt5
   2. vscode


```bash
# 以 Ubuntu/Debian 为例
sudo apt update
sudo apt install wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo install -o root -g root -m 644 microsoft.gpg /usr/share/keyrings/
sudo sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft.gpg] \
https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'

sudo apt update
sudo apt install code

```


```bash
sudo apt update
sudo apt install qtbase5-dev qtbase5-dev-tools \
                 qtchooser qt5-qmake qtbase5-dev-tools \
                 qttools5-dev qttools5-dev-tools

```


```bash
sudo apt update
sudo apt install android-tools-adb android-tools-fastboot

```

### remote adb


```bash
# win10
adb kill-server
adb -a -P 5037 nodaemon server

# Enable-NetFirewallRule -Name FPS-ICMP4-echo-request-In
New-NetFirewallRule -DisplayName "Allow ADB TCP 5037" -Direction Inbound -Protocol TCP -LocalPort 5037 -Action Allow -Profile Any
```


```bash
# linux
export ADB_SERVER_SOCKET=tcp:192.168.52.29:5037
```

### module

1. 开关机
2. 应用启动 关闭脚本
3. 看门狗
4. gpio映射

## modify module

- [x] 4G
  - [x] 确认4g模块上电方式
  - [x] 确认4g模块通讯串口节点 /dev/ttyS4
  - [x] 确认4g模块 ppp 拨号配置文件
  - [ ] 验证at指令收发，以及内容差异
  - [x] 验证涉及网络设置的系统命令是否兼容
- [x] 电量计 梁秉政已修改
  - [x] 修改获取电量接口
  - [x] 修改获取电压接口
- [ ] 充电状态判断 当前版本判断存在问题，需要驱动和硬件重新寻找方案 20250813
  - [ ] 修改判断实现
- [ ] 六轴
  - [ ] 