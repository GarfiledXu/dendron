---
id: zpngmmhgmzs44433n738izr
title: 9_15_mo
desc: ''
updated: 1758272454577
created: 1757898818013
---
## format

```bash
<serial> [<>] 
```

## history

[[plan.work.2025.9_8_mon]]

1. 蓝牙模块独立进程封装
    1. ~~技术调研 [1][2h]~~
    2. ~~rpc库编译测试 [1][2h]~~
    3. ~~rpc库接口调用和源码解析 [1][2h] -> 8h + 6h~~
    4. ~~rpc库线程安全分析，线程安全改造，存在dispatcher内部容器崩溃问题 [3h]~~
       1. ~~确认是std容器在高并发下 插入和删除数据竞争造成的崩溃~~
    5. ~~进程管理模块设计实现 [1][6h]~~
    6. ~~基础rpc接口封装~~ [3h]
    7. ~~蓝牙进程接口设计~~ [1h]
    8. ~~蓝牙进程接口封装~~ [8h] -> [16h]
       1. 客户端 生命周期管理 + 测试程序
       2. 服务端 生命周期管理 + 测试程序
       3. 客户端+服务端 通信时生命周期管理测试程序调试
       4. 进程管理 以及业务生命周期管理 调试
       5. 调试过程异常排查，回归测试
       6. 确保客户端服务端进程强制退出控制正常
       7. 进程逻辑控制
    9. ~~模拟程序实现~~ [2h]
    10. ~~实际代码实现和库封装~~ [3h]
    11. ~~进行集成测试~~ [2h]
    12. 进程管理模块优化 [6h]

2. 录制防溢出补加

3. wifi问题排查
   1. smart设备进入网络设置，已连接的WiFi忽略网络后，WiFi列表依然显示连接状态

        ```bash
        在忽略wifi后，还显示连接，有可能没有触发状态列表更新，导致前端收到的还是旧状态列表，包括连接状态
        #网络忽略流程
        WlanController::setNetworkConfiguration: case WIFI_DEL
        # 1. 前端 显示的有连接状态和已保存状态的条目，数据来源是哪里? m_saveApList 相关操作
        # 2. 前端下发操作指定后，实际的指令解析函数在哪里?setNetworkConfiguration
        # m_offlineSsid 遗留变量，没有任何赋值操作
        # stCallbackRecvWifiStatus -> RK_WIFI_State_SCAN_RESULTS -> RK_WifiScanResults() -> getWifiScanAps: m_saveApList + const char *scan_r + currentid -> ap_lists->send_message_wifi_list
        # 大部分状态会触发->getSaveApInfos()->RK_WifiGetSavedInfo()->会对m_saveApList和m_currLinkSsid进行更新
        # 当前RK_WifiGetSavedInfo实现中 list_networks_rk_wifi_format的实现是list_networks中获取数据
        # wifi模块中只有在 触发了扫描结果状态，会发送一个整合的list给前端以外，就只有一个sendNetworkConfigurationReply函数会在各种操作执行后发出响应(用于操作错误提示)
        # 所以del正常处理流程：执行del操作-> send_repy->触发getSaveApInfos 更新m_saveApList和m_currLinkSsid ->等外部触发获取RK_WifiScanResults发送状态列表显示
        # getSaveApInfos只有在 RK_WIFI_State_OPEN RK_WIFI_State_DISCONNECTED RK_WIFI_State_CONNECTED 三个回调状态中会被触发
        ```

      1. 场景复现 [1h]
      2. 代码分析 [1h] -> 2h not finished + 4h not finished + 1h not finished
      3. 代码修改 [1h] 修改disconnect的实现，在disable和remove之后，还要saveconfig，进行本地同步
         1. 需要重新实现 RK_WifiGetSavedInfo逻辑，不仅仅是当前内存中的网络状态，还有配置文件中保存的
         2. 排查到蓝牙存在问题，断开wifi后，更新ap消息的蓝牙消息没有发送成功
         3. 有可能mainloop崩溃了，或者阻塞
         4. 排查fd
         5. 监狱蓝牙bluez的epoll 文件fd很容易在主进程被篡改，异常操作，所以还是将蓝牙功能封装到独立进程中使用
         6. 说辞：蓝牙协议栈经常要处理底层硬件事件，存在潜在的稳定性隐患
      4. 回归测试 [0.5h]
      5. 版本发布 [0.5h]
   2. smart设备，网络配置时输入不存在的网络后未进行提示网络连接失败
      1. 场景复现 [1h]
      2. 代码分析 [1h]
      3. 代码修改 [1h]
      4. 回归测试 [0.5h]
      5. 版本发布 [0.5h]
   3. wifi连接成功后，显示无网络

4. 镜像升级功能
   1. 测试镜像收集 [1h]
   2. 命令调试验证 [1h]
   3. 代码功能修改 [1.5h]
   4. 测试固件以及测试镜像ute发布 [0.5h]
   5. 进行ute环境完整测试 [1h]

5. smart设备像素缩放功能开发
   1. 方案调研
      1. rock TDE
         1. 文档资料查阅 [1h]
         2. 编写后处理测试demo [2h]
         3. 编写视频流处理测试demo [2h]
         4. 性能测试模块封装 [1h]
         5. 性能测试结论 [1h]
         6. 功能封装
            1. 后处理模块编写（io读取-解码-位图-缩放-编码-io输出）+测试 [2h]
            2. 实时视频流处理模块编写（获取位图后，在编码前，进行位图分辨率缩放）+测试 [2h]
      2. opencv
         1. 项目clone 基础功能编译 [1h]
         2. 文档查阅 设置neon功能编译选项 [1h]
         3. neon指令加速, 后处理编译demo [1h]
         4. 性能测试 + 结论 [0.5h]
      3. libyuv
         1. 项目clone 基础功能编译 [1h]
         2. 文档查阅 设置neon功能编译选项 [1h]
         3. neon指令加速, 后处理编译demo [1h]
         4. 性能测试 + 结论 [0.5h]
      4. stb_image
         1. 项目clone 基础功能编译 [1h]
         2. 性能测试 + 结论 [0.5h]
      5. ffmpeg
         1. 项目clone 基础功能编译 [1h]
         2. 性能测试 + 结论 [0.5h]

## new

1. 蓝牙 wifi配网问题
   1. ~~当前调试设备存在异常，wpa命令 save_conifg 返回FAIL，并且有概率连接成功但wpa listnetwork命令无效，并且wpa配置文件为空，权限也不对,只有-rw------而不是-rw--r--r~~ [1h]
      1. 将配置文件权限改为chmod 644，并且补加内容后正常
      2. 待补加修改文件权限和内容

      ```bash
      root@Rockchip:/oem$ cat /etc/wpa_supplicant.conf
      ctrl_interface=/var/run/wpa_supplicant
      update_config=1

      ```

   2. ~~在网络连接成功之后，退出配置页面wifi图标没有显示，日志中显示set metric100~~ [0.5h]
      1. 需要进行逻辑确认，原来是4g，连接wifi后，是否会直接切换为wifi，显示wifi图标, 的确业务逻辑设计如此
   3. ~~重新确认线程版本的蓝牙 配置wifi时 忽略网络是否会复现 蓝牙失效~~ [0.5h]
      1. 同样必现
   4. ~~当蓝牙进程不存在时，会反复初始化连接 wifi和4g~~ [1h] -> [0.5h]
      1. 由于初始化时进程崩溃，导致守护进程反复拉起
      2. 简单处理，补充bin文件存在判断

2. ~~蓝牙进程bin文件，加入固件包，并进行测试固件升级是否生效~~ [1h]
   1. 修改安装sh文件，更新正常
3. ~~代码同步，同步aplus最新代码逻辑~~ [3h]
   1. aplus新增蓝牙相关的mac地址处理逻辑，在smart上无法正常生效，需进行替换  mac地址的获取
   2. 以及新增的蓝牙gpio相关设置
   3. 以及蓝牙上下电控制
   4. 检查新增功能控制
   5. aplus新增wifi wpa配置处理，进行去除
   6. 核对检查所有修改点
   7. 网络控制模块业务逻辑改动较大
   8. 警告提示 条件逻辑修改: 缺少媒体库
   9. [NETWK] New Ble 获取并转换蓝牙MAC失败
4. ~~修复无线信号强度获取异常~~
5. 蓝牙，配置wifi网路，连接成功后，前端提示配置失败  同步了aplus业务逻辑以后，表现不一致，连接实际成功，但最后没有触发消息返回
   1. 看日志，initAutoConnType=1，走了直接设置98后，没有返回bt message的路径
   2. 初步推断 initAutoConnType 异常, initAutoConnType == 1 之后没有正常设置为0
   3. 加入打印第一次的确是1，然后设置为0，断开连接后再连，进入connect state 还是1，然后设置为0，说明有地方将其重置为1了
   4. 在开机，自动连上wifi，然后mqtt连接成功以后，接收到reset消息后，打印的
      1. [25-09-19 09:31:52.040][commandcon][handleSetDevi 945][thread1162][W] emit MainSignalManager::instance()->handleNetworkModuleCommand(name(), MDSCOMMAND_REQUEST_SET_WIFI_ENABLE

      ```bash
      [NETWK] Handle Wlan Controller Cmd: send:CommandController type:5(CTL_NETWORK_SET_WIFI_ENABLE) args:1
      [25-09-19 09:31:52.131][wlancontro][onHandleWlanC  92][thread1218][W] set initAutoConnType from: [0], to: [1]
      [NETWK] Already Init BT!
      [NETWK] Current Device Use New BLE!!!
      ```

      2. 然后忽略网络后 mqtt断开

      ```bash
      [MQTT] ****************Client Connection Lost {cause: ()}****************
      [MQTT] Failed to publish message, return code -1

      +++++m_lostDTime+++++wait 6 seconds
      +++++m_lostDTime+++++wait 6 seconds

      ```

      3. 重新连接网络后，进入connect回调，从1设置为0
      4. mqtt重新连接上，将0设置为1，
      5. 关键useNetwork: [2]，应该变为1的，
      6. 确认在忽略网络后，glGetNetworkRouteMetric return metric: [98] metric保持了98，导致network type一直是2，而不是1 ： 4g
      7. 目前实现是，直接查询路由的，说明 网络disconnect环节有问题,断开wifi网络后，wifi的路由和ipconfig是保持不变的，但此时网络失效了，所以在等待时间足够长，mqtt自动断开后，路由才真正切换,switchNetworkRoute只在mqtt中进行，但是mqtt超时断开的时间较长,导致判断条件MQTTLINK有延迟
      8. mqtt是有连接的，wifi突然断开，mqtt连接也会存在问题，通过查询metric，判断使用哪个网络不太合理,metric存在但实际网络不可用，比如wifi连接上再次disable以后
   5. 梳理过程 - mqtt_wifi_enble配置同步时序异
      1. 默认配置了wifi优先->开机->首次初始化数据结构=1 -> 进入wifi回调->自动走设置98分支，并且没有消息发送 -> 设置 initAutoConnType=0
         1. if (pthis->m_systemStatus->initAutoConnType == 1)
      2. mqtt连接成功，接收reset，设置initautoconntype为1  （只要mqtt重新连接收到reset）-> initAutoConnType=1
         1. [25-09-19 11:40:38.110][wlancontro][onHandleWlanC  87][thread2665][W] NetworkModel::CTL_NETWORK_SET_WIFI_ENABLE, useNetwork: [2 wifi], mqttStatus: [3 MQTT_LINKED]
         2. [25-09-19 11:40:38.111][wlancontro][onHandleWlanC  93][thread2665][W] set initAutoConnType from: [0], to: [1]
   6. 优化日志
      1. 将bluez源码包含
      2. 减少相关打印

6. 网络连接以及切换逻辑 状态图 [8h]
   1. 元素：wifi 操作指令+其他检测事件，wifi状态机，网络图标状态
7. 4G 信号强度异常
8. 在使用4g的情况下，蓝牙配置网络，前端发送了WIFILISTV2后，设备没有发出响应列表，但是连接没有异常，可以持续接收,wpa命令异常

```bash
root@Rockchip:/$ wpa_cli list_networks
Failed to connect to non-global ctrl_ifname: (nil)  error: No such file or directory

root@Rockchip:/$ cat /etc/wpa_supplicant.conf
ctrl_interface=/var/run/wpa_supplicant
update_config=1

network={
        ssid="Libawall"
        psk="11223344"
        disabled=1
}
root@Rockchip:/$ ls -la /etc/wpa_supplicant.conf
-rw-r--r--    1 root     root           113 /etc/wpa_supplicant.conf

## 文件被意外删除
root@Rockchip:/$ ls /var/run/
utmp           fcgiwrap.pid   nginx.pid      ppp0.pid
udev           fcgiwrap.sock  pppd2.tdb


```


### concept

1. 进程，进程组，会话，终端
2. non-detach detach 守护进程

### task

## statue line

- [x] 9_15_mon [history][1.0] [history][1.0]

- [x] 9_16_tue [history][1.0] [history][1.8]

- [x] 9_17_wed [history][1.8]

- [ ] 9_18_thu
  - [x] am [1.1] [1.2] [1.3] [1.4] [2.0]
  - [ ] pm

- [ ] 9_15_sun

- [ ] 9_15_fri

