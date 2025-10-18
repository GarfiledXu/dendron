---
id: vcefn39xlv26ckjvenebw21
title: Src_struct
desc: ''
updated: 1752134715354
created: 1751609312916
---

## ref

- [bluez5.50源码分析](https://blog.csdn.net/xufeixueren/article/details/106158383)
- [bluez: linux bluetooth stack overview](https://naehrdine.blogspot.com/2021/03/bluez-linux-bluetooth-stack-overview.html)
- [offical: bluetooth](https://www.bluetooth.com/learn-about-bluetooth/tech-overview/)
- [gogle archive: bluez-tool](https://code.google.com/archive/p/bluez-tools/)
- [ubuntu bluez package](https://launchpad.net/ubuntu/+source/bluez/5.76-0ubuntu1)
- [Developer-Study-Guide-How-to-Deploy-BlueZ-on-a-Raspberry-Pi-Board-as-a-Bluetooth-Mesh-Provisioner.pdf](https://www.bluetooth.com/wp-content/uploads/2020/04/Developer-Study-Guide-How-to-Deploy-BlueZ-on-a-Raspberry-Pi-Board-as-a-Bluetooth-Mesh-Provisioner.pdf)
- [Implementing Bluetooth on embedded Linux: Open source BlueZ vs proprietary stacks](https://www.collabora.com/news-and-blog/blog/2025/02/27/implementing-bluetooth-on-embedded-linux-with-open-source-bluez-vs-proprietary-stacks/)
- [bluez inquiry 流程梳理--从代码层面理解bluez架构](https://blog.csdn.net/qq_37182906/article/details/134672178)
- [kali bluze](https://www.kali.org/tools/bluez/)
- [LINUX下blueZ的编程](https://developer.aliyun.com/article/255004)
- [Creating a BLE Peripheral with BlueZ](https://punchthrough.com/creating-a-ble-peripheral-with-bluez/)
- [Bluez相關的各種tools的使用](https://b8807053.pixnet.net/blog/post/347831957)
- [蓝牙协议栈 BlueZ](https://getiot.tech/bluetooth/bluetooth-bluez/)
- [stackover flow](https://stackoverflow.com/questions/30386577/c-c-ble-read-write-example-with-bluez)
- [Linux BlueZ Howto](http://www.grc.upv.es/localdocs/bluezhowto.pdf)
- [good!!!offical: core specification](https://www.bluetooth.com/specifications/specs/core-specification-6-0/)
- [good!!!csdn: BLE](https://blog.csdn.net/tilblackout/category_12107956.html?spm=1001.2014.3001.5482)

## debug tool

1. `sudo apt-get install d-feet` 可视化dbus消息

## question

1. 使用dbus的意义在哪里，对比直接通过底层hci接口进行编程

## concept

1. advertising
2. irk
3. long_term_key
4. index
5. gatt
6. att
7. l2cap
8. agent
9. db
10. pdu
11. profile
12. attribute
13. characteristic
14. gap
15. service
16. topology
17. scatternet
18. piconet

## module

1. mgmt:
   1. register callback
2. mainloop
3. timeout_queue(ell.c)
4. bt_gatt_server_*()
5. bt_gatt_client_*

## integrate lib

1. glib:
   1. 事件循环: 异步io，定时器处理
   2. 数据结构
   3. 内存管理

## source map

1. gatt
   1. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/attrib/att.c (base structure and interface)
   2. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/attrib/gatt.c
   3. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/attrib/gattrib.c
   4. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/attrib/gatttool.c
   5. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/src/gatt-client.c (middle interface)
   6. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/src/gatt-database.c
   7. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/client/gatt.c (client-tool by dbus)
   8. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/test/test-gatt-profile (python test)
   9. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/unit/test-gatt.c (unit test)
   10. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/unit/test-gattrib.c
   11. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/tools/btgatt-server.c (tool)
   12. src/bluetooth_bluez_impl/bt/bluez5_utils-5.65/tools/btgatt-client.c

## think

1. 蓝牙协议栈的层次 对比 tcp/ip 协议包括其应用栈，哪些层次是类似的?协议中的角色
2. 从一个顶层标准协议(gatt)开始分析，梳理线程关系
3. GATT协议是否是有链接，是否点对点字节流传输，对比tcp传输和udp的特点是什么? 最适用的传输需求?构成要素?
4. 作为无线协议，蓝牙设备是如何被可见/扫描的，具体是通过哪些协议
5. bluez的实现一共涉及了哪些三方依赖?
   1. DBus
   2. glib
6. 扫描和被扫描的前提条件和原理?
7. 流程拆解:
   1. 未建立连接前的数据传输: 双方数据交换的时序是怎么样的
   2. 两个设备，连接如何建立: 建立时序
   3. 连接建立后的数据传输
8. 梳理BLE协议栈:
   1. GAP
   2. ATT
   3. GATT
9. 应用层自定义和设计GATT服务时，哪些数据驱动进行连接？标识设备?
10. 单设备在复杂拓扑结构图中如散网中担任多重角色，即复杂组网

## 要素 时序 方向
