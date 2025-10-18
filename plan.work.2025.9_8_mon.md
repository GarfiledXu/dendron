---
id: 00yiis3w4ssp9mvhrh49d2k
title: 9_8_mon
desc: ''
updated: 1757898379705
created: 1757295025172
---

## total

0. ~~计划~~
   1. ~~整理本周任务 [1h]~~
   2. ~~估算预期耗时 [0.5h -> 1h]~~
1. ~~守护进程问题~~
   1. ~~日志整理 [0.5h]~~
      1. E:\work\问题日志\smart\守护进程异常
      2. YDAS250701000012
      3. 09月04日 15点 43-45左右吧
   2. ~~日志分析 [1.5h->0.5h]~~
   3. ~~demon编写验证 [2h]~~ **-> 4h**
   4. 问题复现 [2h] 暂不处理

        ```bash
        Load average: 63.84 31.53 16.10 63/424 3612
        PID  PPID USER     STAT   VSZ %VSZ %CPU COMMAND
        3563     1 root     S    91224  49%   5% /oem/ydn-rk
        2611     1 root     R     101m  56%   4% /oem/ydn-rk
        3016     1 root     R      99m  55%   4% /oem/ydn-rk
        3207     1 root     R     101m  56%   4% /oem/ydn-rk
        2790     1 root     D      99m  55%   4% /oem/ydn-rk
        2607     1 root     S     108m  60%   4% /oem/ydn-rk
        3408     1 root     S      99m  55%   3% /oem/ydn-rk
        2449     1 root     S     101m  56%   3% /oem/ydn-rk
        3064  3030 root     S    14932   8%   1% rk_mpi_ao_test -i /usr/audio/10026.wav
        3936     1 root     R    42036  23%   1% /oem/ydn-rk
        525     1 root     R    37592  20%   1% /oem/ydn-rk
        1410     1 root     R    34680  19%   1% /oem/ydn-rk
        1820     1 root     R    34680  19%   1% /oem/ydn-rk
        1813     2 root     IW       0   0%   1% [kworker/0:4-eve]
        3837     2 root     DW       0   0%   1% [kworker/0:5+eve]
        3748     1 root     R    42320  23%   1% /oem/ydn-rk
        313     1 root     R    38492  21%   1% /oem/ydn-rk
        733     1 root     R    37560  20%   1% /oem/ydn-rk
        1205     1 root     R    34680  19%   1% /oem/ydn-rk
        1612     1 root     R    34680  19%   1% /oem/ydn-rk
    
        ```

   5. 代码改进 [3h] 暂不处理
   6. ~~回归测试 [1h]~~
2. ~~电池电量偏移映射，日志数据获取，更新固件预设值-> 预设值30~~
   1. ~~日志数据收集和整理 [1h]~~
   2. ~~更新固件预设值 [0.5h]~~
   3. ~~发布版本 [0.5h]~~
3. ~~某些场景下，表现为ui刷新延迟~~
   1. 场景复现 [1h]
   2. ~~代码初步分析 [1h]~~
   3. ~~代码修改 [2h]~~
   4. 回归测试 [1h]
4. 镜像升级功能
   1. 测试镜像收集 [1h]
   2. 命令调试验证 [1h]
   3. 代码功能修改 [1.5h]
   4. 测试固件以及测试镜像ute发布 [0.5h]
   5. 进行ute环境完整测试 [1h]
5. wifi问题排查
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
6. smart设备像素缩放功能开发
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
7. ~~小程序升级异常~~
   1. ~~日志搜集整理 [0.5h]~~
   2. ~~问题排查 [1h -> 1h]~~

        ```bash
        # Handle One Message 之后，根本没有进入[UPGR] Get {ugMode: 信号处理
        # [SYS] Update Status: {isLogin:1(idy:3) operation:13 没有进入操作13 说明根本没有进入函数 handleUpgradeOperation, operation一直是0
        # 确认问题，序列化时没有将type作为必要字段，导致后续直接忽略处理了
        ```

8. 指纹模块问题

9. 录制防溢出补加

10. 蓝牙模块独立进程封装
    1. ~~技术调研 [2h]~~
    2. ~~rpc库编译测试 [2h]~~
    3. rpc库接口调用和源码解析 [2h] -> 8h + 6h
    4. rpc库线程安全分析，线程安全改造，存在dispatcher内部容器崩溃问题 [3h]
       1. 确认是std容器在高并发下 插入和删除数据竞争造成的崩溃
    5. 进程管理模块设计实现 [6h]
    6. rpc模块设计实现
    7. 蓝牙进程模块架构 [3h]
       1. 进程管理
       2. 接口设计
    8. 模拟程序实现 [2h]
    9.  实际代码实现和库封装 [3h]
    10. 进行集成测试 [2h]

## 状态系数

1. 上午1.0
2. 下午0.7
3. 晚上0.6

## 9_8_mon

- [x] AM: [0.1] [0.2] [2] [1.1]
- [x] PM: [7.0] [1.1] [1.2] [1.3]

## 9_9_tue

- [x] AM: [1.6] [5.2]
- [x] PM: [5.0]

## 9_10_wed

- [x] AM: [5.0]
- [x] PM: [5.0] [10.1] [10.2]

## 9_11_thu

- [x] AM: [10.3]
- [x] PM: [10.3]

## 9_12_sun

- [x] AM: [10.3]
- [x] PM: [10.3]

## 9_12_fri

- [x] PM: [10.3]

