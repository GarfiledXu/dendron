---
id: a5fv0fbsz7cszeahws74dom
title: 配网_奔溃
desc: ''
updated: 1760493269242
created: 1760493197238
---

### 20251015 smart YDAS250701000004 获取wifilist奔溃

```bash
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:31, 0 sig:-54] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4165mV curent:747nA isLessDisk:0 remainSize:24
494(less:200)MB isLow:0 infrared:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 100
[09:50:24.999][D][my_btgatt_server.cpp: 429][t:2955] enter ble_server_notify
[09:50:24.999][D][my_btgatt_server.cpp: 460][t:2955] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [36]
[NETWK] BT Write Data(ret=0): [/>|30|WIFISETTINGV2|OK~0|1~connect|#][size:0 ver:2]
enter rkbt_write_cb, data: />|20|WIFILISTV2|~|#1~1~Libawall~11223344|#��掉~-1~1|#还是一个新的smart长虹村那个电话打底衫打脑壳驾驶证你发插卡机在哪放插卡机电脑活动时间那�
[rkbt_write_cb] offset: 0, len: 20
Data: 2F 3E 7C 32 30 7C 57 49 46 49 4C 49 53 54 56 32 7C 7E 7C 23
[09:50:25.074][I][bt_ble_rpc_server.cp: 239][t:2962] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|20|WIFILISTV2|~|#1~1~Libawall~11223344|#��掉~-1~1|#还是一个新的smar
t长虹村那个电话打底衫打脑壳驾驶证你发插卡机在哪放插卡机电脑活动时间那�], len: [20]
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|20|WIFILISTV2|~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:2(CTL_NETWORK_SCAN_APS) args:1
m_isScan= false
[09:50:25.076][I][rk_wifi_c_adatper.cp:  50][t:2946] [==>] RK_WifiScan
[09:50:25.089][I][rk_wifi_c_adatper.cp:  52][t:2946] [end] RK_WifiScan, ret: [true]
Run RK_WifiScan :  true
[09:50:25.895][I][wifi_mng_wpa_impl.cp:1342][t:2978] [wpa_event] raw: <3>CTRL-EVENT-SCAN-STARTED
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[09:50:28.659][I][wifi_mng_wpa_impl.cp:1342][t:2978] [wpa_event] raw: <3>CTRL-EVENT-BSS-ADDED 98 08:9b:4b:bb:b1:67
[09:50:28.659][I][wifi_mng_wpa_impl.cp:1342][t:2978] [wpa_event] raw: <3>CTRL-EVENT-BSS-REMOVED 40 12:9b:4b:42:b4:d7
[09:50:28.659][W][wifi_mng_wpa_impl.cp:1320][t:2978] [wpa_event] SCAN RESULTS: <3>CTRL-EVENT-SCAN-RESULTS
[NETWK] CB WIFI STATUS : 8
san result list num: [56]
[09:50:29.167][I][rk_wifi_c_adatper.cp:  61][t:2978] get scan result success
[NETWK] Start Parse Wifi Scan Info! (linkSSID:Libawall
**************** [crash signal number: 11] Quit!!!****************
[09:50:29.248][D][my_btgatt_server.cpp: 429][t:2955] enter ble_server_notify
[09:50:29.249][D][my_btgatt_server.cpp: 460][t:2955] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#][size:0 ver:2]
terminate called without an active exception
Aborted (core dumped)


```
