---
id: 1kahawhyxy0fgkorq7lex73
title: Cmd
desc: ''
updated: 1745389289456
created: 1745389268611
---

### 查看端口占用

```bash
C:\Users\Administrator\AppData\Roaming\MobaXterm\home>adb forward tcp:6001 tcp:6001
adb.exe: error: cannot bind listener: cannot bind to 0.0.0.0:6001: 通常每个套接字地址(协议/网络地址/端口)只允许使用一次。 (10048)

C:\Users\Administrator\AppData\Roaming\MobaXterm\home>netstat -ano | findstr :6001
  TCP    0.0.0.0:6001           0.0.0.0:0              LISTENING       20684
  TCP    127.0.0.1:6001         127.0.0.1:41071        ESTABLISHED     20684
  TCP    127.0.0.1:6001         127.0.0.1:41072        ESTABLISHED     20684
  TCP    127.0.0.1:6001         127.0.0.1:41073        ESTABLISHED     20684
  TCP    127.0.0.1:41071        127.0.0.1:6001         ESTABLISHED     20684
  TCP    127.0.0.1:41072        127.0.0.1:6001         ESTABLISHED     20684
  TCP    127.0.0.1:41073        127.0.0.1:6001         ESTABLISHED     20684

C:\Users\Administrator\AppData\Roaming\MobaXterm\home>tasklist /FI "PID eq 20684"

映像名称                       PID 会话名              会话#       内存使用
========================= ======== ================ =========== ============
XWin_MobaX.exe               20684 Console                    1     68,968 K

```