---
id: 1j4qifpsan52wfyr6fhteee
title: Initauto2
desc: ''
updated: 1758360384570
created: 1758353290698
---

## 场景，从开机 默认连接wifi 蓝牙配网 忽略网络 连接网络 设备连接成功 但前端app等待超时，提示配置失败

``` bash
root@Rockchip:/oem$ ./ydn-rk dbg=2
QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-root'
[SYS] App Compile Date Time :  Sep 20 2025 15:22:19
[SYS] *********[HardwareVersion:TX_SMART_MB_V05; ImageVersion:YDN_1.1] [hasWifi:1 hasVideoRecord:1 hasPhotoEncode:1 pkgTimes:2025/09/20 15:23:30:000]*********[SYS]
[SYS] Read sys.cfg finish[IS_SYNC:0]!!!!
[SYS] Get API=https://uteydaapi.libawall.com
[SYS] Get AXISOFFSET=0
[SYS] Get CAP=100
[SYS] Get CLIENTID=ba3dcc5d-a807-48ad-8379-960a0d814fa1|time=1756625455521
[SYS] Get DEVICEINFO=YDAS250701000005
[SYS] Get ENVIRONMENT=3
[SYS] Get IP_PORT=115.238.152.230:1887
[SYS] Get ISUPGRADE=0
[SYS] Get IS_OS_UPGRADE=0
[SYS] Get KEY=33GVH9CVLD46B3JF7D
[SYS] Get LOCK=0
[SYS] Get MQTTPASSWD=0VyxiXaa61MSEW0emXq/Ww==
[SYS] Get MQTTUSERNAME=YDAS250701000005&yda
[SYS] Get OFF_TIME=-1
[SYS] Get PRIORITY=1
[SYS] Get PUBLISH=1
[SYS] Get SEALDISTANCE=0
[SYS] Get SEALID=7076
[SYS] Get SEALPATTERN=1
[SYS] Get SEALTYPE=2
[SYS] Get SLEEP_TIME=3
[SYS] Get STAFFNAME=
[SYS] Get STAMPINSTALL=1
[SYS] Get STAMPNAME=徐佳飞-smart-奥特之章重生
[SYS] Get SUPERUSER=1-
[SYS] Get SW_BLUETOOTH=1
[SYS] Get SW_FOURG=1
[SYS] Get SW_WIFI=1
[SYS] Get UPGRADETYPE=0
[SYS] Get VERSION=1.5.6
[SYS] Get VOICE=1
[U**I] onHandleMainUICommand :  61
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[0 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:0 isCharge:0 energy:0 capcity:0mV curent:0nA isLessDisk:0 remainSize:0(less:200)MB isLow:0 infrared:0 isStampLock:
0 useNetwork:0 fingeOperation:0 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:0 isGpySync:0 mediaUpNums:0}
[SYS] Initing Load Module:  "EventModule"
[UASRT2]:open ttyS2 successed!
Opened device: /dev/input/event0
MotionDetector openDevice fail, ret: 1
[SYS] Initing Load Module:  "FingerModule"
[SYS] Start Set Finger Mode Is wake up!
[SYS] Initing Load Module:  "MotorModule"
[MOTOR] Init Get KeysStatus++++++++press:1 (no:1 yes:0) motor:0 (yes:1 no:0) gate_on:1 (no:1 yes:0) gate_off:0 (yes:1 no:0)!
[SYS] Initing Load Module:  "InfraredModule"
[SYS] Initing Load Module:  "MqttModule"
[SYS] Set Finger Mode Is wake up!
[SYS] Initing Load Module:  "StampModule"
[SYS] Initing Load Module:  "AudioModule"
[SYS] Initing Load Module:  "MediaModule"
killall: curl: no process killed
[SYS] Initing Load Module:  "FrontModule"
[MEDIA] Get libeasymedia.so Size(init:0 use:0) Bytes; recordvideo Size: 584788 Bytes takephoto Size: 530096 Bytes catch Size: 0 Bytes
[SYS] Initing Load Module:  "ShutdownModule"
[SYS] Initing Load Module:  "UpgradeModule"
[UPGR] Init Get isUpgradeRun: 0
[SYS] Initing Load Module:  "UpperModule"
[spdlog] get sink_mode: 1
[spdlog] get level_env: info
[25-09-20 15:26:13.939][devcheckco][checkRunningD 332][thread3306][W] get useNetwork changed: [-1] -> [0]
[SYS] Initing Load Module:  "NetworkModule"
killall: udhcpc: no process killed
killall: wpa_supplicant: no process killed
[NETWK] Init BT!!!
[NETWK] Start Init  "WlanController"
[NETWK] Init WIFI!!!
[25-09-20 15:26:14.093][wifi_mng_w][start_polling 983][thread3372][I] [===>] start_polling_status_rk]
[25-09-20 15:26:14.097][wifi_mng_w][start_polling1086][thread3372][I] [exit] enter start_polling_status_rk]
BT BLE MAC: 4C:04:91:06:BA:94
[25-09-20 15:26:14.169][rk_bt_c_ad][Mock_RK_BLEIn 244][thread3374][I] [==>] device: [YDAS250701000005]
[25-09-20 15:26:14.173][bt_ble_rpc][client_init    42][thread3374][W] [BtBleRpcClient] server_pid: [0], client_pid: [3298]
killall: wpa_supplicant: no process killed
[SYS] Finish Load All Modules!
[SYS] Init Get m_modulesLMap: QMap(("AudioModule", 1)("EventModule", 1)("FingerModule", 1)("FrontModule", 1)("InfraredModule", 1)("MediaModule", 1)("MotorModule", 1)("MqttModule", 1)("NetworkModule", 1)("ShutdownModule", 1)(
"StampModule", 1)("UpgradeModule", 1)("UpperModule", 1))
[spdlog] get sink_mode: 1
[spdlog] get level_env: info
killall: pppd: no process killed
[25-09-20 15:26:14.248][ble_server][main            5][thread3389][I] === Start BtBleRpcServer ===
[25-09-20 15:26:14.253][bt_ble_rpc][server_init    48][thread3389][W] [BtBleRpcServer] server_pid: [3389], client_pid: [3298]
killall: &&: no process killed
[25-09-20 15:26:14.276][bt_ble_rpc][client_init    68][thread3374][I] [BtBleRpcClient] get server pid: [3389]
[25-09-20 15:26:14.277][bt_ble_rpc][register_rpc  263][thread3374][I] will open rpc lient
[SYS] Start Set Finger Mode Is sleep!
[SYS] Set Finger Mode Is sleep!
[25-09-20 15:26:14.286][bt_ble_rpc][operator()    268][thread3397][I] will run rpc lient io thread
[25-09-20 15:26:14.287][bt_ble_rpc][operator()    208][thread3397][I] [BtBleRpcClient] RPC connected
killall: 1: no process killed
killall: &&: no process killed
[25-09-20 15:26:14.396][bt_ble_rpc][ble_init_sync 152][thread3374][I] BLE init done, ret: [0], cost: 18 ms
[25-09-20 15:26:14.397][rk_bt_c_ad][Mock_RK_BLEIn 268][thread3374][I] ble_init_sync success
[25-09-20 15:26:14.397][rk_bt_c_ad][Mock_RK_BLEIn 272][thread3374][I] [end] device: [YDAS250701000005]
[25-09-20 15:26:14.397][rk_bt_c_ad][Mock_RK_BleDi 297][thread3374][E] mock api: [RK_BleDisconnect], not impl!
[NETWK] Current Device Use New BLE!!!
[25-09-20 15:26:14.402][my_btgatt_][operator()    382][thread3402][I] [===>] ble_server_running_sync_ thread
killall: -0: no process killed
killall: pppd: no process killed
[NETWK] Open 4G Power Successed!!!
[25-09-20 15:26:14.479][rk_wifi_c_][Mock_RK_WifiO  20][thread3372][I] RK_WifiOpen, ret: [0]
[WifiMng] Enabling network id=0 (Libawall)
[WifiMng] Reconnecting...
[25-09-20 15:26:15.133][wifi_mng_w][operator()   1077][thread3377][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-20 15:26:15.134][wifi_mng_w][operator()   1079][thread3377][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[NETWK] Urc Cache: AT+QCFG="urc/cache",0
[NETWK] Get CREG Is OK!
No defaultroute line to comment in "/etc/ppp/peers/quectel-ppp"
[NETWK] Init EC200U 4G Check IP Delays [1]
[25-09-20 15:26:19.169][wifi_mng_w][operator()   1077][thread3377][I] [on loop] start_polling_status_rk, enter callback, rk_state: [CONNECTED]
[NETWK] CB WIFI STATUS : 4
=== WPA Network List (count: 1) ===
Index: 0
  ID:     0
  SSID:   Libawall
  BSSID:  any
  FLAGS:        [CURRENT]
-----------------------------------
[25-09-20 15:26:19.177][rk_wifi_c_][print_saved_i 126][thread3377][I] Total saved networks: 1
[25-09-20 15:26:19.177][rk_wifi_c_][print_saved_i 136][thread3377][I]   [#0] SSID: 'Libawall', BSSID: 'any', State: '[CURRENT]'
[25-09-20 15:26:19.178][rk_wifi_c_][Mock_RK_WifiG 144][thread3377][I] [exit] RK_WifiGetSavedInfo: [0]
[NETWK] **********************
[NETWK] updateSaveApInfo {id=0 ssid=Libawall bssid=any state:[CURRENT]}
[NETWK] **********************
[NETWK] CONNETED WIFI SSID: (initAutoConnType:1) ssid:Libawall mac:94:ba:06:91:04:0b ip:
[25-09-20 15:26:19.178][wlancontro][stCallbackRec 185][thread3377][W] [==>] stCallbackRecvWifiStatus: RK_WIFI_State_CONNECTED, initAutoConnType: [1], (0: NULL 1:Wlan 2:4G)
[25-09-20 15:26:19.178][wlancontro][stCallbackRec 190][thread3377][W] stCallbackRecvWifiStatus: because initAutoConnType: [1], setUdhcpcOnMetric(NTYPE_WIFI, 98)
[NETWK] WIFI SET UDHCP (type:2) metric:98
[glSetUdhcpcOnMetric] IP output: ""
[glSetUdhcpcOnMetric] Interface "wlan0" has no IP, start udhcpc
udhcpc: started, v1.27.2
cat /tmp/metric success
udhcpc: sending discover
udhcpc: sending select for 192.168.48.19
udhcpc: lease of 192.168.48.19 obtained, lease time 43200
cat /tmp/metric success
deleting routers
set wlan0 metric : 98
adding dns 223.5.5.5
adding dns 223.6.6.6
[25-09-20 15:26:19.451][global_api][glGetCurrentS2113][thread3372][W] glGetNetworkRouteMetric metric changed: [-1] -> [98]
[NETWK] Init EC200U 4G Check IP Delays [2]
[25-09-20 15:26:19.938][devcheckco][checkRunningD 332][thread3306][W] get useNetwork changed: [0] -> [2]
[NETWK] Init EC200U 4G Check IP Delays [3]
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[MQTT] Link [Wlan] [https://uteydaapi.libawall.com/equipment/link] OK!
[MQTT] Update Get [Can:1 isCloudDisableMqtt:0] To Enable Connecting Mqtt-Server!
[U**I] onHandleMainUICommand :  56
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[0 sig:0, 1 sig:-50] isDeviceLock:0 mqttStatus:1 isCharge:1 energy:100 capcity:4164mV curent:-30nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:0 isGpySync:0 mediaUpNums:0}
[MEDIA] No Log Files To Upload!!!
[MQTT] Start Connect Server [network=[2]WIFI] [env:3] [ip_port:115.238.152.230:1887] [clientId:ba3dcc5d-a807-48ad-8379-960a0d814fa1|time=1756625455521] [mqttuser:YDAS250701000005&yda] [mqttpwd:0VyxiXaa61MSEW0emXq/Ww==]!
[glSetUdhcpcOnMetric] IP output: "192.168.48.19"
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 98
[NETWK] EC200U 4G PPPD Successed! & Get 4G Address : "          inet addr:10.203.131.178  P-t-P:10.64.64.64  Mask:255.255.255.255\n"
[NETWK] Update Network Status (1(NSTA_FOURG) value:1)
[MQTT] HAS CONNECT OK!!!
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/reset/invoke]
[MQTT] FINISH MQTTClient Connected Successed!
[U**I] onHandleMainUICommand :  57
[MQTT] Message arrived [recvcnt:1] [mainqueues_size:0] [len:361] topicName: [<���J]
[25-09-20 15:26:24.299][wlancontro][stCallbackRec 243][thread3377][W] [end] stCallbackRecvWifiStatus: initAutoConnType: [0]
[25-09-20 15:26:24.299][wifi_mng_w][operator()   1079][thread3377][I] [on loop] start_polling_status_rk, break callback, rk_state: [CONNECTED]
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[MQTT] Message arrived [recvcnt:2] [mainqueues_size:0] [len:96] topicName: []
[COMM] Handle One Message( m_msgId=152624653 [type:2] topic:/sys/yda/YDAS250701000005/service/reset/invoke datas:{"id":"152624653","time":1758353184653,"params":{"voice":true,"wifiPriority":1,"sealId":7076,"sealPattern":1,"u
serMobileNetwork":1,"reconnection":false,"sealType":2,"sealName":"徐佳飞-smart-奥特之章重生","useWifi":1,"useBluetooth":1,"gyroParameter":1,"needConvertVideo":true,"remoteWake":-1,"reset":false,"sleepTime":3,"offTime":-1,"re
moteLock":false}} )
[COMM] Sync Date Time(2025-09-20 15:26:25) to Local! Get Local(2025-09-20 15:26:25)
[U**I] onHandleMainUICommand :  0
[25-09-20 15:26:25.036][commandcon][handleSetDevi 945][thread3318][W] emit MainSignalManager::instance()->handleNetworkModuleCommand(name(), MDSCOMMAND_REQUEST_SET_WIFI_ENABLE
[EVENT] Module Recv Commands (send: CommandController cmd:21))
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:5(CTL_NETWORK_SET_WIFI_ENABLE) args:1
[25-09-20 15:26:25.118][wlancontro][onHandleWlanC  89][thread3372][W] onHandleWlanControllerCommands NetworkModel::CTL_NETWORK_SET_WIFI_ENABLE, useNetwork: [2], mqttStatus: [3]
[25-09-20 15:26:25.118][wlancontro][onHandleWlanC  99][thread3372][W] set initAutoConnType: [0] -> [1]
[NETWK] Already Init BT!
[NETWK] Current Device Use New BLE!!!
killall: rk_mpi_ao_test: no process killed
[SYS] Audio Cmd End:  "rk_mpi_ao_test -i /usr/audio/10001.wav --sound_card_name=hw:0,0 --device_ch=2 --device_rate=48000 --input_rate=48001 --input_ch=2 --set_volume=50 > /dev/null 2>&1 &"
[COMM] Handle One Message( m_msgId=1 [type:2] topic:/sys/yda/YDAS250701000005/event/fingerprint_import/post_reply datas:{"code":0,"data":{"fingerprint":[],"privilege":[]},"success":true,"id":"1","time":1758353184722} )
[MEDIA] Init Get photoUpNums: 0 videoUpNums:0
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-54] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4160mV curent:-61nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-52] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4166mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-20 15:26:42.104][my_btgatt_][operator()    794][thread3402][I] will service_builder
populate_rkbt_service (RK-style BLE)
[NETWK] GET BT STATUS : 1 [CONNECTED] [name:YDAS250701000005 addr:94:BA:06:91:04:0C]
[25-09-20 15:26:42.106][rk_bt_c_ad][operator()    248][thread3397][W] [BtBleRpcClient] State changed, addr: 94:BA:06:91:04:0C, dev: YDAS250701000005, state: 1
[NETWK] Update Network Status (3(NSTA_BT) value:1)
[NETWK] Init Wait WIFI/4G AutoConnect State=[1] !(0: NULL 1:Wlan 2:4G)!
[25-09-20 15:26:42.108][bt_ble_rpc][bluez_call_st 424][thread3402][I] bluez_call_state_sync_ timeout for cmd: [ble_state], ret code: [0], cost time ms: [2]
Wifi: Service Changed CCC Write called [1:0]
Wifi: Service Changed Enabled: true
enter rkbt_write_cb, data: />|78|DEVICEMATCHV2|徐佳飞~83185~1758353203~c736e1a8d5b90b6383d882e2628316e9~3~|#
[rkbt_write_cb] offset: 0, len: 84
Data: 2F 3E 7C 37 38 7C 44 45 56 49 43 45 4D 41 54 43 48 56 32 7C E5 BE 90 E4 BD B3 E9 A3 9E 7E 38 33 31 38 35 7E 31 37 35 38 33 35 33 32 30 33 7E 63 37 33 36 65 31 61 38 64 35 62 39 30 62 36 33 38 33 64 38 38 32 65 32 36 32
 38 33 31 36 65 39 7E 33 7E 7C 23
[25-09-20 15:26:43.816][bt_ble_rpc][operator()    239][thread3402][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|78|DEVICEMATCHV2|徐佳飞~83185~1758353203~c736e1a8d5b90b6383d882e2628316e9~3~|#], len:
[84]
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|78|DEVICEMATCHV2|徐佳飞~83185~1758353203~c736e1a8d5b90b6383d882e2628316e9~3~|# )
[COMM] Sync Date Time(2025-09-20 15:26:44) to Local! Get Local(2025-09-20 15:26:44)
[COMM] USER LOGIN (staffId:83185 staffName:徐佳飞) !
[U**I] onHandleMainUICommand :  2
killall: rk_mpi_ao_test: no process killed
[NETWK] BT Write Data(ret=0): [/>|30|DEVICEMATCHV2|OK~0|徐佳飞~83185~4|#][size:0 ver:2]
[SYS] Audio Cmd End:  "rk_mpi_ao_test -i /usr/audio/10012.wav --sound_card_name=hw:0,0 --device_ch=2 --device_rate=48000 --input_rate=48001 --input_ch=2 --set_volume=50 > /dev/null 2>&1 &"
enter rkbt_write_cb, data: />|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#
[rkbt_write_cb] offset: 0, len: 89
Data: 2F 3E 7C 37 31 7C 44 45 56 49 43 45 49 4E 46 4F 53 45 54 56 32 7C 2D 31 7E 33 7E 31 7E 31 7E 31 7E 31 7E 31 7E 31 7E 31 7E 37 30 37 36 7E 32 7E E5 BE 90 E4 BD B3 E9 A3 9E 2D 73 6D 61 72 74 2D E5 A5 A5 E7 89 B9 E4 B9 8B
 E7 AB A0 E9 87 8D E7 94 9F 7E 2D 31 7E 31 7C 23
[25-09-20 15:26:44.259][bt_ble_rpc][operator()    239][thread3402][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#], len: [
89]
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|# )
[NETWK] BT Write Data(ret=0): [/>|25|DEVICEINFOSETV2|OK~0|#][size:0 ver:2]
enter rkbt_write_cb, data: />|27|PRIVILEGEDATAV2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#
[rkbt_write_cb] offset: 0, len: 27
Data: 2F 3E 7C 32 37 7C 50 52 49 56 49 4C 45 47 45 44 41 54 41 56 32 7C 31 30 30 7C 23
[25-09-20 15:26:44.468][bt_ble_rpc][operator()    239][thread3402][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|27|PRIVILEGEDATAV2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#], len: [
27]
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|27|PRIVILEGEDATAV2|100|# )
[NETWK] BT Write Data(ret=0): [/>|99|PRIVILEGEDATAV2|OK~0|0~1~0~[]|#][size:0 ver:2]
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-53] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4166mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
enter rkbt_write_cb, data: />|20|WIFILISTV2|~|#2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#
[rkbt_write_cb] offset: 0, len: 20
Data: 2F 3E 7C 32 30 7C 57 49 46 49 4C 49 53 54 56 32 7C 7E 7C 23
[25-09-20 15:26:45.967][bt_ble_rpc][operator()    239][thread3402][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|20|WIFILISTV2|~|#2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#], len: [
20]
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|20|WIFILISTV2|~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:2(CTL_NETWORK_SCAN_APS) args:1
m_isScan= false
[25-09-20 15:26:45.977][rk_wifi_c_][Mock_RK_WifiS  41][thread3372][I] RK_WifiScan, ret: [true]
Run RK_WifiScan :  true
[NETWK] CB WIFI STATUS : 8
san result list num: [{}][25-09-20 15:26:46.620][rk_wifi_c_][Mock_RK_WifiS  50][thread3377][I] get scan result success
[NETWK] Start Parse Wifi Scan Info! (linkSSID:Libawall
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-46~1~ChinaNet-TNT~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-47~1~device~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|39|WIFILISTV2|OK~0|-48~1~Libawall-5G~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-48~1~zzwifi~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~1~1|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-54~1~ceshi~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-66~1~ceshi_5G~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|54|WIFILISTV2|OK~0|-67~1~DIRECT-54-HP M232 LaserJet~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-68~1~TP-LINK_F41B~0~0|#][size:0 ver:2]
enter rkbt_write_cb, data: />|33|WIFISETTINGV2|2~Libawall~|#1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#
[rkbt_write_cb] offset: 0, len: 33
Data: 2F 3E 7C 33 33 7C 57 49 46 49 53 45 54 54 49 4E 47 56 32 7C 32 7E 4C 69 62 61 77 61 6C 6C 7E 7C 23
[25-09-20 15:26:52.118][bt_ble_rpc][operator()    239][thread3402][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|33|WIFISETTINGV2|2~Libawall~|#1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#], len: [
33]
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|33|WIFISETTINGV2|2~Libawall~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:2|0|Libawall||1
[25-09-20 15:26:52.161][rk_wifi_c_][Mock_RK_WifiD 156][thread3372][I] [===>] RK_WifiDisconnectNetwork1
[NETWK] CB WIFI STATUS : 5
[NETWK] Update Network Status (2(NSTA_WIFI) value:0)
[NETWK] Init Wait WIFI/4G AutoConnect State=[1] !(0: NULL 1:Wlan 2:4G)!
=== WPA Network List (count: 0) ===
[25-09-20 15:26:52.202][rk_wifi_c_][print_saved_i 126][thread3372][I] Total saved networks: 0
[25-09-20 15:26:52.202][rk_wifi_c_][Mock_RK_WifiG 144][thread3372][I] [exit] RK_WifiGetSavedInfo: [0]
[25-09-20 15:26:52.202][rk_wifi_c_][Mock_RK_WifiD 158][thread3372][I] [exit] RK_WifiDisconnectNetwork1
"[NET] Disconnect Wifi Info: id: 0 ssid[Libawall =?= Libawall]"
[NETWK] BT Write Data(ret=0): [/>|33|WIFISETTINGV2|OK~0|5~disconnect|#][size:0 ver:2]
enter rkbt_write_cb, data: />|20|WIFILISTV2|~|#2~Libawall~|#1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#
[rkbt_write_cb] offset: 0, len: 20
Data: 2F 3E 7C 32 30 7C 57 49 46 49 4C 49 53 54 56 32 7C 7E 7C 23
[25-09-20 15:26:52.268][bt_ble_rpc][operator()    239][thread3402][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|20|WIFILISTV2|~|#2~Libawall~|#1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#], len: [
20]
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|20|WIFILISTV2|~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:2(CTL_NETWORK_SCAN_APS) args:1
m_isScan= false
[25-09-20 15:26:52.281][rk_wifi_c_][Mock_RK_WifiS  41][thread3372][I] RK_WifiScan, ret: [true]
Run RK_WifiScan :  true
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:-52] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4166mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[NETWK] CB WIFI STATUS : 8
san result list num: [{}][25-09-20 15:26:53.199][rk_wifi_c_][Mock_RK_WifiS  50][thread3377][I] get scan result success
[NETWK] Start Parse Wifi Scan Info! (linkSSID:
[25-09-20 15:26:53.214][wifi_mng_w][operator()   1077][thread3377][I] [on loop] start_polling_status_rk, enter callback, rk_state: [OPEN]
[NETWK] CB WIFI STATUS : 6
=== WPA Network List (count: 0) ===
[25-09-20 15:26:53.222][rk_wifi_c_][print_saved_i 126][thread3377][I] Total saved networks: 0
[25-09-20 15:26:53.222][rk_wifi_c_][Mock_RK_WifiG 144][thread3377][I] [exit] RK_WifiGetSavedInfo: [0]
[25-09-20 15:26:53.222][wifi_mng_w][operator()   1079][thread3377][I] [on loop] start_polling_status_rk, break callback, rk_state: [OPEN]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-46~1~ChinaNet-TNT~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-47~1~device~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|39|WIFILISTV2|OK~0|-48~1~Libawall-5G~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-48~1~zzwifi~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-52~1~TP-LINK_F41B~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-54~1~ceshi~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-54~1~Libawall~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|54|WIFILISTV2|OK~0|-63~1~DIRECT-54-HP M232 LaserJet~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-66~1~ceshi_5G~0~0|#][size:0 ver:2]
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:-256] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4166mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:
0 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
enter rkbt_write_cb, data: />|43|WIFISETTINGV2|1~1~Libawall~11223344|#76~2~徐佳飞-smart-奥特之章重生~-1~1|#
[rkbt_write_cb] offset: 0, len: 43
Data: 2F 3E 7C 34 33 7C 57 49 46 49 53 45 54 54 49 4E 47 56 32 7C 31 7E 31 7E 4C 69 62 61 77 61 6C 6C 7E 31 31 32 32 33 33 34 34 7C 23
[25-09-20 15:27:02.499][bt_ble_rpc][operator()    239][thread3402][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|43|WIFISETTINGV2|1~1~Libawall~11223344|#76~2~徐佳飞-smart-奥特之章重生~-1~1|#], len: [
43]
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|43|WIFISETTINGV2|1~1~Libawall~11223344|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:1|1|Libawall|11223344|1
[25-09-20 15:27:02.582][rk_wifi_c_][Mock_RK_WifiC  68][thread3372][I] [===>] RK_WifiConnect
[25-09-20 15:27:02.586][rk_wifi_c_][Mock_RK_WifiC  72][thread3372][I] the matched wpa_network_config_list count: [0], with ssid: [Libawall], pask: [11223344]
[25-09-20 15:27:02.822][rk_wifi_c_][Mock_RK_WifiC  99][thread3372][I] [exit] RK_WifiConnect
[25-09-20 15:27:03.290][wifi_mng_w][operator()   1077][thread3377][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-20 15:27:03.290][wifi_mng_w][operator()   1079][thread3377][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[25-09-20 15:27:06.312][wifi_mng_w][operator()   1077][thread3377][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-20 15:27:06.312][wifi_mng_w][operator()   1079][thread3377][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[MQTT] ****************Client Connection Lost {cause: ()}****************
[25-09-20 15:27:07.319][wifi_mng_w][operator()   1077][thread3377][I] [on loop] start_polling_status_rk, enter callback, rk_state: [CONNECTED]
[NETWK] CB WIFI STATUS : 4
=== WPA Network List (count: 1) ===
Index: 0
  ID:     0
  SSID:   Libawall
  BSSID:  any
  FLAGS:        [CURRENT]
-----------------------------------
[25-09-20 15:27:07.326][rk_wifi_c_][print_saved_i 126][thread3377][I] Total saved networks: 1
[25-09-20 15:27:07.327][rk_wifi_c_][print_saved_i 136][thread3377][I]   [#0] SSID: 'Libawall', BSSID: 'any', State: '[CURRENT]'
[25-09-20 15:27:07.327][rk_wifi_c_][Mock_RK_WifiG 144][thread3377][I] [exit] RK_WifiGetSavedInfo: [0]
[NETWK] **********************
[NETWK] updateSaveApInfo {id=0 ssid=Libawall bssid=any state:[CURRENT]}
[NETWK] **********************
[NETWK] CONNETED WIFI SSID: (initAutoConnType:1) ssid:Libawall mac:94:ba:06:91:04:0b ip:192.168.48.19
[25-09-20 15:27:07.327][wlancontro][stCallbackRec 185][thread3377][W] [==>] stCallbackRecvWifiStatus: RK_WIFI_State_CONNECTED, initAutoConnType: [1], (0: NULL 1:Wlan 2:4G)
[25-09-20 15:27:07.327][wlancontro][stCallbackRec 190][thread3377][W] stCallbackRecvWifiStatus: because initAutoConnType: [1], setUdhcpcOnMetric(NTYPE_WIFI, 98)
[NETWK] WIFI SET UDHCP (type:2) metric:98
[glSetUdhcpcOnMetric] IP output: "192.168.48.19"
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 98
[25-09-20 15:27:07.393][wlancontro][stCallbackRec 236][thread3377][W] [end] stCallbackRecvWifiStatus: initAutoConnType: [1] -> [0]
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[25-09-20 15:27:08.411][wlancontro][stCallbackRec 243][thread3377][W] [end] stCallbackRecvWifiStatus: initAutoConnType: [0]
[25-09-20 15:27:08.416][wifi_mng_w][operator()   1079][thread3377][I] [on loop] start_polling_status_rk, break callback, rk_state: [CONNECTED]
+++++m_lostDTime+++++wait 6 seconds
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-54] isDeviceLock:0 mqttStatus:1 isCharge:1 energy:100 capcity:4166mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
[MQTT] Start Connect Server [network=[2]WIFI] [env:3] [ip_port:115.238.152.230:1887] [clientId:ba3dcc5d-a807-48ad-8379-960a0d814fa1|time=1756625455521] [mqttuser:YDAS250701000005&yda] [mqttpwd:0VyxiXaa61MSEW0emXq/Ww==]!
[MQTT] HAS CONNECT OK!!!
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/reset/invoke]
[MQTT] FINISH MQTTClient Connected Successed!
[U**I] onHandleMainUICommand :  57
[MQTT] Message arrived [recvcnt:1] [mainqueues_size:0] [len:361] topicName: [<��V5]
[MQTT] Message arrived [recvcnt:2] [mainqueues_size:0] [len:96] topicName: [��d���d�/YDAS250701000005/event/fingerprint_import/post_reply]
[COMM] Handle One Message( m_msgId=152715781 [type:2] topic:/sys/yda/YDAS250701000005/service/reset/invoke datas:{"id":"152715781","time":1758353235781,"params":{"voice":true,"wifiPriority":1,"sealId":7076,"sealPattern":1,"u
serMobileNetwork":1,"reconnection":false,"sealType":2,"sealName":"徐佳飞-smart-奥特之章重生","useWifi":1,"useBluetooth":1,"gyroParameter":1,"needConvertVideo":true,"remoteWake":-1,"reset":false,"sleepTime":3,"offTime":-1,"re
moteLock":false}} )
[COMM] Sync Date Time(2025-09-20 15:27:16) to Local! Get Local(2025-09-20 15:27:16)
[EVENT] Module Recv Commands (send: CommandController cmd:21))
[25-09-20 15:27:16.030][commandcon][handleSetDevi 945][thread3318][W] emit MainSignalManager::instance()->handleNetworkModuleCommand(name(), MDSCOMMAND_REQUEST_SET_WIFI_ENABLE
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:5(CTL_NETWORK_SET_WIFI_ENABLE) args:1
[25-09-20 15:27:16.032][wlancontro][onHandleWlanC  89][thread3372][W] onHandleWlanControllerCommands NetworkModel::CTL_NETWORK_SET_WIFI_ENABLE, useNetwork: [2], mqttStatus: [3]
[25-09-20 15:27:16.032][wlancontro][onHandleWlanC  99][thread3372][W] set initAutoConnType: [0] -> [1]
[NETWK] Already Init BT!
[NETWK] Current Device Use New BLE!!!
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-53] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4166mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[COMM] Handle One Message( m_msgId=8 [type:2] topic:/sys/yda/YDAS250701000005/event/fingerprint_import/post_reply datas:{"code":0,"data":{"fingerprint":[],"privilege":[]},"success":true,"id":"8","time":1758353235867} )
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-53] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4166mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4167mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
^C**************** [crash signal number: 2] Quit!!!****************
terminate called without an active exception
^CAborted (core dumped)

```

### 提取的关键日志

```bash

==> 初始化
[25-09-20 15:26:13.939][devcheckco][checkRunningD 332][thread3306][W] get useNetwork changed: [-1] -> [0]

==> 开机wifi自动连接 [[[[wifi 线程]]]]
[25-09-20 15:26:19.178][wlancontro][stCallbackRec 185][thread3377][W] [==>] stCallbackRecvWifiStatus: RK_WIFI_State_CONNECTED, initAutoConnType: [1], (0: NULL 1:Wlan 2:4G)
[25-09-20 15:26:19.178][wlancontro][stCallbackRec 190][thread3377][W] stCallbackRecvWifiStatus: because initAutoConnType: [1], setUdhcpcOnMetric(NTYPE_WIFI, 98)
==> wifi成功获取到ip和dns
[25-09-20 15:26:19.451][global_api][glGetCurrentS2113][thread3372][W] glGetNetworkRouteMetric metric changed: [-1] -> [98]
[25-09-20 15:26:19.938][devcheckco][checkRunningD 332][thread3306][W] get useNetwork changed: [0] -> [2]
==> sleep 1
==> mqtt连接成功 [[[[mqtt线程]]]]
[MQTT] FINISH MQTTClient Connected Successed!
==> 此时刚好退出wiif状态回调
[25-09-20 15:26:24.299][wlancontro][stCallbackRec 243][thread3377][W] [end] stCallbackRecvWifiStatus: initAutoConnType: [0]

==> 接收到reset mqtt消息
[COMM] Handle One Message( m_msgId=152624653 [type:2] topic:/sys/yda/YDAS250701000005/service/reset/invoke datas:{"id":"152624653","time":1758353184653,"params":{"voice":true,"wifiPriority":1,"sealId":7076,"sealPattern":1,"u
serMobileNetwork":1,"reconnection":false,"sealType":2,"sealName":"徐佳飞-smart-奥特之章重生","useWifi":1,"useBluetooth":1,"gyroParameter":1,"needConvertVideo":true,"remoteWake":-1,"reset":false,"sleepTime":3,"offTime":-1,"re
moteLock":false}} )
==> 同步时间
==> 发送wifienable信号
[25-09-20 15:26:25.036][commandcon][handleSetDevi 945][thread3318][W] emit MainSignalManager::instance()->handleNetworkModuleCommand(name(), MDSCOMMAND_REQUEST_SET_WIFI_ENABLE
==> 信号处理
[25-09-20 15:26:25.118][wlancontro][onHandleWlanC  89][thread3372][W] onHandleWlanControllerCommands NetworkModel::CTL_NETWORK_SET_WIFI_ENABLE, useNetwork: [2], mqttStatus: [3]
[25-09-20 15:26:25.118][wlancontro][onHandleWlanC  99][thread3372][W] set initAutoConnType: [0] -> [1]
!!! 这里将回调中设置的1->0又重新设置回0->1了


==> 蓝牙连接，接收到wifi list指令
==> 触发扫描，返回扫描到的wifi列表
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-66~1~ceshi_5G~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|54|WIFILISTV2|OK~0|-67~1~DIRECT-54-HP M232 LaserJet~0~0|#][size:0 ver:2]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-68~1~TP-LINK_F41B~0~0|#][size:0 ver:2]
enter rkbt_write_cb, data: />|33|WIFISETTINGV2|2~Libawall~|#1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#
==> 接收到wifi disconnect指令
[25-09-20 15:26:52.161][rk_wifi_c_][Mock_RK_WifiD 156][thread3372][I] [===>] RK_WifiDisconnectNetwork1
[NETWK] CB WIFI STATUS : 5
[NETWK] Update Network Status (2(NSTA_WIFI) value:0)
[NETWK] Init Wait WIFI/4G AutoConnect State=[1] !(0: NULL 1:Wlan 2:4G)!
=== WPA Network List (count: 0) ===
[25-09-20 15:26:52.202][rk_wifi_c_][print_saved_i 126][thread3372][I] Total saved networks: 0
[25-09-20 15:26:52.202][rk_wifi_c_][Mock_RK_WifiG 144][thread3372][I] [exit] RK_WifiGetSavedInfo: [0]
[25-09-20 15:26:52.202][rk_wifi_c_][Mock_RK_WifiD 158][thread3372][I] [exit] RK_WifiDisconnectNetwork1
==> 重新接收到wifi list
==> 重新扫描并且返回wifi list

==> 接收到wifi config指令，进行连接
[25-09-20 15:27:02.499][bt_ble_rpc][operator()    239][thread3402][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|43|WIFISETTINGV2|1~1~Libawall~11223344|#76~2~徐佳飞-smart-奥特之章重生~-1~1|#], len: [
43]
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|43|WIFISETTINGV2|1~1~Libawall~11223344|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:1|1|Libawall|11223344|1
[25-09-20 15:27:02.582][rk_wifi_c_][Mock_RK_WifiC  68][thread3372][I] [===>] RK_WifiConnect
[25-09-20 15:27:02.586][rk_wifi_c_][Mock_RK_WifiC  72][thread3372][I] the matched wpa_network_config_list count: [0], with ssid: [Libawall], pask: [11223344]
[25-09-20 15:27:02.822][rk_wifi_c_][Mock_RK_WifiC  99][thread3372][I] [exit] RK_WifiConnect
==> 进入wifi connected回调
[NETWK] CB WIFI STATUS : 4
[NETWK] CONNETED WIFI SSID: (initAutoConnType:1) ssid:Libawall mac:94:ba:06:91:04:0b ip:192.168.48.19
[25-09-20 15:27:07.327][wlancontro][stCallbackRec 185][thread3377][W] [==>] stCallbackRecvWifiStatus: RK_WIFI_State_CONNECTED, initAutoConnType: [1], (0: NULL 1:Wlan 2:4G)
[25-09-20 15:27:07.327][wlancontro][stCallbackRec 190][thread3377][W] stCallbackRecvWifiStatus: because initAutoConnType: [1], setUdhcpcOnMetric(NTYPE_WIFI, 98)
[NETWK] WIFI SET UDHCP (type:2) metric:98
[glSetUdhcpcOnMetric] IP output: "192.168.48.19"
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 98
[25-09-20 15:27:07.393][wlancontro][stCallbackRec 236][thread3377][W] [end] stCallbackRecvWifiStatus: initAutoConnType: [1] -> [0]
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[25-09-20 15:27:08.411][wlancontro][stCallbackRec 243][thread3377][W] [end] stCallbackRecvWifiStatus: initAutoConnType: [0]
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[25-09-20 15:27:08.411][wlancontro][stCallbackRec 243][thread3377][W] [end] stCallbackRecvWifiStatus: initAutoConnType: [0]

==> 此时mqtt线程监测到mqtt连接异常
+++++m_lostDTime+++++wait 6 seconds
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-54] isDeviceLock:0 mqttStatus:1 isCharge:1 energy:100 capcity:4166mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
==> mqtt开始重新连接
[MQTT] Start Connect Server [network=[2]WIFI] [env:3] [ip_port:115.238.152.230:1887] [clientId:ba3dcc5d-a807-48ad-8379-960a0d814fa1|time=1756625455521] [mqttuser:YDAS250701000005&yda] [mqttpwd:0VyxiXaa61MSEW0emXq/Ww==]!
[MQTT] HAS CONNECT OK!!!
[MQTT] FINISH MQTTClient Connected Successed!
==> 连接成功后收到
[25-09-20 15:27:16.030][commandcon][handleSetDevi 945][thread3318][W] emit MainSignalManager::instance()->handleNetworkModuleCommand(name(), MDSCOMMAND_REQUEST_SET_WIFI_ENABLE
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:5(CTL_NETWORK_SET_WIFI_ENABLE) args:1
[25-09-20 15:27:16.032][wlancontro][onHandleWlanC  89][thread3372][W] onHandleWlanControllerCommands NetworkModel::CTL_NETWORK_SET_WIFI_ENABLE, useNetwork: [2], mqttStatus: [3]
[25-09-20 15:27:16.032][wlancontro][onHandleWlanC  99][thread3372][W] set initAutoConnType: [0] -> [1]

```

## fix bug

源头分析：之所以配置失败，是因为在蓝牙配置wifi后进入connectwifi回调时，initauto不是预期的0，而是1，导致了作为开机自动连接处理，不发送蓝牙ack消息，导致前端判断超时
inintauto的变化，是在wifi回调的末尾会置为0的，也就意味着有地方置为了1，确认了是resert消息附带wifienable后就会触发
这是新增业务逻辑，要进行代码理解
在收到reset消息后，有wifienable的指令那么就会现场进行initauto值的重置，并且调用controlOpenWlanController,initauto值的重置目的就是为了立刻生效，在wifi回调中重新进入开机自动连接的处理，
这个设计没有毛病，在wifi没有开启的情况下，就能触发自动连接，但是wifi开启的情况下，自动连接后或者配置wifi连接后，就会判断为 刚开机，需要reset后设置了initauto为1后，辨别这种情况，如果已经开启了wifi，并且initauto为1的情况，那就重置为0，这样下次进入connect回调就不会当作开机处理

```c++
void WlanController::controlOpenWlanController(bool enable)
{
    if (enable) {
        if (!m_isWifiOpen) {
            qDebug() << "[NETWK] Init WIFI!!!";
            m_isWifiOpen = true;
            RK_WifiInit(stCallbackRecvWifiStatus);
            NetworkModel::instance()->setWifiState(true);
            m_pingTimer->stop();
            ::system("ifconfig wlan0 up");

            if (m_systemStatus->initAutoConnType == 1) {
                m_waitWifiTimer->start();
            }
        }
        #if defined(YDA_SMART)
        else {
            //开机自动连接wifi，mqtt连接成功后,进入这里
            //配网时忽略wifi，再次连接，mqtt连接成功后，进入这里
            //使用4g, mqtt连接成功后，initAutoConnType = 2,进入到这里 业务逻辑，默认4g，
            if (m_systemStatus->initAutoConnType == 1) {
                m_systemStatus->initAutoConnType = 0;
            }
        }
        #endif
    } else {
        if (m_isWifiOpen) {
            m_isWifiOpen = false;
            m_waitWifiTimer->stop();
            NetworkModel::instance()->setWifiState(false);
            m_systemStatus->initAutoConnType = 0;
            ::system("ifconfig wlan0 down");
            emit updateWlanStatus(NetworkModel::NSTA_WIFI, false);
            qDebug() << "[NETWK] Deinit WIFI!!!";
        }
    }
}
```