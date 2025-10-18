---
id: 5btzhegpz28cuyc6sm07qol
title: 9_28_mo
desc: ''
updated: 1760060509546
created: 1759030840560
---

## history

## new

1. 协助测试提供特殊包，低电量不进行关机操作
2. 开启mqtt消息日志打印
3. 配合上位机新增测试功能和字段，测试通过，4g wifi 信号强度 电量
4. 验证快捷用印：关闭特权用印，申请流程打开快捷用印并且设置常规用印
5. 解决smart录入指纹，aplus不可用问题，已修复
6. 蓝牙连接用印时设备已经验证通过指纹了，盖印一次后还需要验证指纹（偶现）
7. 解决来回删除录入指纹在smart端，aplus端不可用问题
8. 【设备端】smart设备，使用指纹快捷用印登录状态，使用蓝牙二次连接提示设备占用

```bash
hci0:   Type: Primary  Bus: UART
        BD Address: 94:BA:06:91:04:0C  ACL MTU: 1021:8  SCO MTU: 255:12
        UP RUNNING
        RX bytes:11370 acl:139 sco:0 events:291 errors:0
        TX bytes:9120 acl:157 sco:0 commands:118 errors:0
广播状态

没有看到 ISCAN 或 ADVERTISING 字段

✅ 说明设备 当前没有开启广播

设备不会在周围可被扫描到
```
10. 【设备端】smart设备，设备灵敏度设置为20，APP蓝牙连接远程用印时大力摇晃都不会提示设备移动（10：16 ，YDAS250701000009）
11. 远程客户设备手动配置wifi网络
12. 【设备端】smart设备-1.5.3，流程为仅开启天玺二维码单份，使用APP蓝牙连接smart用印时二次盖印后移动
13. 镜像升级功能待实现
