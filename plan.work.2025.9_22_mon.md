---
id: 6ldfaxc9bvhtxmj3yw437rf
title: 9_22_mon
desc: ''
updated: 1758889291739
created: 1758507110569
---

## history

1. wifi配网问题
   1. 在密码错误的情况下连接，无法提示密码错误，并且显示已连接
      1. 修改wifi回调，使用wpa_cli.so进行状态获取
   2. 在密码错误的情况下连接，无法提示密码错误，并且显示已保存
      1. 当前业务逻辑，设备端会将当前list_network结果返回给前端，前端会将wifilist中匹配list_network的结果标记为已保存，但是目前似乎密码错误时，listnetwork的结果已久存在
      2. 需要在错误回调执行前将当前网络忽略
      3. 实现简单定时器，定时轮询
      4. 使用wpa_ctrl库的事件订阅来替换原先的wpa_cli命令，wifi状态主动轮询，定义辅助线程
      5. 确定原因，由于在接收到密码错误之前就会接收到disconnect事件，而当前在disconnect回调中会触发更新listnetwork，即还没来得及清空网络
      6. 处理，接收到disconnect事件，当判断到还在连接中时，则清空网络
   3. 在case的基础上，点击已保存的网络，会重新触发连接，这个时候会导致无响应，因为实际上并没有保存ssid和ps
      1. 封装去除已保存的网络状态函数，并且在 connectbyssid时，要进行调用，并且触发返回连接错误回调
   4. case2，在输入错误密码的时候，有概率无响应，没有任何事件，并且定时器没有发挥作用,按理说，定时器应该触发连接失败回调
   5. 忽略网络也可能存在 没有disconnect事件
      1. 只能重启wpa服务程序才能有效，需要加一个定时器，超时判断是否还存在网络，存在则意味着忽略失败，进行err提示
   6. 还是case2，输入错误密码，连接定时器超时，结果网络list变为已保存，并没有去除掉
   7. disconnect 异常，能够进入disconnect回调，但前端返回的状态还是已连接
      1. 猜测，触发discnnect时，与事件处理线程并行，导致提前执行回调，此时网络还存在于内存中,需要做互斥处理
      2. 问题原因：还没执行到回调就break了
   8. 输入错误密码，提示wifi连接错误后，还是会显示保存
      2. 重新断开连接进入后，也还是已保存，仅触发了一个扫描，没有listnetwork操作，因为丢失了open事件和close事件，需要补加，open事件会触发listnetwork
      3. 如果定时器在running，则轮询时主动轮询获取事件
      4. 有概率第一次密码错误，触发超时，没有进行网络忘却，第二次继续密码错误正常获取到密码错误时间，解决方案：在进行密码错误 忘记网络时，要将同名所有网络一次性忘却
   9. 在已经连接了一个wifi的基础上，再连接其他wifi，或者切换已保存的正确wif，会先冒出disconnect事件，然后触发对当前网络进行忘却
      1. 在已连接网络的基础上，切换新网络，会先接收到disconnect事件，此时需要判断网络状态，如果disable的ssid与当前connecting的target的ssid 相同，才能说明是连接失败，否则有可能是切换网络，不能进行网络remove, 当添加新网络连接时，已经解决可以切换
      2. 但是切换到已保存网络时，还是出现连接失败了，看日志是还是会被异常去除
         1. 还是并行导致的问题，需要在事件处理处和操作触发函数进行互斥保护

   ```bash
    ## 9.1
    [14:55:01.790][I][rk_wifi_c_adatper.cp: 117][t:3121] [===>] RK_WifiConnectWithSsid, ssid: Libawall
    [14:55:01.798][I][wifi_mng_wpa_impl.cp: 256][t:3121] GET list_network output: [network id / ssid / bssid / flags
    0       Libawall        any     [DISABLED]
    1       Libawall-5G     any     [CURRENT]
    ]
    [14:55:01.827][I][wifi_mng_wpa_impl.cp:1673][t:3121] set_connecting_status_rk, ssid=[Libawall], id=[0], connect timer start
    [14:55:01.827][I][rk_wifi_c_adatper.cp: 122][t:3121] [exit] RK_WifiConnectWithSsid, ret: [1], out_network_id: [0]
    [14:55:01.835][W][wifi_mng_wpa_impl.cp:1274][t:3156] [wpa_event] DISCONNECTED: <3>CTRL-EVENT-DISCONNECTED bssid=08:9b:4b:42:a9:62 reason=3 locally_generated=1
    [14:55:01.846][I][wifi_mng_wpa_impl.cp: 256][t:3156] GET list_network output: [network id / ssid / bssid / flags
    0       Libawall        any     [CURRENT]
    1       Libawall-5G     any     [DISABLED]
    ]
    [14:55:01.846][I][wifi_mng_wpa_impl.cp:1398][t:3156] removing network id [0], ssid [Libawall]
    [14:55:01.956][I][wifi_mng_wpa_impl.cp: 256][t:3156] GET list_network output: [network id / ssid / bssid / flags
    1       Libawall-5G     any     [DISABLED]
    ]
    [14:55:01.956][I][wifi_mng_wpa_impl.cp:1410][t:3156] remaining network id [1], ssid [Libawall-5G], flags [      [DISABLED]]
    [NETWK] CB WIFI STATUS : 5
    [14:55:01.956][D][rk_wifi_c_adatper.cp: 154][t:3156] [==>] Mock_RK_WifiGetSavedInfo
    [NETWK] Update Network Status (2(NSTA_WIFI) value:0)
    [14:55:01.983][I][wifi_mng_wpa_impl.cp: 256][t:3156] GET list_network output: [network id / ssid / bssid / flags
    1       Libawall-5G     any     [DISABLED]
    ]
    ## 9.2
    [NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:3|1|Libawall-5G||1
    [16:05:49.514][I][rk_wifi_c_adatper.cp: 117][t:2703] [===>] RK_WifiConnectWithSsid, ssid: Libawall-5G
    [16:05:49.521][I][wifi_mng_wpa_impl.cp: 256][t:2703] GET list_network output: [network id / ssid / bssid / flags
    0       Libawall-5G     any     [DISABLED]
    1       Libawall        any     [CURRENT]
    ]
    [16:05:49.547][W][wifi_mng_wpa_impl.cp:1274][t:2738] [wpa_event] DISCONNECTED: <3>CTRL-EVENT-DISCONNECTED bssid=08:9b:4b:42:a9:61 reason=3 locally_generated=1
    [16:05:49.550][I][wifi_mng_wpa_impl.cp:1673][t:2703] set_connecting_status_rk, ssid=[Libawall-5G], id=[0], connect timer start
    [16:05:49.550][I][rk_wifi_c_adatper.cp: 122][t:2703] [exit] RK_WifiConnectWithSsid, ret: [1], out_network_id: [0]
    [16:05:49.558][I][wifi_mng_wpa_impl.cp: 256][t:2738] GET list_network output: [network id / ssid / bssid / flags
    0       Libawall-5G     any     [CURRENT]
    1       Libawall        any     [DISABLED]
    ]
    [16:05:49.558][I][wifi_mng_wpa_impl.cp:1298][t:2738] removing current network: id [0], ssid [Libawall-5G], flags [      [CURRENT]]
    [NETWK] CB WIFI STATUS : 5
    [16:05:49.583][D][rk_wifi_c_adatper.cp: 154][t:2738] [==>] Mock_RK_WifiGetSavedInfo
    [NETWK] Update Network Status (2(NSTA_WIFI) value:0)
    [16:05:49.611][I][wifi_mng_wpa_impl.cp: 256][t:2738] GET list_network output: [network id / ssid / bssid / flags
    1       Libawall        any     [DISABLED]
    ]
    === WPA Network List (count: 1) ===
    Index: 0
    ID:     1
    SSID:   Libawall
    BSSID:  any
    FLAGS:        [DISABLED]
    -----------------------------------


   ```

2. 测试case
   1. 默认开机无网络连接
3. 4g信号强度获取存在问题

## new

1. 4g更新固件缓慢易失败问题排查，下载速率日志打印
2. 将当前改动rebase到基于3.11最新固件版本
3. 调研图像缩放方案，编写demo进行验证，预先实现 软实现后处理，总耗时1s左右，对单张jpg进行编码解码以及缩放处理，但cpu占用过高，在处理瞬间可以达到85%以上
4. 调研和开启硬编码时进行缩放，编写demo进行验证
5. YDAS250701000004 停留在智能印章界面， 系统存活，但是守护进程和ydn进程不存在
6. 协助开元 增加上位机 wifi 4g相关的参数, 排查相关问题

## 问题排查

1. [1.5.9] 视频上传失败，json文件存在问题,确认是旧视频，清理即可
   1. 实际问题是日志里根本没有触发视频录制
2. 位置信息
3. 指纹问题
4. 关闭目前smart 3.12相关问题
5. 28402 【设备端】smart设备-1.5.9，流程未开启标记风险点，使用印控仪用印完成后存在【风险】视频（9.25号16：20 YDAS250701000014）
   1. 确认smart在app端也同样情况，会生成风险点打印
   2. 相同流程，smart aplus报文对比
   3. {"id":"1625831782","time":1758875317175,"params":{"sealId":7046,"sealAuthentication":1,"positions":[1],"infrared":1,"leftTimes":888,"remote":3,"everyType":2,"parseBeforeStamping":2,"timeDivision":30,"coverId":0,"equipmentRecVideoAllType":1,"riskType":2,"businessKey":"5e587765f05c41d2bfc088d0d9370a2f","operationId":1971492074911358976,"documentId":85850,"pictureType":0,"staffId":83185,"authentication":2}} 大小相同
   4. 猜测，需要在视频进程判断模式，然后进行视频和json文件清除
6. 28287 【设备端】smart设备-1.5.3，流程盖印方式未开启识别录制，使用小程序用印完成后存在【风险】视频（9.9号17：29 YDAS250701000009）
   1. 1.5.9版本未复现.
7. 关闭未复现的旧版本bug

```bash
root@Rockchip:/userdata/videos/data$ ls -la
drwxr-xr-x    4 root     root          4096 .
drwxr-xr-x    2 root     root          4096 8a6f723a1e0044a59313f4d1ae6f6011.json
drwxr-xr-x    3 root     root          4096 ..
drwxr-xr-x    2 root     root          4096 111de7800d6f4295b4344569dfae0f61.json
root@Rockchip:/userdata/videos/data$ cat 8a6f723a1e0044a59313f4d1ae6f6011.json/
cat: read error: Is a directory
root@Rockchip:/userdata/videos/data$ cd 8a6f723a1e0044a59313f4d1ae6f6011.json/

```
