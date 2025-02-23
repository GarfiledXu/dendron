---
id: 6u3mm4lct3cjoiyeocsfhcq
title: Wsl2
desc: ''
updated: 1734580041150
created: 1725527247175
---

#### 如何让外部网络访问wsl2网络
##### 场景
在wsl2上开启的http服务器，在外部其它设备上用wsl2的ip进行访问，无法访问
##### ref
[csdn](https://blog.csdn.net/w1820020635/article/details/136405333)
[新版本下如何通过外部网络访问wsl](https://www.cnblogs.com/sxrhhh/p/17901967.html)
[wsl2 通过桥接网络实现被外部局域网主机直接访问](https://www.cnblogs.com/huanliu/p/17161388.html)

##### 新机安装ubuntu wsl以后，如何转为wsl2？
- 查看wsl版本
```c++
C:\Users\XuJiaFei>wsl --list --verbose
  NAME            STATE           VERSION
* Ubuntu-22.04    Running         2
```
- wsl 磁盘空间设置
  - 默认安装位置？c盘
  - 磁盘如何移动？移动到非c盘
  - 指定大小？重新设置大小
```c++
W:\backupwsl>wsl --list --verbose
  NAME            STATE           VERSION
* Ubuntu-22.04    Running         2

W:\backupwsl> wsl.exe --shutdown

wsl --manage  Ubuntu-22.04 --move W:\wsl
```


##### 如何确定主机上的某个端口被墙了？
当前主机上使用http 5001的端口，无法接收到消息