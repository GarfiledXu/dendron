---
id: tydhrirguzfz4khswhb6yf3
title: Ble
desc: ''
updated: 1757659865857
created: 1757659853547
---

## question

@必联电子-肖永祥WIFI模块-无线路由 您好，我们在应用层开发 BLE 功能时遇到一些问题，想请教一下可能的原因和解决思路。具体情况如下：
目前我们从 Linux 的 BlueZ 协议栈中提取了 ble低功耗蓝牙相关源码，编译成基础库，并在应用主进程中直接链接使用。应用通过底层封装的 ATT/GATT API 进行蓝牙通信。
在某些业务场景下，发现蓝牙数据发送会失效：
L2CAP 层低功耗蓝牙连接仍然存在；
应用调用 GATT/ATT 层接口发送数据时，数据会正确加入底层发送队列，接口也没有返回错误；
但 BlueZ 的 I/O 事件循环（基于 epoll）没有收到 EPOLLOUT 事件，因此数据无法实际写出。
我们尝试在复现问题时使用 strace 跟踪 BlueZ 的 I/O 线程相关系统调用，发现：
开启 strace 时问题几乎不复现；
仅有一次复现的情况中，BlueZ 的 I/O 线程停留在 epoll_wait 系统调用上。
我们希望确认，这种在应用层直接调用 BlueZ ATT/GATT API 时遇到的数据发送阻塞问题，是否有已知原因或建议的处理方式。