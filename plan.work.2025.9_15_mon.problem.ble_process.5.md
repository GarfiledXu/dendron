---
id: t2m7hrt2ny2mpupf47w83tn
title: '5'
desc: ''
updated: 1758197893131
created: 1758197801845
---


```bash

root@Rockchip:/oem$ ^C
root@Rockchip:/oem$ ^C
root@Rockchip:/oem$ ./ydn-rk dbg=2
QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-root'
[SYS] App Compile Date Time :  Sep 18 2025 16:45:31
[SYS] *********[HardwareVersion:TX_SMART_MB_V05; ImageVersion:YDN_1.1] [hasWifi:1 hasVideoRecord:1 hasPhotoEncode:1 pkgTimes:2025/09/18 20:09:23:000]*********[SYS]
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
File udev_file is exist

ppp file no need change!

ppp-udev file no need change!

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
[SYS] Initing Load Module:  "NetworkModule"
killall: udhcpc: no process killed
killall: wpa_supplicant: no process killed
[NETWK] Init BT!!!
[NETWK] Start Init  "WlanController"
[NETWK] Init WIFI!!!
spdlog get env mode: default
enter only console_sink
[25-09-18 20:09:44.101][wifi_mng_w][start_polling 979][thread1707][I] [===>] start_polling_status_rk]
[25-09-18 20:09:44.103][wifi_mng_w][start_polling1082][thread1707][I] [exit] enter start_polling_status_rk]
BT BLE MAC: 4C:04:91:06:BA:94
killall: wpa_supplicant: no process killed
[25-09-18 20:09:44.175][rk_bt_c_ad][Mock_RK_BLEIn 244][thread1709][I] [==>] device: [YDAS250701000005]
[25-09-18 20:09:44.179][bt_ble_rpc][client_init    42][thread1709][W] [BtBleRpcClient] server_pid: [0], client_pid: [1633]
[SYS] Finish Load All Modules!
[SYS] Init Get m_modulesLMap: QMap(("AudioModule", 1)("EventModule", 1)("FingerModule", 1)("FrontModule", 1)("InfraredModule", 1)("MediaModule", 1)("MotorModule", 1)("MqttModule", 1)("NetworkModule", 1)("ShutdownModule", 1)(
"StampModule", 1)("UpgradeModule", 1)("UpperModule", 1))
spdlog get env mode: default
enter only console_sink
[25-09-18 20:09:44.250][ble_server][main            5][thread1724][I] === Start BtBleRpcServer ===
[25-09-18 20:09:44.257][bt_ble_rpc][server_init    48][thread1724][W] [BtBleRpcServer] server_pid: [1724], client_pid: [1633]
killall: pppd: no process killed
killall: &&: no process killed
[SYS] Start Set Finger Mode Is sleep!
[SYS] Set Finger Mode Is sleep!
[25-09-18 20:09:44.286][bt_ble_rpc][client_init    68][thread1709][I] [BtBleRpcClient] get server pid: [1724]
[25-09-18 20:09:44.287][bt_ble_rpc][register_rpc  263][thread1709][I] will open rpc lient
[25-09-18 20:09:44.295][bt_ble_rpc][operator()    268][thread1732][I] will run rpc lient io thread
[25-09-18 20:09:44.295][bt_ble_rpc][operator()    208][thread1732][I] [BtBleRpcClient] RPC connected
killall: 1: no process killed
killall: &&: no process killed
killall: -0: no process killed
killall: pppd: no process killed
[NETWK] Open 4G Power Successed!!!
[25-09-18 20:09:44.389][bt_ble_rpc][operator()    132][thread1724][D] [BtBleRpcServer] Client init:
    notify_uuid: dfd4416e-1810-47f7-8248-eb8be3dc47f9
    receive_uuid: 00009999-0000-1000-8000-00805F9B34FB
    service_uuid: 0000180A-0000-1000-8000-00805F9B34FB
    advertising_dev_name: YDAS250701000005
    advertising_service_uuid: 0000180A-0000-1000-8000-00805F9B34FB
    server_bin_name: ble_domain_rpc_server
    server_bin_dir: /oem
    server_pidfile_path: /oem/ble_domain_rpc_server
[25-09-18 20:09:44.403][my_btgatt_][L2CapAttChann  16][thread1724][D] [===>] L2CapAttChannelListener constructor
[25-09-18 20:09:44.403][my_btgatt_][L2CapAttChann  18][thread1724][D] [exit] L2CapAttChannelListener constructor
[25-09-18 20:09:44.403][my_btgatt_][ble_server_in 376][thread1724][D] [===>] ble_server_init_aysnc
[25-09-18 20:09:44.403][my_btgatt_][ble_server_in 378][thread1724][D] will launch the ble_server_running_sync_ thread
[25-09-18 20:09:44.403][my_btgatt_][ble_server_in 391][thread1724][D] end launch the ble_server_running_sync_ thread
[25-09-18 20:09:44.404][bt_ble_rpc][ble_init_sync 152][thread1709][I] BLE init done, ret: [0], cost: 15 ms
[25-09-18 20:09:44.404][rk_bt_c_ad][Mock_RK_BLEIn 268][thread1709][I] ble_init_sync success
[25-09-18 20:09:44.405][rk_bt_c_ad][Mock_RK_BLEIn 272][thread1709][I] [end] device: [YDAS250701000005]
[25-09-18 20:09:44.405][rk_bt_c_ad][Mock_RK_BleDi 297][thread1709][E] mock api: [RK_BleDisconnect], not impl!
[NETWK] Current Device Use New BLE!!!
[25-09-18 20:09:44.408][my_btgatt_][operator()    382][thread1737][I] [===>] ble_server_running_sync_ thread
[25-09-18 20:09:44.408][my_btgatt_][ble_server_ru 667][thread1737][D] [===>] ble_server_running_sync_
[25-09-18 20:09:44.409][my_btgatt_][ble_server_ru 686][thread1737][D] get hci_devid: [0]
[25-09-18 20:09:44.409][my_btgatt_][ble_server_ru 707][thread1737][D] start server running loop
[25-09-18 20:09:44.409][my_btgatt_][ble_server_ru 709][thread1737][D] [===>] server loop, count: [1]
[25-09-18 20:09:44.409][my_btgatt_][ble_server_ru 720][thread1737][D] set_advertising_data, adverting_device_name: [YDAS250701000005], advertisng_service_uuid: [0000180A-0000-1000-8000-00805F9B34FB]
[25-09-18 20:09:44.489][rk_wifi_c_][Mock_RK_WifiO  20][thread1707][I] RK_WifiOpen, ret: [0]
[WifiMng] Enabling network id=0 (Libawall)
[WifiMng] Reconnecting...
[25-09-18 20:09:44.683][my_btgatt_][set_advertisi 150][thread1737][D] after bt_string_to_uuid, uuid type: 16
[25-09-18 20:09:44.688][my_btgatt_][ble_server_ru 740][thread1737][D] Attempt 1: set_advertising_data result: 0
[25-09-18 20:09:44.688][my_btgatt_][ble_server_ru 753][thread1737][D] set_device_name: [YDAS250701000005]
[25-09-18 20:09:44.688][my_btgatt_][ble_server_ru 756][thread1737][D] mainloop_init
[25-09-18 20:09:44.688][my_btgatt_][ble_server_ru 761][thread1737][D] create mainloop_notify_fd_: [12]
[25-09-18 20:09:44.688][my_btgatt_][ble_server_ru 776][thread1737][D] success, add mainloop_notify_fd_ to mainloop_fd, re: [0]
[25-09-18 20:09:44.688][my_btgatt_][start_listen_ 269][thread1737][D] [===>] start_listen_and_accept
[25-09-18 20:09:44.688][my_btgatt_][create_socket  55][thread1737][D] [===>] create_socket_
[25-09-18 20:09:44.688][my_btgatt_][create_socket  66][thread1737][D] create listen_sock_: [13]
[25-09-18 20:09:44.688][my_btgatt_][create_socket  79][thread1737][D] bind listen_sock_: [13]
[25-09-18 20:09:44.688][my_btgatt_][create_socket  88][thread1737][D] setsockopt listen_sock_: [13]
[25-09-18 20:09:44.688][my_btgatt_][create_socket  90][thread1737][D] start listen_sock_: [13]
[25-09-18 20:09:44.688][my_btgatt_][create_socket  97][thread1737][D] [exit] create_socket_
[25-09-18 20:09:44.688][my_btgatt_][start_listen_ 319][thread1737][D] mainloop_add_fd listen_sock_: [13]
[25-09-18 20:09:44.689][my_btgatt_][start_listen_ 332][thread1737][D] [exit] start_listen_and_accept
[25-09-18 20:09:44.689][my_btgatt_][ble_server_ru 820][thread1737][D] [===>] mainloop run
[25-09-18 20:09:45.154][wifi_mng_w][operator()   1073][thread1712][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-18 20:09:45.155][wifi_mng_w][operator()   1075][thread1712][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[25-09-18 20:09:46.296][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:09:46.297][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[NETWK] Urc Cache: AT+QCFG="urc/cache",0


OK


[NETWK] 4G EC200 TCP CLOSE & GPS OPEN OK!
[NETWK] EC200 COPS:AT+COPS?


+COPS: 0,0,"CHINA MOBILE",7



OK


[NETWK] EC200 QCCID:AT+QCCID


+QCCID: 89860855102481537262



OK


[NETWK] EC200 servingcell:AT+QENG="servingcell"


+QENG: "servingcell","NOCONN","LTE","FDD",460,00,4CBEB84,24,3590,8,3,3,57B9,-99,-20,-62,-4,27



OK


[NETWK] Get CREG Is OK!
No defaultroute line to comment in "/etc/ppp/peers/quectel-ppp"
[25-09-18 20:09:48.175][wifi_mng_w][operator()   1073][thread1712][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-18 20:09:48.175][wifi_mng_w][operator()   1075][thread1712][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[25-09-18 20:09:48.296][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:09:48.297][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[NETWK] Init EC200U 4G Check IP Delays [1]
[25-09-18 20:09:49.182][wifi_mng_w][operator()   1073][thread1712][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-18 20:09:49.183][wifi_mng_w][operator()   1075][thread1712][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[NETWK] Init EC200U 4G Check IP Delays [2]
[25-09-18 20:09:50.190][wifi_mng_w][operator()   1073][thread1712][I] [on loop] start_polling_status_rk, enter callback, rk_state: [CONNECTED]
[NETWK] CB WIFI STATUS : 4
=== WPA Network List (count: 1) ===
Index: 0
  ID:     0
  SSID:   Libawall
  BSSID:  any
  FLAGS:        [CURRENT]
-----------------------------------
[25-09-18 20:09:50.198][rk_wifi_c_][print_saved_i 124][thread1712][I] Total saved networks: 1
[25-09-18 20:09:50.198][rk_wifi_c_][print_saved_i 134][thread1712][I]   [#0] SSID: 'Libawall', BSSID: 'any', State: '[CURRENT]'
[25-09-18 20:09:50.198][rk_wifi_c_][Mock_RK_WifiG 142][thread1712][I] [exit] RK_WifiGetSavedInfo: [0]
[NETWK] **********************
[NETWK] updateSaveApInfo {id=0 ssid=Libawall bssid=any state:[CURRENT]}
[NETWK] **********************
[NETWK] CONNETED WIFI SSID: (initAutoConnType:1) ssid:Libawall mac:94:ba:06:91:04:0b ip:
[25-09-18 20:09:50.198][wlancontro][stCallbackRec 182][thread1712][W] set initAutoConnType 98
[NETWK] WIFI SET UDHCP (type:2) metric:98
[glSetUdhcpcOnMetric] IP output: ""
[glSetUdhcpcOnMetric] Interface "wlan0" has no IP, start udhcpc
udhcpc: started, v1.27.2
cat /tmp/metric success
udhcpc: sending discover
udhcpc: sending select for 192.168.49.26
udhcpc: lease of 192.168.49.26 obtained, lease time 43200
cat /tmp/metric success
deleting routers
set wlan0 metric : 98
[25-09-18 20:09:50.299][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:09:50.299][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
adding dns 223.5.5.5
adding dns 223.6.6.6
[NETWK] Init EC200U 4G Check IP Delays [3]
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[0 sig:0, 0 sig:-62] isDeviceLock:0 mqttStatus:1 isCharge:1 energy:98 capcity:4113mV curent:-28nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:0 isGpySync:0 mediaUpNums:0}
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[glSetUdhcpcOnMetric] IP output: "192.168.49.26"
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 98
[NETWK] EC200U 4G PPPD Successed! & Get 4G Address : "          inet addr:10.199.142.163  P-t-P:10.64.64.64  Mask:255.255.255.255\n"
[NETWK] Update Network Status (1(NSTA_FOURG) value:1)
[MEDIA] No Log Files To Upload!!!
[25-09-18 20:09:52.300][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:09:52.300][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Link [Wlan] [https://uteydaapi.libawall.com/equipment/link] OK!
[MQTT] Update Get [Can:1 isCloudDisableMqtt:0] To Enable Connecting Mqtt-Server!
[U**I] onHandleMainUICommand :  56
[MQTT] Start Connect Server [network=[2]WIFI] [env:3] [ip_port:115.238.152.230:1887] [clientId:ba3dcc5d-a807-48ad-8379-960a0d814fa1|time=1756625455521] [mqttuser:YDAS250701000005&yda] [mqttpwd:0VyxiXaa61MSEW0emXq/Ww==]!
[MQTT] HAS CONNECT OK!!!
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/event/fingerprint_login/post_reply]
[EVENT] Get KeysStatus++++++++press:1 motor:0 gate_on:1 gate_off:0
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/fingerprint_entering/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/change_seal/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/change_seal_finish/invoke]
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 98
[25-09-18 20:09:54.295][wlancontro][stCallbackRec 226][thread1712][W] will reset m_systemStatus->initAutoConnType to 0, current: [0]
[25-09-18 20:09:54.301][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:09:54.301][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/begin_seal/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/user_login/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/user_logout/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/equipment_lock/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/examine_result/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/cancel_operation/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/event/document_state_uploading_event/post_reply]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/ota/firmware/upgrade]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/ota/firmware/request_reply]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/begin_ink/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/begin_ink_result/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/with_ink/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/fingerprint_import/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/event/fingerprint_import/post_reply]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/stop_privilege_seal/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/voice/invoke]
[25-09-18 20:09:55.314][wifi_mng_w][operator()   1075][thread1712][I] [on loop] start_polling_status_rk, break callback, rk_state: [CONNECTED]
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/remoteLock/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/find/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/seal_stretch_out/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/seal_retraction/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/secret/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/fingerprint_auth/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/network_configuration/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/wifi_list/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/seal_pattern/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/wifi_priority/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/singing/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/module/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/click_result/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/device_config_set/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/privilege_data/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/privilege_data_result/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/reset/invoke]
[MQTT] FINISH MQTTClient Connected Successed!
[U**I] onHandleMainUICommand :  57
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/ota/firmware/request) Msg:{
    "id": "2",
    "params": {
    },
    "time": 1758197396,
    "version": "1.5.6"
}

[25-09-18 20:09:56.302][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:09:56.303][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/fingerprint_import/post) Msg:{
    "id": "1",
    "params": {
        "init": true
    }
}

[MQTT] Message arrived [recvcnt:1] [mainqueues_size:0] [len:361] topicName: [<XԥP�H]
[MQTT] Message arrived [recvcnt:2] [mainqueues_size:0] [len:96] topicName: [�b��b�/YDAS250701000005/event/fingerprint_import/post_reply]
[COMM] Handle One Message( m_msgId=200956261 [type:2] topic:/sys/yda/YDAS250701000005/service/reset/invoke datas:{"id":"200956261","time":1758197396261,"params":{"voice":true,"wifiPriority":1,"sealId":7076,"sealPattern":1,"u
serMobileNetwork":1,"reconnection":false,"sealType":2,"sealName":"徐佳飞-smart-奥特之章重生","useWifi":1,"useBluetooth":1,"gyroParameter":1,"needConvertVideo":true,"remoteWake":-1,"reset":false,"sleepTime":3,"offTime":-1,"re
moteLock":false}} )
[COMM] Sync Date Time(2025-09-18 20:09:57) to Local! Get Local(2025-09-18 20:09:57)
[25-09-18 20:09:57.022][commandcon][syncLocalDate 118][thread1653][W] Server epoch: [1758197396261]
[25-09-18 20:09:57.022][commandcon][syncLocalDate 119][thread1653][W] UTC time: [2025-09-18 12:09:56]
[25-09-18 20:09:57.023][commandcon][syncLocalDate 120][thread1653][W] Local time: [2025-09-18 20:09:57]
[U**I] onHandleMainUICommand :  0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/service/reset/invoke_reply) Msg:{
    "code": 0,
    "data": {
        "status": 1
    },
    "id": "200956261",
    "success": true,
    "time": 1758197397
}

[SYS] Set
STAMPINSTALL:1
STAMPNAME:徐佳飞-smart-奥特之章重生
SEALID:7076
ISUPGRADE:0
SEALPATTERN:1
CAP:100
VOICE:1
LOCK:0
ENVIRONMENT:3
SEALTYPE:2
PUBLISH:1
SEALDISTANCE:0
AXISOFFSET:0
DEVICEINFO:YDAS250701000005
MQTTUSERNAME:YDAS250701000005&yda
MQTTPASSWD:0VyxiXaa61MSEW0emXq/Ww==
CLIENTID:ba3dcc5d-a807-48ad-8379-960a0d814fa1|time=1756625455521
KEY:33GVH9CVLD46B3JF7D
VERSION:1.5.6
IP&PORT:115.238.152.230:1887
API:https://uteydaapi.libawall.com
UPGRADETYPE:0
IS_OS_UPGRADE:0
SW_WIFI:1
SW_BLUETOOTH:1
SW_FOURG:1
SUPERUSER:1-
PRIORITY:1
SLEEP_TIME:3
OFF_TIME:-1

[EVENT] Module Recv Commands (send: CommandController cmd:21))
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "3",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197397,
    "version": "1.5.6"
}

[NETWK] Handle Wlan Controller Cmd: send:CommandController type:5(CTL_NETWORK_SET_WIFI_ENABLE) args:1
[25-09-18 20:09:57.114][wlancontro][onHandleWlanC  92][thread1707][W] set initAutoConnType from: [0], to: [1]
[NETWK] Already Init BT!
[NETWK] Current Device Use New BLE!!!
killall: rk_mpi_ao_test: no process killed
[SYS] Audio Cmd End:  "rk_mpi_ao_test -i /usr/audio/10001.wav --sound_card_name=hw:0,0 --device_ch=2 --device_rate=48000 --input_rate=48001 --input_ch=2 --set_volume=50 &"
cmd parse result:
input  file name      : /usr/audio/10001.wav
output file name      : (null)
loop count            : 1
channel number        : 1
open sound rate       : 48000
open sound channel    : 2
input stream rate     : 48001
input channel         : 2
bit_width             : 16
frame_number          : 4
frame_length          : 1024
sound card name       : hw:0,0
device id             : 0
set volume curve      : 0
set volume            : 50
set mute              : 0
set track_mode        : 0
get volume            : 0
get mute              : 0
get track_mode        : 0
query stat            : 0
pause and resume chn  : 0
save file             : 0
query save file stat  : 0
clear buf             : 0
get attribute         : 0
clear attribute       : 0
set loopback mode     : 0
vqe enable            : 0
adec input file name  : (null)
rockit log path (null), log_size = 0, can use export rt_log_path=, export rt_log_size= change
log_file = (nil)
RTVersion        12:09:57-240 {dump              :064} ---------------------------------------------------------
RTVersion        12:09:57-240 {dump              :065} rockit version: git-7324009fa Fri Sep 1 14:55:53 2023 +0800
RTVersion        12:09:57-241 {dump              :066} rockit building: built- 2023-09-01 14:59:16
RTVersion        12:09:57-241 {dump              :067} ---------------------------------------------------------
(null)           12:09:57-241 {log_level_init    :206}

 please use echo name=level > /tmp/rt_log_level set log level
        name: all cmpi mb sys vdec venc rgn vpss vgs tde avs wbc vo vi ai ao aenc adec
        log_level: 0 1 2 3 4 5 6

rockit default level 4, can use export rt_log_level=x, x=0,1,2,3,4,5,6 change
(null)           12:09:57-242 {read_log_level    :097} text is all=4
(null)           12:09:57-242 {read_log_level    :099} module is all, log_level is 4
cmpi             12:09:57-246 {main              :823} start running loop count  = 0
(null)           12:09:57-248 {monitor_log_level :148} #Start monitor_log_level thread, arg:(nil)
cmpi             12:09:57-271 {test_init_mpi_ao  :226} Set volume curve type: 0
cmpi             12:09:57-272 {commandThread     :376} test info : mute = 0, volume = 50
cmpi             12:09:57-273 {sendDataThread    :309} params->s32ChnIndex : 0
[COMM] Handle One Message( m_msgId=1 [type:2] topic:/sys/yda/YDAS250701000005/event/fingerprint_import/post_reply datas:{"code":0,"data":{"fingerprint":[],"privilege":[]},"success":true,"id":"1","time":1758197396329} )
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "4",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197397,
    "version": "1.5.6"
}

[MEDIA] Init Get photoUpNums: 0 videoUpNums:0
[25-09-18 20:09:58.660][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:09:58.661][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
cmpi             12:09:59-162 {sendDataThread    :352} eof
cmpi             12:09:59-189 {main              :828} end running loop count  = 0
RKSockServer     12:09:59-248 {start             :162} accept failed
(null)           12:09:59-251 {monitor_log_level :189} monitor_log_level quit
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-62] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4118mV curent:-61nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:10:00.662][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:00.663][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:02.663][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:02.664][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:04.664][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:04.665][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "5",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197405,
    "version": "1.5.6"
}

[25-09-18 20:10:06.665][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:06.666][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-63] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:10:08.667][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:08.668][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:10.668][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:10.669][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0

[25-09-18 20:10:12.669][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:12.669][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "6",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197413,
    "version": "1.5.6"
}

[25-09-18 20:10:14.670][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:14.670][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-61] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4129mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:10:16.671][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:16.672][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:18.672][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:18.673][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:20.674][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:20.675][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "7",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197421,
    "version": "1.5.6"
}

[25-09-18 20:10:22.675][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:22.676][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:22.869][my_btgatt_][operator()    276][thread1737][D] [===>] l2cap listen socket cb, fd: [13], event: [1]
[25-09-18 20:10:22.869][my_btgatt_][operator()    291][thread1737][D] accpet success, client fd: [14]
[25-09-18 20:10:22.869][my_btgatt_][operator()    295][thread1737][D] connect from client address: [7B:F1:D8:9A:90:14], listen_fd: [13]
[25-09-18 20:10:22.869][my_btgatt_][operator()    299][thread1737][D] set the l2cap flag_is_connected_ to: [true]
[25-09-18 20:10:22.869][my_btgatt_][operator()    302][thread1737][D] will execute accept_callback_
[25-09-18 20:10:22.869][my_btgatt_][operator()    783][thread1737][D] [===>] l2cap_att_channel_listener_::accept_callback, fd: [14]
[25-09-18 20:10:22.869][my_btgatt_][operator()    785][thread1737][D] will server_create
[25-09-18 20:10:22.871][my_btgatt_][operator()    794][thread1737][I] will service_builder
populate_rkbt_service (RK-style BLE)
[NETWK] GET BT STATUS : 1 [CONNECTED] [name:YDAS250701000005 addr:94:BA:06:91:04:0C]
[25-09-18 20:10:22.872][rk_bt_c_ad][operator()    248][thread1732][W] [BtBleRpcClient] State changed, addr: 94:BA:06:91:04:0C, dev: YDAS250701000005, state: 1
[NETWK] Update Network Status (3(NSTA_BT) value:1)
[NETWK] Init Wait WIFI/4G AutoConnect State=[1] !(0: NULL 1:Wlan 2:4G)!
[25-09-18 20:10:22.874][bt_ble_rpc][bluez_call_st 424][thread1737][I] bluez_call_state_sync_ timeout for cmd: [ble_state], ret code: [0], cost time ms: [2]
[25-09-18 20:10:22.874][my_btgatt_][operator()    805][thread1737][D] [exit] l2cap_att_channel_listener_::accept_callback
[25-09-18 20:10:22.874][my_btgatt_][operator()    304][thread1737][D] end execute accept_callback_
[25-09-18 20:10:22.874][my_btgatt_][operator()    313][thread1737][D] [exit] l2cap listen socket cb, fd: [13], event: [1]
[25-09-18 20:10:22.989][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 3

[25-09-18 20:10:22.989][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x02

[25-09-18 20:10:22.989][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:exchange_mtu_cb() MTU exchange
complete, with MTU: 185

[25-09-18 20:10:22.989][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x03

[25-09-18 20:10:22.990][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 03 00 02                                         ...

[25-09-18 20:10:23.079][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 7

[25-09-18 20:10:23.080][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x10

[25-09-18 20:10:23.080][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:read_by_grp_type_cb() Read By G
rp Type - start: 0x0001 end: 0xffff

[25-09-18 20:10:23.080][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x11

[25-09-18 20:10:23.080][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 11 06 01 00 06 00 00 18 07 00 0a 00 01 18 0b 00  ................

[25-09-18 20:10:23.080][my_btgatt_][att_debug_cb  213][thread1737][D] att:   10 00 0a 18                                      ....

[25-09-18 20:10:23.169][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 7

[25-09-18 20:10:23.169][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x10

[25-09-18 20:10:23.170][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:read_by_grp_type_cb() Read By G
rp Type - start: 0x0011 end: 0xffff

[25-09-18 20:10:23.170][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x01

[25-09-18 20:10:23.170][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 01 10 11 00 0a                                   .....

[25-09-18 20:10:23.259][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 7

[25-09-18 20:10:23.260][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x08

[25-09-18 20:10:23.260][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:read_by_type_cb() Read By Type
- start: 0x0007 end: 0x000a

[25-09-18 20:10:23.260][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x09

[25-09-18 20:10:23.260][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 09 07 08 00 22 09 00 05 2a                       ...."...*

[25-09-18 20:10:23.349][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 5

[25-09-18 20:10:23.349][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x04

[25-09-18 20:10:23.350][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:find_info_cb() Find Info - star
t: 0x000a end: 0x000a

[25-09-18 20:10:23.350][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x05

[25-09-18 20:10:23.350][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 05 01 0a 00 02 29                                .....)

[25-09-18 20:10:23.470][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 5

[25-09-18 20:10:23.470][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x12

[25-09-18 20:10:23.470][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000a

[25-09-18 20:10:23.470][my_btgatt_][gatt_svc_chng 345][thread1737][D] Service Changed CCC Write called

[25-09-18 20:10:23.470][my_btgatt_][gatt_svc_chng 365][thread1737][D] Service Changed Enabled: true

[25-09-18 20:10:23.470][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-18 20:10:23.470][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x13

[25-09-18 20:10:23.470][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 13                                               .

[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-61] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4129mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:10:23.559][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 7

[25-09-18 20:10:23.560][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x08

[25-09-18 20:10:23.560][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:read_by_type_cb() Read By Type
- start: 0x000b end: 0x0010

[25-09-18 20:10:23.560][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x09

[25-09-18 20:10:23.560][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 09 07 0c 00 1a 0d 00 99 99                       .........

[25-09-18 20:10:23.679][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 7

[25-09-18 20:10:23.680][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x08

[25-09-18 20:10:23.680][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:read_by_type_cb() Read By Type
- start: 0x000e end: 0x0010

[25-09-18 20:10:23.680][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x09

[25-09-18 20:10:23.680][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 09 15 0e 00 1a 0f 00 f9 47 dc e3 8b eb 48 82 f7  ........G....H..

[25-09-18 20:10:23.680][my_btgatt_][att_debug_cb  213][thread1737][D] att:   47 10 18 6e 41 d4 df                             G..nA..

[25-09-18 20:10:23.769][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 7

[25-09-18 20:10:23.770][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x08

[25-09-18 20:10:23.770][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:read_by_type_cb() Read By Type
- start: 0x000b end: 0x0010

[25-09-18 20:10:23.770][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x01

[25-09-18 20:10:23.770][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 01 08 0b 00 0a                                   .....

[25-09-18 20:10:23.859][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 5

[25-09-18 20:10:23.860][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x04

[25-09-18 20:10:23.860][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:find_info_cb() Find Info - star
t: 0x0010 end: 0x0010

[25-09-18 20:10:23.860][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x05

[25-09-18 20:10:23.860][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 05 01 10 00 02 29                                .....)

[25-09-18 20:10:24.009][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 5

[25-09-18 20:10:24.009][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x12

[25-09-18 20:10:24.009][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x0010

Wifi: Service Changed CCC Write called [1:0]
Wifi: Service Changed Enabled: true
[25-09-18 20:10:24.009][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-18 20:10:24.009][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x13

[25-09-18 20:10:24.010][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 13                                               .

[25-09-18 20:10:24.677][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:24.678][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:24.821][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 87

[25-09-18 20:10:24.821][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x12

[25-09-18 20:10:24.821][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|78|DEVICEMATCHV2|徐佳飞~83185~1758197423~c736e1a8d5b90b6383d882e2628316e9~3~|#7m/home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shareA╔
[rkbt_write_cb] offset: 0, len: 84
Data: 2F 3E 7C 37 38 7C 44 45 56 49 43 45 4D 41 54 43 48 56 32 7C E5 BE 90 E4 BD B3 E9 A3 9E 7E 38 33 31 38 35 7E 31 37 35 38 31 39 37 34 32 33 7E 63 37 33 36 65 31 61 38 64 35 62 39 30 62 36 33 38 33 64 38 38 32 65 32 36 32
 38 33 31 36 65 39 7E 33 7E 7C 23
[25-09-18 20:10:24.821][bt_ble_rpc][operator()    239][thread1737][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|78|DEVICEMATCHV2|徐佳飞~83185~1758197423~c736e1a8d5b90b6383d882e2628316e9~3~|#7m/home/
xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shareA╔], len: [84]
[25-09-18 20:10:24.822][rk_bt_c_ad][operator()    252][thread1732][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|78|DEVICEMATCHV2|徐佳飞~83185~1758197423~c736e1a8d5b90b6383d882e262831
6e9~3~|#, len: 84
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|78|DEVICEMATCHV2|徐佳飞~83185~1758197423~c736e1a8d5b90b6383d882e2628316e9~3~|# )
[25-09-18 20:10:24.826][bt_ble_rpc][bluez_call_re 395][thread1737][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [3]
[25-09-18 20:10:24.826][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-18 20:10:24.826][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x13

[25-09-18 20:10:24.827][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 13                                               .

[COMM] Sync Date Time(2025-09-18 20:10:24) to Local! Get Local(2025-09-18 20:10:24)
[25-09-18 20:10:24.022][commandcon][syncLocalDate 118][thread1653][W] Server epoch: [1758197423]
[25-09-18 20:10:24.022][commandcon][syncLocalDate 119][thread1653][W] UTC time: [1970-01-21 08:23:17]
[25-09-18 20:10:24.022][commandcon][syncLocalDate 120][thread1653][W] Local time: [2025-09-18 20:10:24]
[COMM] USER LOGIN (staffId:83185 staffName:徐佳飞) !
[U**I] onHandleMainUICommand :  2
[25-09-18 20:10:24.105][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|30|DEVICEMATCHV2|OK~0|徐佳飞~83185~4|#]
[25-09-18 20:10:24.105][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:24.105][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [44]
[25-09-18 20:10:24.106][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:24.106][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 30 7c 44 45 56 49 43 45 4d  .../>|30|DEVICEM

[25-09-18 20:10:24.106][my_btgatt_][att_debug_cb  213][thread1737][D] att:   41 54 43 48 56 32 7c 4f 4b 7e 30 7c e5 be 90 e4  ATCHV2|OK~0|....

[25-09-18 20:10:24.107][my_btgatt_][att_debug_cb  213][thread1737][D] att:   bd b3 e9 a3 9e 7e 38 33 31 38 35 7e 34 7c 23     .....~83185~4|#

[25-09-18 20:10:24.107][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|30|DEVICEMATCHV2|OK~0|徐佳飞~83185~4|#]
[25-09-18 20:10:24.107][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|30|DEVICEMATCHV2|OK~0|徐佳飞~83185~4|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|30|DEVICEMATCHV2|OK~0|徐佳飞~83185~4|#][size:0 ver:2]
killall: rk_mpi_ao_test: no process killed
[SYS] Audio Cmd End:  "rk_mpi_ao_test -i /usr/audio/10012.wav --sound_card_name=hw:0,0 --device_ch=2 --device_rate=48000 --input_rate=48001 --input_ch=2 --set_volume=50 &"
cmd parse result:
input  file name      : /usr/audio/10012.wav
output file name      : (null)
loop count            : 1
channel number        : 1
open sound rate       : 48000
open sound channel    : 2
input stream rate     : 48001
input channel         : 2
bit_width             : 16
frame_number          : 4
frame_length          : 1024
sound card name       : hw:0,0
device id             : 0
set volume curve      : 0
set volume            : 50
set mute              : 0
set track_mode        : 0
get volume            : 0
get mute              : 0
get track_mode        : 0
query stat            : 0
pause and resume chn  : 0
save file             : 0
query save file stat  : 0
clear buf             : 0
get attribute         : 0
clear attribute       : 0
set loopback mode     : 0
vqe enable            : 0
adec input file name  : (null)
rockit log path (null), log_size = 0, can use export rt_log_path=, export rt_log_size= change
log_file = (nil)
RTVersion        12:10:24-242 {dump              :064} ---------------------------------------------------------
RTVersion        12:10:24-242 {dump              :065} rockit version: git-7324009fa Fri Sep 1 14:55:53 2023 +0800
RTVersion        12:10:24-243 {dump              :066} rockit building: built- 2023-09-01 14:59:16
RTVersion        12:10:24-243 {dump              :067} ---------------------------------------------------------
(null)           12:10:24-243 {log_level_init    :206}

 please use echo name=level > /tmp/rt_log_level set log level
        name: all cmpi mb sys vdec venc rgn vpss vgs tde avs wbc vo vi ai ao aenc adec
        log_level: 0 1 2 3 4 5 6

rockit default level 4, can use export rt_log_level=x, x=0,1,2,3,4,5,6 change
(null)           12:10:24-244 {read_log_level    :097} text is all=4
(null)           12:10:24-244 {read_log_level    :099} module is all, log_level is 4
cmpi             12:10:24-248 {main              :823} start running loop count  = 0
(null)           12:10:24-251 {monitor_log_level :148} #Start monitor_log_level thread, arg:(nil)
[25-09-18 20:10:24.254][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 92

[25-09-18 20:10:24.254][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x12

[25-09-18 20:10:24.254][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share!╔
[rkbt_write_cb] offset: 0, len: 89
Data: 2F 3E 7C 37 31 7C 44 45 56 49 43 45 49 4E 46 4F 53 45 54 56 32 7C 2D 31 7E 33 7E 31 7E 31 7E 31 7E 31 7E 31 7E 31 7E 31 7E 37 30 37 36 7E 32 7E E5 BE 90 E4 BD B3 E9 A3 9E 2D 73 6D 61 72 74 2D E5 A5 A5 E7 89 B9 E4 B9 8B
 E7 AB A0 E9 87 8D E7 94 9F 7E 2D 31 7E 31 7C 23
[25-09-18 20:10:24.254][bt_ble_rpc][operator()    239][thread1737][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share!╔], len: [89]
[25-09-18 20:10:24.256][rk_bt_c_ad][operator()    252][thread1732][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生
~-1~1|#, len: 89
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|# )
[25-09-18 20:10:24.259][bt_ble_rpc][bluez_call_re 395][thread1737][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [4]
[25-09-18 20:10:24.259][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-18 20:10:24.260][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x13

[25-09-18 20:10:24.260][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 13                                               .

[SYS] Set
STAMPINSTALL:1
STAMPNAME:徐佳飞-smart-奥特之章重生
SEALID:7076
ISUPGRADE:0
SEALPATTERN:1
CAP:100
VOICE:1
LOCK:0
ENVIRONMENT:3
SEALTYPE:2
PUBLISH:1
SEALDISTANCE:0
AXISOFFSET:0
DEVICEINFO:YDAS250701000005
MQTTUSERNAME:YDAS250701000005&yda
MQTTPASSWD:0VyxiXaa61MSEW0emXq/Ww==
CLIENTID:ba3dcc5d-a807-48ad-8379-960a0d814fa1|time=1756625455521
KEY:33GVH9CVLD46B3JF7D
VERSION:1.5.6
IP&PORT:115.238.152.230:1887
API:https://uteydaapi.libawall.com
UPGRADETYPE:0
IS_OS_UPGRADE:0
SW_WIFI:1
SW_BLUETOOTH:1
SW_FOURG:1
SUPERUSER:1-
PRIORITY:1
SLEEP_TIME:3
OFF_TIME:-1

cmpi             12:10:24-273 {test_init_mpi_ao  :226} Set volume curve type: 0
cmpi             12:10:24-274 {commandThread     :376} test info : mute = 0, volume = 50
cmpi             12:10:24-275 {sendDataThread    :309} params->s32ChnIndex : 0
[25-09-18 20:10:24.317][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|25|DEVICEINFOSETV2|OK~0|#]
[25-09-18 20:10:24.318][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:24.318][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [28]
[25-09-18 20:10:24.318][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:24.319][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 32 35 7c 44 45 56 49 43 45 49  .../>|25|DEVICEI

[25-09-18 20:10:24.319][my_btgatt_][att_debug_cb  213][thread1737][D] att:   4e 46 4f 53 45 54 56 32 7c 4f 4b 7e 30 7c 23     NFOSETV2|OK~0|#

[25-09-18 20:10:24.320][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|25|DEVICEINFOSETV2|OK~0|#]
[25-09-18 20:10:24.320][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|25|DEVICEINFOSETV2|OK~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|25|DEVICEINFOSETV2|OK~0|#][size:0 ver:2]
[25-09-18 20:10:24.405][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 30

[25-09-18 20:10:24.405][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x12

[25-09-18 20:10:24.405][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|27|PRIVILEGEDATAV2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share!╔
[rkbt_write_cb] offset: 0, len: 27
Data: 2F 3E 7C 32 37 7C 50 52 49 56 49 4C 45 47 45 44 41 54 41 56 32 7C 31 30 30 7C 23
[25-09-18 20:10:24.406][bt_ble_rpc][operator()    239][thread1737][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|27|PRIVILEGEDATAV2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share!╔], len: [27]
[25-09-18 20:10:24.406][rk_bt_c_ad][operator()    252][thread1732][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|27|PRIVILEGEDATAV2|100|#, len: 27
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|27|PRIVILEGEDATAV2|100|# )
[25-09-18 20:10:24.408][bt_ble_rpc][bluez_call_re 395][thread1737][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [2]
[25-09-18 20:10:24.408][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-18 20:10:24.410][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x13

[25-09-18 20:10:24.411][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 13                                               .

[25-09-18 20:10:24.531][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|99|PRIVILEGEDATAV2|OK~0|0~1~0~[]|#]
[25-09-18 20:10:24.531][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:24.531][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [37]
[25-09-18 20:10:24.532][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:24.532][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 39 39 7c 50 52 49 56 49 4c 45  .../>|99|PRIVILE

[25-09-18 20:10:24.532][my_btgatt_][att_debug_cb  213][thread1737][D] att:   47 45 44 41 54 41 56 32 7c 4f 4b 7e 30 7c 30 7e  GEDATAV2|OK~0|0~

[25-09-18 20:10:24.533][my_btgatt_][att_debug_cb  213][thread1737][D] att:   31 7e 30 7e 5b 5d 7c 23                          1~0~[]|#

[25-09-18 20:10:24.533][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|99|PRIVILEGEDATAV2|OK~0|0~1~0~[]|#]
[25-09-18 20:10:24.533][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|99|PRIVILEGEDATAV2|OK~0|0~1~0~[]|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|99|PRIVILEGEDATAV2|OK~0|0~1~0~[]|#][size:0 ver:2]
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-61] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4129mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
cmpi             12:10:25-620 {sendDataThread    :352} eof
cmpi             12:10:25-648 {main              :828} end running loop count  = 0
[25-09-18 20:10:25.842][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:25.843][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0

[25-09-18 20:10:26.024][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 23

[25-09-18 20:10:26.025][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x12

[25-09-18 20:10:26.025][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|20|WIFILISTV2|~|#2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share
[rkbt_write_cb] offset: 0, len: 20
Data: 2F 3E 7C 32 30 7C 57 49 46 49 4C 49 53 54 56 32 7C 7E 7C 23
[25-09-18 20:10:26.025][bt_ble_rpc][operator()    239][thread1737][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|20|WIFILISTV2|~|#2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share], len: [20]
[25-09-18 20:10:26.025][rk_bt_c_ad][operator()    252][thread1732][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|20|WIFILISTV2|~|#, len: 20
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|20|WIFILISTV2|~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:2(CTL_NETWORK_SCAN_APS) args:1
m_isScan= false
[25-09-18 20:10:26.027][bt_ble_rpc][bluez_call_re 395][thread1737][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [2]
[25-09-18 20:10:26.027][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-18 20:10:26.028][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x13

[25-09-18 20:10:26.028][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 13                                               .

[25-09-18 20:10:26.035][rk_wifi_c_][Mock_RK_WifiS  41][thread1707][I] RK_WifiScan, ret: [true]
Run RK_WifiScan :  true
[NETWK] CB WIFI STATUS : 8
RKSockServer     12:10:26-250 {start             :162} accept failed
(null)           12:10:26-253 {monitor_log_level :189} monitor_log_level quit
Scan complete in 522 ms, found networks:
14:d8:64:8d:f4:1d       5180    -44     [WPA2-PSK-CCMP][ESS]    TP-LINK_5G_F41B
e2:c3:22:6a:b0:8f       5745    -54     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
cc:d8:43:09:96:52       5785    -65     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ceshi_5G
08:9b:4b:42:a9:62       5240    -48     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:a9:62       5240    -48     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:62       5240    -48     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:ab:27       5200    -51     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:ab:27       5200    -51     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:ab:27       5200    -52     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:b3:d3:79       5180    -59     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:b3:d3:79       5180    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:b3:d3:79       5180    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:bb:b1:54       5240    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:a9:56       5220    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:a9:56       5220    -63     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:54       5240    -63     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b1:54       5240    -63     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:56       5220    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
b8:08:d7:18:81:bc       2437    -45     [WPA2-PSK-CCMP][ESS]    zzwifi
e2:c3:22:6a:b0:8b       2412    -44     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
12:9b:4b:42:a9:55       2437    -59     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:a9:55       2437    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:ab:18       5220    -68     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:b3:d3:78       2437    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:a9:55       2437    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
08:9b:4b:42:ab:18       5220    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:bb:b1:70       5220    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:18       5220    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
14:d8:64:8d:f4:1b       2412    -51     [WPA2-PSK-CCMP][ESS]    TP-LINK_F41B
cc:d8:43:09:96:51       2422    -56     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ceshi
0e:9b:4b:b3:d3:78       2437    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:53       2462    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
0e:9b:4b:bb:b1:70       5220    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:bb:b1:70       5220    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:bb:b1:68       5200    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:b4:d8       5200    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:bb:b1:68       5200    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b1:68       5200    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:42:a9:61       2412    -54     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
0e:9b:4b:42:b4:d8       5200    -73     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:b4:d8       5200    -73     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:a9:61       2412    -55     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:61       2412    -55     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:bb:b1:6f       2462    -63     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:b3:d3:7f       5805    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:b3:d3:7f       5805    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:b3:d3:7f       5805    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:42:ab:11       2437    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:ab:12       5745    -76     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:ab:12       5745    -76     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:ab:12       5745    -76     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:42:b4:d7       2437    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b1:67       2462    -66     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
0e:9b:4b:bb:b1:6f       2462    -66     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:42:b4:de       5180    -78     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:42:b4:d7       2437    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:ab:0c       5745    -79     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
[25-09-18 20:10:26.602][rk_wifi_c_][Mock_RK_WifiS  50][thread1712][I] get scan result success
[25-09-18 20:10:26.629][rk_wifi_c_][Mock_RK_WifiS  60][thread1712][I] RK_WifiScanResults, result: [{"ssid":"TP-LINK_5G_F41B","rssi":-44,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-54,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"ceshi_5G","rssi":-65,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-48,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-48,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-48,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-51,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-51,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-52,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-59,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-63,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-63,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-63,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"zzwifi","rssi":-45,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-44,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-59,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-68,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"TP-LINK_F41B","rssi":-51,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ceshi","rssi":-56,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-54,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-73,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-73,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-55,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-55,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-63,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-76,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-76,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-76,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-66,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-66,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-78,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-79,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
]
[NETWK] Start Parse Wifi Scan Info! (linkSSID:Libawall
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "8",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197426,
    "version": "1.5.6"
}

[25-09-18 20:10:26.704][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#]
[25-09-18 20:10:26.705][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:26.705][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-18 20:10:26.705][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:26.706][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 4c 49 53  .../>|33|WIFILIS

[25-09-18 20:10:26.706][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 30 7e 31 7e 75 6e 6b  TV2|OK~0|0~1~unk

[25-09-18 20:10:26.706][my_btgatt_][att_debug_cb  213][thread1737][D] att:   6e 6f 77 6e 7e 30 7e 30 7c 23                    nown~0~0|#

[25-09-18 20:10:26.706][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#]
[25-09-18 20:10:26.706][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#][size:0 ver:2]
[25-09-18 20:10:26.916][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-44~1~ChinaNet-TNT~0~0|#]
[25-09-18 20:10:26.917][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:26.917][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-18 20:10:26.917][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:26.918][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 34 30 7c 57 49 46 49 4c 49 53  .../>|40|WIFILIS

[25-09-18 20:10:26.918][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 34 7e 31 7e 43  TV2|OK~0|-44~1~C

[25-09-18 20:10:26.918][my_btgatt_][att_debug_cb  213][thread1737][D] att:   68 69 6e 61 4e 65 74 2d 54 4e 54 7e 30 7e 30 7c  hinaNet-TNT~0~0|

[25-09-18 20:10:26.918][my_btgatt_][att_debug_cb  213][thread1737][D] att:   23                                               #

[25-09-18 20:10:26.918][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|40|WIFILISTV2|OK~0|-44~1~ChinaNet-TNT~0~0|#]
[25-09-18 20:10:26.918][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-44~1~ChinaNet-TNT~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-44~1~ChinaNet-TNT~0~0|#][size:0 ver:2]
[25-09-18 20:10:27.129][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-45~1~zzwifi~0~0|#]
[25-09-18 20:10:27.129][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:27.130][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-18 20:10:27.130][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:27.131][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 34 7c 57 49 46 49 4c 49 53  .../>|34|WIFILIS

[25-09-18 20:10:27.131][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 35 7e 31 7e 7a  TV2|OK~0|-45~1~z

[25-09-18 20:10:27.131][my_btgatt_][att_debug_cb  213][thread1737][D] att:   7a 77 69 66 69 7e 30 7e 30 7c 23                 zwifi~0~0|#

[25-09-18 20:10:27.131][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|34|WIFILISTV2|OK~0|-45~1~zzwifi~0~0|#]
[25-09-18 20:10:27.132][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-45~1~zzwifi~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-45~1~zzwifi~0~0|#][size:0 ver:2]
[25-09-18 20:10:27.342][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-48~1~Libawall-5G~0~0|#]
[25-09-18 20:10:27.343][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:27.343][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [45]
[25-09-18 20:10:27.343][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:27.344][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 39 7c 57 49 46 49 4c 49 53  .../>|39|WIFILIS

[25-09-18 20:10:27.344][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 38 7e 31 7e 4c  TV2|OK~0|-48~1~L

[25-09-18 20:10:27.344][my_btgatt_][att_debug_cb  213][thread1737][D] att:   69 62 61 77 61 6c 6c 2d 35 47 7e 30 7e 30 7c 23  ibawall-5G~0~0|#

[25-09-18 20:10:27.344][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|39|WIFILISTV2|OK~0|-48~1~Libawall-5G~0~0|#]
[25-09-18 20:10:27.344][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-48~1~Libawall-5G~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|39|WIFILISTV2|OK~0|-48~1~Libawall-5G~0~0|#][size:0 ver:2]
[25-09-18 20:10:27.374][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 7

[25-09-18 20:10:27.374][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x08

[25-09-18 20:10:27.374][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:read_by_type_cb() Read By Type
- start: 0x0001 end: 0x0006

[25-09-18 20:10:27.374][my_btgatt_][gap_device_na 233][thread1737][D] GAP Device Name Read called, 2798744752

[25-09-18 20:10:27.374][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x09

[25-09-18 20:10:27.374][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 09 13 03 00 59 44 41 53 32 35 30 37 30 31 30 30  ....YDAS25070100

[25-09-18 20:10:27.375][my_btgatt_][att_debug_cb  213][thread1737][D] att:   30 30 30 35 00                                   0005.

[25-09-18 20:10:27.555][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#]
[25-09-18 20:10:27.557][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:27.557][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-18 20:10:27.557][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:27.558][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 4c 49 53  .../>|33|WIFILIS

[25-09-18 20:10:27.558][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 38 7e 31 7e 67  TV2|OK~0|-48~1~g

[25-09-18 20:10:27.558][my_btgatt_][att_debug_cb  213][thread1737][D] att:   75 65 73 74 7e 30 7e 30 7c 23                    uest~0~0|#

[25-09-18 20:10:27.561][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [5], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#]
[25-09-18 20:10:27.561][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#][size:0 ver:2]
[25-09-18 20:10:27.755][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-48~1~device~0~0|#]
[25-09-18 20:10:27.755][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:27.755][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-18 20:10:27.756][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:27.756][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 34 7c 57 49 46 49 4c 49 53  .../>|34|WIFILIS

[25-09-18 20:10:27.756][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 38 7e 31 7e 64  TV2|OK~0|-48~1~d

[25-09-18 20:10:27.756][my_btgatt_][att_debug_cb  213][thread1737][D] att:   65 76 69 63 65 7e 30 7e 30 7c 23                 evice~0~0|#

[25-09-18 20:10:27.756][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|34|WIFILISTV2|OK~0|-48~1~device~0~0|#]
[25-09-18 20:10:27.756][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-48~1~device~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-48~1~device~0~0|#][size:0 ver:2]
[25-09-18 20:10:27.843][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:27.843][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:27.954][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-51~1~TP-LINK_F41B~0~0|#]
[25-09-18 20:10:27.954][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:27.955][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-18 20:10:27.955][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:27.956][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 34 30 7c 57 49 46 49 4c 49 53  .../>|40|WIFILIS

[25-09-18 20:10:27.956][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 35 31 7e 31 7e 54  TV2|OK~0|-51~1~T

[25-09-18 20:10:27.956][my_btgatt_][att_debug_cb  213][thread1737][D] att:   50 2d 4c 49 4e 4b 5f 46 34 31 42 7e 30 7e 30 7c  P-LINK_F41B~0~0|

[25-09-18 20:10:27.956][my_btgatt_][att_debug_cb  213][thread1737][D] att:   23                                               #

[25-09-18 20:10:27.957][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|40|WIFILISTV2|OK~0|-51~1~TP-LINK_F41B~0~0|#]
[25-09-18 20:10:27.957][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-51~1~TP-LINK_F41B~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-51~1~TP-LINK_F41B~0~0|#][size:0 ver:2]
[25-09-18 20:10:28.154][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-54~1~Libawall~1~1|#]
[25-09-18 20:10:28.155][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:28.155][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-18 20:10:28.156][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:28.156][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 36 7c 57 49 46 49 4c 49 53  .../>|36|WIFILIS

[25-09-18 20:10:28.156][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 35 34 7e 31 7e 4c  TV2|OK~0|-54~1~L

[25-09-18 20:10:28.156][my_btgatt_][att_debug_cb  213][thread1737][D] att:   69 62 61 77 61 6c 6c 7e 31 7e 31 7c 23           ibawall~1~1|#

[25-09-18 20:10:28.157][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|36|WIFILISTV2|OK~0|-54~1~Libawall~1~1|#]
[25-09-18 20:10:28.157][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-54~1~Libawall~1~1|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-54~1~Libawall~1~1|#][size:0 ver:2]
[25-09-18 20:10:28.355][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#]
[25-09-18 20:10:28.355][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:28.355][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-18 20:10:28.356][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:28.356][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 4c 49 53  .../>|33|WIFILIS

[25-09-18 20:10:28.357][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 35 36 7e 31 7e 63  TV2|OK~0|-56~1~c

[25-09-18 20:10:28.357][my_btgatt_][att_debug_cb  213][thread1737][D] att:   65 73 68 69 7e 30 7e 30 7c 23                    eshi~0~0|#
[25-09-18 20:10:28.357][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#]
[25-09-18 20:10:28.357][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#][size:0 ver:2]

[25-09-18 20:10:28.555][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-65~1~ceshi_5G~0~0|#]
[25-09-18 20:10:28.557][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:28.557][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-18 20:10:28.557][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:28.558][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 36 7c 57 49 46 49 4c 49 53  .../>|36|WIFILIS

[25-09-18 20:10:28.558][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 36 35 7e 31 7e 63  TV2|OK~0|-65~1~c

[25-09-18 20:10:28.558][my_btgatt_][att_debug_cb  213][thread1737][D] att:   65 73 68 69 5f 35 47 7e 30 7e 30 7c 23           eshi_5G~0~0|#

[25-09-18 20:10:28.560][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [4], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|36|WIFILISTV2|OK~0|-65~1~ceshi_5G~0~0|#]
[25-09-18 20:10:28.560][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-65~1~ceshi_5G~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-65~1~ceshi_5G~0~0|#][size:0 ver:2]
[25-09-18 20:10:29.843][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:29.844][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:30.196][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 36

[25-09-18 20:10:30.196][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x12

[25-09-18 20:10:30.196][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|33|WIFISETTINGV2|2~Libawall~|#1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share
[rkbt_write_cb] offset: 0, len: 33
Data: 2F 3E 7C 33 33 7C 57 49 46 49 53 45 54 54 49 4E 47 56 32 7C 32 7E 4C 69 62 61 77 61 6C 6C 7E 7C 23
[25-09-18 20:10:30.196][bt_ble_rpc][operator()    239][thread1737][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|33|WIFISETTINGV2|2~Libawall~|#1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share], len: [33]
[25-09-18 20:10:30.197][rk_bt_c_ad][operator()    252][thread1732][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|33|WIFISETTINGV2|2~Libawall~|#, len: 33
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|33|WIFISETTINGV2|2~Libawall~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:2|0|Libawall||1
[25-09-18 20:10:30.200][bt_ble_rpc][bluez_call_re 395][thread1737][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [4]
[25-09-18 20:10:30.201][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-18 20:10:30.201][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x13

[25-09-18 20:10:30.202][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 13                                               .

[25-09-18 20:10:30.238][rk_wifi_c_][Mock_RK_WifiD 154][thread1707][I] [===>] RK_WifiDisconnectNetwork1
[NETWK] CB WIFI STATUS : 5
[NETWK] Update Network Status (2(NSTA_WIFI) value:0)
[NETWK] Init Wait WIFI/4G AutoConnect State=[1] !(0: NULL 1:Wlan 2:4G)!
=== WPA Network List (count: 0) ===
[25-09-18 20:10:30.278][rk_wifi_c_][print_saved_i 124][thread1707][I] Total saved networks: 0
[25-09-18 20:10:30.278][rk_wifi_c_][Mock_RK_WifiG 142][thread1707][I] [exit] RK_WifiGetSavedInfo: [0]
[25-09-18 20:10:30.278][rk_wifi_c_][Mock_RK_WifiD 156][thread1707][I] [exit] RK_WifiDisconnectNetwork1
"[NET] Disconnect Wifi Info: id: 0 ssid[Libawall =?= Libawall]"
[25-09-18 20:10:30.404][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFISETTINGV2|OK~0|5~disconnect|#]
[25-09-18 20:10:30.405][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:30.405][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-18 20:10:30.405][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:30.406][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 53 45 54  .../>|33|WIFISET

[25-09-18 20:10:30.406][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 49 4e 47 56 32 7c 4f 4b 7e 30 7c 35 7e 64 69  TINGV2|OK~0|5~di

[25-09-18 20:10:30.406][my_btgatt_][att_debug_cb  213][thread1737][D] att:   73 63 6f 6e 6e 65 63 74 7c 23                    sconnect|#

[25-09-18 20:10:30.406][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFISETTINGV2|OK~0|5~disconnect|#]
[25-09-18 20:10:30.406][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFISETTINGV2|OK~0|5~disconnect|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFISETTINGV2|OK~0|5~disconnect|#][size:0 ver:2]
[25-09-18 20:10:30.465][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 23

[25-09-18 20:10:30.465][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x12

[25-09-18 20:10:30.465][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|20|WIFILISTV2|~|#2~Libawall~|#1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share
[rkbt_write_cb] offset: 0, len: 20
Data: 2F 3E 7C 32 30 7C 57 49 46 49 4C 49 53 54 56 32 7C 7E 7C 23
[25-09-18 20:10:30.465][bt_ble_rpc][operator()    239][thread1737][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|20|WIFILISTV2|~|#2~Libawall~|#1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share], len: [20]
[25-09-18 20:10:30.466][rk_bt_c_ad][operator()    252][thread1732][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|20|WIFILISTV2|~|#, len: 20
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|20|WIFILISTV2|~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:2(CTL_NETWORK_SCAN_APS) args:1
m_isScan= false
[25-09-18 20:10:30.467][bt_ble_rpc][bluez_call_re 395][thread1737][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [2]
[25-09-18 20:10:30.468][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-18 20:10:30.468][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x13

[25-09-18 20:10:30.469][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 13                                               .

[25-09-18 20:10:30.480][rk_wifi_c_][Mock_RK_WifiS  41][thread1707][I] RK_WifiScan, ret: [true]
Run RK_WifiScan :  true
[NETWK] CB WIFI STATUS : 8
Scan complete in 508 ms, found networks:
14:d8:64:8d:f4:1d       5180    -44     [WPA2-PSK-CCMP][ESS]    TP-LINK_5G_F41B
e2:c3:22:6a:b0:8f       5745    -54     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
cc:d8:43:09:96:52       5785    -65     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ceshi_5G
08:9b:4b:42:a9:62       5240    -48     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:a9:62       5240    -48     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:62       5240    -48     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:ab:27       5200    -51     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:ab:27       5200    -51     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:ab:27       5200    -52     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:b3:d3:79       5180    -59     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:b3:d3:79       5180    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:b3:d3:79       5180    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:bb:b1:54       5240    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:a9:56       5220    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:a9:56       5220    -63     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:54       5240    -63     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b1:54       5240    -63     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:56       5220    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
b8:08:d7:18:81:bc       2437    -45     [WPA2-PSK-CCMP][ESS]    zzwifi
e2:c3:22:6a:b0:8b       2412    -46     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
12:9b:4b:42:a9:55       2437    -59     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:a9:55       2437    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:ab:18       5220    -68     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:b3:d3:78       2437    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:a9:55       2437    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
08:9b:4b:42:ab:18       5220    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:bb:b1:70       5220    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:18       5220    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
14:d8:64:8d:f4:1b       2412    -52     [WPA2-PSK-CCMP][ESS]    TP-LINK_F41B
cc:d8:43:09:96:51       2422    -56     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ceshi
0e:9b:4b:b3:d3:78       2437    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:53       2462    -55     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
0e:9b:4b:bb:b1:70       5220    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:bb:b1:70       5220    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:bb:b1:68       5200    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:b4:d8       5200    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:bb:b1:68       5200    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b1:68       5200    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:42:a9:61       2412    -53     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
0e:9b:4b:42:b4:d8       5200    -73     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:b4:d8       5200    -73     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:a9:61       2412    -49     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:61       2412    -52     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:bb:b1:6f       2462    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:b3:d3:7f       5805    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:b3:d3:7f       5805    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:b3:d3:7f       5805    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:42:ab:11       2437    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:ab:12       5745    -76     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:ab:12       5745    -76     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:ab:12       5745    -76     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:42:b4:d7       2437    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b1:67       2462    -74     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
0e:9b:4b:bb:b1:6f       2462    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:42:b4:de       5180    -78     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:42:b4:d7       2437    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:ab:0c       5745    -79     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
[25-09-18 20:10:31.195][rk_wifi_c_][Mock_RK_WifiS  50][thread1712][I] get scan result success
[25-09-18 20:10:31.206][rk_wifi_c_][Mock_RK_WifiS  60][thread1712][I] RK_WifiScanResults, result: [{"ssid":"TP-LINK_5G_F41B","rssi":-44,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-54,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"ceshi_5G","rssi":-65,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-48,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-48,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-48,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-51,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-51,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-52,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-59,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-63,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-63,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-63,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"zzwifi","rssi":-45,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-46,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-59,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-68,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"TP-LINK_F41B","rssi":-52,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ceshi","rssi":-56,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-55,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-53,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-73,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-73,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-49,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-52,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-76,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-76,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-76,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-74,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-78,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-79,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
]
[NETWK] Start Parse Wifi Scan Info! (linkSSID:
[25-09-18 20:10:31.212][wifi_mng_w][operator()   1073][thread1712][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-18 20:10:31.212][wifi_mng_w][operator()   1075][thread1712][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[25-09-18 20:10:31.354][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#]
[25-09-18 20:10:31.354][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:31.355][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-18 20:10:31.355][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:31.356][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 4c 49 53  .../>|33|WIFILIS

[25-09-18 20:10:31.356][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 30 7e 31 7e 75 6e 6b  TV2|OK~0|0~1~unk

[25-09-18 20:10:31.356][my_btgatt_][att_debug_cb  213][thread1737][D] att:   6e 6f 77 6e 7e 30 7e 30 7c 23                    nown~0~0|#

[25-09-18 20:10:31.357][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#]
[25-09-18 20:10:31.358][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#][size:0 ver:2]
[25-09-18 20:10:31.555][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-45~1~zzwifi~0~0|#]
[25-09-18 20:10:31.555][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:31.555][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-18 20:10:31.555][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:31.557][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 34 7c 57 49 46 49 4c 49 53  .../>|34|WIFILIS

[25-09-18 20:10:31.557][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 35 7e 31 7e 7a  TV2|OK~0|-45~1~z

[25-09-18 20:10:31.557][my_btgatt_][att_debug_cb  213][thread1737][D] att:   7a 77 69 66 69 7e 30 7e 30 7c 23                 zwifi~0~0|#

[25-09-18 20:10:31.560][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [5], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|34|WIFILISTV2|OK~0|-45~1~zzwifi~0~0|#]
[25-09-18 20:10:31.560][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-45~1~zzwifi~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-45~1~zzwifi~0~0|#][size:0 ver:2]
[25-09-18 20:10:31.754][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-46~1~ChinaNet-TNT~0~0|#]
[25-09-18 20:10:31.755][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:31.755][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-18 20:10:31.755][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:31.756][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 34 30 7c 57 49 46 49 4c 49 53  .../>|40|WIFILIS

[25-09-18 20:10:31.756][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 36 7e 31 7e 43  TV2|OK~0|-46~1~C

[25-09-18 20:10:31.756][my_btgatt_][att_debug_cb  213][thread1737][D] att:   68 69 6e 61 4e 65 74 2d 54 4e 54 7e 30 7e 30 7c  hinaNet-TNT~0~0|

[25-09-18 20:10:31.756][my_btgatt_][att_debug_cb  213][thread1737][D] att:   23                                               #

[25-09-18 20:10:31.756][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|40|WIFILISTV2|OK~0|-46~1~ChinaNet-TNT~0~0|#]
[25-09-18 20:10:31.756][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-46~1~ChinaNet-TNT~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-46~1~ChinaNet-TNT~0~0|#][size:0 ver:2]
[25-09-18 20:10:31.843][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:31.844][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:31.955][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-48~1~Libawall-5G~0~0|#]
[25-09-18 20:10:31.955][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:31.956][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [45]
[25-09-18 20:10:31.956][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:31.957][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 39 7c 57 49 46 49 4c 49 53  .../>|39|WIFILIS

[25-09-18 20:10:31.957][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 38 7e 31 7e 4c  TV2|OK~0|-48~1~L

[25-09-18 20:10:31.957][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|39|WIFILISTV2|OK~0|-48~1~Libawall-5G~0~0|#]
[25-09-18 20:10:31.957][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-48~1~Libawall-5G~0~0|#], ret: [0]
[25-09-18 20:10:31.958][my_btgatt_][att_debug_cb  213][thread1737][D] att:   69 62 61 77 61 6c 6c 2d 35 47 7e 30 7e 30 7c 23  ibawall-5G~0~0|#

[NETWK] BT Write Data(ret=0): [/>|39|WIFILISTV2|OK~0|-48~1~Libawall-5G~0~0|#][size:0 ver:2]
[25-09-18 20:10:32.154][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#]
[25-09-18 20:10:32.155][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:32.155][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-18 20:10:32.155][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:32.156][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 4c 49 53  .../>|33|WIFILIS

[25-09-18 20:10:32.156][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 38 7e 31 7e 67  TV2|OK~0|-48~1~g

[25-09-18 20:10:32.156][my_btgatt_][att_debug_cb  213][thread1737][D] att:   75 65 73 74 7e 30 7e 30 7c 23                    uest~0~0|#

[25-09-18 20:10:32.156][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#]
[25-09-18 20:10:32.156][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#][size:0 ver:2]
[25-09-18 20:10:32.219][wifi_mng_w][operator()   1073][thread1712][I] [on loop] start_polling_status_rk, enter callback, rk_state: [OPEN]
[NETWK] CB WIFI STATUS : 6
=== WPA Network List (count: 0) ===
[25-09-18 20:10:32.227][rk_wifi_c_][print_saved_i 124][thread1712][I] Total saved networks: 0
[25-09-18 20:10:32.227][rk_wifi_c_][Mock_RK_WifiG 142][thread1712][I] [exit] RK_WifiGetSavedInfo: [0]
[25-09-18 20:10:32.227][wifi_mng_w][operator()   1075][thread1712][I] [on loop] start_polling_status_rk, break callback, rk_state: [OPEN]
[25-09-18 20:10:32.355][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-48~1~device~0~0|#]
[25-09-18 20:10:32.355][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:32.355][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-18 20:10:32.356][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:32.356][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 34 7c 57 49 46 49 4c 49 53  .../>|34|WIFILIS

[25-09-18 20:10:32.356][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 38 7e 31 7e 64  TV2|OK~0|-48~1~d

[25-09-18 20:10:32.357][my_btgatt_][att_debug_cb  213][thread1737][D] att:   65 76 69 63 65 7e 30 7e 30 7c 23                 evice~0~0|#

[25-09-18 20:10:32.357][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|34|WIFILISTV2|OK~0|-48~1~device~0~0|#]
[25-09-18 20:10:32.357][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-48~1~device~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-48~1~device~0~0|#][size:0 ver:2]
[25-09-18 20:10:32.554][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-52~1~TP-LINK_F41B~0~0|#]
[25-09-18 20:10:32.555][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:32.555][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-18 20:10:32.557][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:32.557][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 34 30 7c 57 49 46 49 4c 49 53  .../>|40|WIFILIS

[25-09-18 20:10:32.557][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 35 32 7e 31 7e 54  TV2|OK~0|-52~1~T

[25-09-18 20:10:32.557][my_btgatt_][att_debug_cb  213][thread1737][D] att:   50 2d 4c 49 4e 4b 5f 46 34 31 42 7e 30 7e 30 7c  P-LINK_F41B~0~0|

[25-09-18 20:10:32.557][my_btgatt_][att_debug_cb  213][thread1737][D] att:   23                                               #

[25-09-18 20:10:32.559][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [4], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|40|WIFILISTV2|OK~0|-52~1~TP-LINK_F41B~0~0|#]
[25-09-18 20:10:32.560][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-52~1~TP-LINK_F41B~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-52~1~TP-LINK_F41B~0~0|#][size:0 ver:2]
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:-256] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:10:32.755][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-53~1~Libawall~0~0|#]
[25-09-18 20:10:32.755][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:32.756][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-18 20:10:32.756][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:32.756][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 36 7c 57 49 46 49 4c 49 53  .../>|36|WIFILIS

[25-09-18 20:10:32.757][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 35 33 7e 31 7e 4c  TV2|OK~0|-53~1~L

[25-09-18 20:10:32.757][my_btgatt_][att_debug_cb  213][thread1737][D] att:   69 62 61 77 61 6c 6c 7e 30 7e 30 7c 23           ibawall~0~0|#

[25-09-18 20:10:32.757][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|36|WIFILISTV2|OK~0|-53~1~Libawall~0~0|#]
[25-09-18 20:10:32.757][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-53~1~Libawall~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-53~1~Libawall~0~0|#][size:0 ver:2]
[25-09-18 20:10:32.955][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#]
[25-09-18 20:10:32.955][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:32.955][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-18 20:10:32.956][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:32.956][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 4c 49 53  .../>|33|WIFILIS

[25-09-18 20:10:32.956][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 35 36 7e 31 7e 63  TV2|OK~0|-56~1~c

[25-09-18 20:10:32.956][my_btgatt_][att_debug_cb  213][thread1737][D] att:   65 73 68 69 7e 30 7e 30 7c 23                    eshi~0~0|#

[25-09-18 20:10:32.957][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#]
[25-09-18 20:10:32.957][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#][size:0 ver:2]
[25-09-18 20:10:33.155][rk_bt_c_ad][Mock_RK_BleWr 288][thread1709][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-65~1~ceshi_5G~0~0|#]
[25-09-18 20:10:33.155][my_btgatt_][ble_server_no 429][thread1724][D] enter ble_server_notify
[25-09-18 20:10:33.156][my_btgatt_][ble_server_no 460][thread1724][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-18 20:10:33.156][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x1b

[25-09-18 20:10:33.157][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 1b 0f 00 2f 3e 7c 33 36 7c 57 49 46 49 4c 49 53  .../>|36|WIFILIS

[25-09-18 20:10:33.158][my_btgatt_][att_debug_cb  213][thread1737][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 36 35 7e 31 7e 63  TV2|OK~0|-65~1~c

[25-09-18 20:10:33.158][my_btgatt_][att_debug_cb  213][thread1737][D] att:   65 73 68 69 5f 35 47 7e 30 7e 30 7c 23           eshi_5G~0~0|#

[25-09-18 20:10:33.158][bt_ble_rpc][notify_sync   186][thread1709][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [3], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|36|WIFILISTV2|OK~0|-65~1~ceshi_5G~0~0|#]
[25-09-18 20:10:33.159][rk_bt_c_ad][Mock_RK_BleWr 291][thread1709][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-65~1~ceshi_5G~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-65~1~ceshi_5G~0~0|#][size:0 ver:2]
[25-09-18 20:10:33.844][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:33.845][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:35.845][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:35.846][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:37.846][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:37.847][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:39.847][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:39.848][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:-256] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "9",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197441,
    "version": "1.5.6"
}

[25-09-18 20:10:41.848][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:41.849][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:43.850][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:43.850][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:44.385][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 46

[25-09-18 20:10:44.385][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x12

[25-09-18 20:10:44.385][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|43|WIFISETTINGV2|1~1~Libawall~11223344|#76~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share
[rkbt_write_cb] offset: 0, len: 43
Data: 2F 3E 7C 34 33 7C 57 49 46 49 53 45 54 54 49 4E 47 56 32 7C 31 7E 31 7E 4C 69 62 61 77 61 6C 6C 7E 31 31 32 32 33 33 34 34 7C 23
[25-09-18 20:10:44.386][bt_ble_rpc][operator()    239][thread1737][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|43|WIFISETTINGV2|1~1~Libawall~11223344|#76~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share], len: [43]
[25-09-18 20:10:44.386][rk_bt_c_ad][operator()    252][thread1732][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|43|WIFISETTINGV2|1~1~Libawall~11223344|#, len: 43
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|43|WIFISETTINGV2|1~1~Libawall~11223344|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:1|1|Libawall|11223344|1
[25-09-18 20:10:44.390][bt_ble_rpc][bluez_call_re 395][thread1737][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [4]
[25-09-18 20:10:44.391][my_btgatt_][gatt_debug_cb 220][thread1737][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-18 20:10:44.391][my_btgatt_][att_debug_cb  213][thread1737][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x13

[25-09-18 20:10:44.391][my_btgatt_][att_debug_cb  213][thread1737][D] att: < 13                                               .

[25-09-18 20:10:44.428][rk_wifi_c_][Mock_RK_WifiC  66][thread1707][I] [===>] RK_WifiConnect
[25-09-18 20:10:44.431][rk_wifi_c_][Mock_RK_WifiC  70][thread1707][I] the matched wpa_network_config_list count: [0], with ssid: [Libawall], pask: [11223344]
[25-09-18 20:10:44.439][rk_wifi_c_][Mock_RK_WifiC  85][thread1707][D] WifiMngWpaImpl::ins().add_network, cur_network_id: [0]
[25-09-18 20:10:44.446][rk_wifi_c_][Mock_RK_WifiC  87][thread1707][D] WifiMngWpaImpl::ins().set_network_ssid: [true]
[25-09-18 20:10:44.563][rk_wifi_c_][Mock_RK_WifiC  89][thread1707][D] WifiMngWpaImpl::ins().set_network_psk: [true]
[25-09-18 20:10:44.592][rk_wifi_c_][Mock_RK_WifiC  91][thread1707][D] WifiMngWpaImpl::ins().save_config: [true]
[25-09-18 20:10:44.646][rk_wifi_c_][Mock_RK_WifiC  93][thread1707][D] WifiMngWpaImpl::ins().enable_network: [true]
[25-09-18 20:10:44.663][rk_wifi_c_][Mock_RK_WifiC  95][thread1707][D] WifiMngWpaImpl::ins().select_network: [true]
[25-09-18 20:10:44.663][rk_wifi_c_][Mock_RK_WifiC  97][thread1707][I] [exit] RK_WifiConnect
[25-09-18 20:10:45.318][wifi_mng_w][operator()   1073][thread1712][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-18 20:10:45.318][wifi_mng_w][operator()   1075][thread1712][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[25-09-18 20:10:45.851][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:45.852][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:47.852][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:47.853][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:48.342][wifi_mng_w][operator()   1073][thread1712][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-18 20:10:48.342][wifi_mng_w][operator()   1075][thread1712][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:-256] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:10:49.349][wifi_mng_w][operator()   1073][thread1712][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-18 20:10:49.349][wifi_mng_w][operator()   1075][thread1712][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[25-09-18 20:10:49.854][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:49.854][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:50.356][wifi_mng_w][operator()   1073][thread1712][I] [on loop] start_polling_status_rk, enter callback, rk_state: [CONNECTED]
[NETWK] CB WIFI STATUS : 4
=== WPA Network List (count: 1) ===
Index: 0
  ID:     0
  SSID:   Libawall
  BSSID:  any
  FLAGS:        [CURRENT]
-----------------------------------
[25-09-18 20:10:50.363][rk_wifi_c_][print_saved_i 124][thread1712][I] Total saved networks: 1
[25-09-18 20:10:50.363][rk_wifi_c_][print_saved_i 134][thread1712][I]   [#0] SSID: 'Libawall', BSSID: 'any', State: '[CURRENT]'
[25-09-18 20:10:50.364][rk_wifi_c_][Mock_RK_WifiG 142][thread1712][I] [exit] RK_WifiGetSavedInfo: [0]
[NETWK] **********************
[NETWK] updateSaveApInfo {id=0 ssid=Libawall bssid=any state:[CURRENT]}
[NETWK] **********************
[NETWK] CONNETED WIFI SSID: (initAutoConnType:1) ssid:Libawall mac:94:ba:06:91:04:0b ip:192.168.49.26
[25-09-18 20:10:50.364][wlancontro][stCallbackRec 182][thread1712][W] set initAutoConnType 98
[NETWK] WIFI SET UDHCP (type:2) metric:98
[glSetUdhcpcOnMetric] IP output: "192.168.49.26"
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 98
[25-09-18 20:10:50.429][wlancontro][stCallbackRec 226][thread1712][W] will reset m_systemStatus->initAutoConnType to 0, current: [1]
[25-09-18 20:10:50.429][wlancontro][stCallbackRec 228][thread1712][W] set m_systemStatus->initAutoConnType to 0
[25-09-18 20:10:51.446][wifi_mng_w][operator()   1075][thread1712][I] [on loop] start_polling_status_rk, break callback, rk_state: [CONNECTED]
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[25-09-18 20:10:51.855][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:51.855][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:53.856][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:53.856][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:55.857][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:55.858][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "10",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197456,
    "version": "1.5.6"
}

[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-57] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:10:57.858][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:57.859][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:10:59.859][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:10:59.860][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:01.861][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:01.861][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:03.862][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:03.863][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-57] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:11:05.863][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:05.864][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:07.864][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:07.865][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:09.866][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:09.866][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "11",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197471,
    "version": "1.5.6"
}

[25-09-18 20:11:11.867][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:11.868][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-57] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:11:13.868][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:13.869][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:15.869][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:15.870][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:17.871][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:17.871][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:19.871][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:19.871][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:11:21.872][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:21.873][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:23.874][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:23.875][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:25.875][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:25.875][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "12",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197486,
    "version": "1.5.6"
}

[25-09-18 20:11:27.876][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:27.877][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:11:29.877][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:29.878][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:31.879][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:31.879][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:33.880][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:33.881][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:35.881][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:35.882][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:11:37.883][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:37.884][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:39.884][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:39.884][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "13",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197501,
    "version": "1.5.6"
}

[25-09-18 20:11:41.885][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:41.885][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:43.886][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:43.887][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4129mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:11:45.887][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:45.888][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:47.888][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:47.889][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:49.889][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:49.890][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:51.891][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:51.891][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:11:53.891][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:53.892][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:55.893][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:55.893][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "14",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197516,
    "version": "1.5.6"
}

[25-09-18 20:11:57.894][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:57.894][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:11:59.895][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:11:59.895][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:12:01.896][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:01.897][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:03.897][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:03.898][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:05.899][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:05.899][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:07.900][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:07.901][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:12:09.901][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:09.902][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "15",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197531,
    "version": "1.5.6"
}

[25-09-18 20:12:11.902][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:11.903][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:13.904][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:13.905][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:15.906][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:15.906][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:12:17.907][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:17.908][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:19.908][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:19.909][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:21.909][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:21.910][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:23.911][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:23.912][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:12:25.912][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:25.912][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "16",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197546,
    "version": "1.5.6"
}

[25-09-18 20:12:27.913][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:27.914][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:29.914][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:29.915][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:31.915][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:31.916][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:12:33.917][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:33.918][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:35.918][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:35.919][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:37.920][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:37.920][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:39.920][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:39.921][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "17",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197561,
    "version": "1.5.6"
}

[25-09-18 20:12:41.921][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:41.922][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:43.922][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:43.923][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[EVENT] ?????? Again Check Keys Error (get_keys: 0x0b  again_keys: 0x0a)
[25-09-18 20:12:45.924][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:45.924][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:47.925][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:47.925][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:12:49.926][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:49.927][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:51.927][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:51.928][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:53.929][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:53.929][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:55.930][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:55.931][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "18",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197576,
    "version": "1.5.6"
}

[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:12:57.931][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:57.932][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:12:59.933][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:12:59.934][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:01.934][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:01.934][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:03.935][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:03.935][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:13:05.936][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:05.937][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:07.938][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:07.938][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:09.939][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:09.940][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "19",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197591,
    "version": "1.5.6"
}

[25-09-18 20:13:11.940][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:11.941][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:13:13.941][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:13.942][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:15.943][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:15.943][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:17.943][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:17.944][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:19.945][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:19.946][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:13:21.946][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:21.947][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:23.947][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:23.947][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:25.948][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:25.949][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "20",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197606,
    "version": "1.5.6"
}

[25-09-18 20:13:27.950][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:27.950][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4129mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:13:29.951][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:29.952][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:31.952][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:31.953][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:33.953][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:33.954][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:35.955][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:35.956][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4129mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:13:37.957][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:37.957][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:39.958][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:39.959][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "21",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197621,
    "version": "1.5.6"
}

[25-09-18 20:13:41.959][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:41.960][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:43.960][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:43.961][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:13:45.961][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:45.962][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:47.963][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:47.963][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:49.964][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:49.965][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:51.965][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:51.966][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-54] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:13:53.966][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:53.967][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:55.968][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:55.969][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "22",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197636,
    "version": "1.5.6"
}

[25-09-18 20:13:57.970][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:57.971][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:13:59.971][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:13:59.972][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-54] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:14:01.971][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:01.972][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:03.973][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:03.973][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:05.973][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:05.974][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:07.975][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:07.975][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-54] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4129mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:14:09.976][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:09.977][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "23",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197651,
    "version": "1.5.6"
}

[25-09-18 20:14:11.978][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:11.979][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:13.980][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:13.981][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:15.981][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:15.982][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-54] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:14:17.982][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:17.983][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:19.984][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:19.984][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:21.985][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:21.985][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:23.986][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:23.987][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:14:25.987][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:25.987][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "24",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197666,
    "version": "1.5.6"
}

[25-09-18 20:14:27.988][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:27.989][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:29.990][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:29.990][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:31.990][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:31.991][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:14:33.991][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:33.992][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:35.993][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:35.993][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:37.994][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:37.995][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:39.995][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:39.996][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "25",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197681,
    "version": "1.5.6"
}

[25-09-18 20:14:41.997][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:41.997][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:43.998][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:43.999][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:46.000][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:46.001][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:48.002][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:48.002][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:14:50.003][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:50.004][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:52.003][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:52.004][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:54.005][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:54.006][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:14:56.006][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:56.006][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "26",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197696,
    "version": "1.5.6"
}

[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-55] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:14:58.007][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:14:58.008][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:00.008][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:00.009][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:02.009][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:02.010][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:04.011][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:04.011][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:15:06.012][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:06.013][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:08.013][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:08.014][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:10.015][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:10.015][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "27",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197711,
    "version": "1.5.6"
}

[25-09-18 20:15:12.016][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:12.017][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:15:14.017][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:14.017][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:16.018][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:16.019][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:18.019][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:18.020][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:20.020][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:20.021][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:15:22.021][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:22.022][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:24.023][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:24.023][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:26.024][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:26.025][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "28",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197726,
    "version": "1.5.6"
}

[25-09-18 20:15:28.026][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:28.026][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-57] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:15:30.027][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:30.028][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:32.028][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:32.029][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:34.030][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:34.030][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:36.030][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:36.031][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-57] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:15:38.031][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:38.032][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:40.033][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:40.034][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "29",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197741,
    "version": "1.5.6"
}

[25-09-18 20:15:42.034][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:42.035][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:44.036][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:44.036][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:15:46.037][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:46.037][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[EVENT] Device Lcd Screen Auto Off !!!!!!!!!!!!! [181min]
[25-09-18 20:15:48.038][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:48.039][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:50.039][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:50.040][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:52.041][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:52.041][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:15:54.042][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:54.043][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:15:56.043][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:56.044][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "30",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197756,
    "version": "1.5.6"
}

[25-09-18 20:15:58.045][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:15:58.045][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:16:00.046][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:00.046][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:16:02.047][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:02.048][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:16:04.048][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:04.049][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:16:06.049][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:06.050][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[EVENT] ?????? Again Check Keys Error (get_keys: 0x08  again_keys: 0x0a)
[25-09-18 20:16:08.051][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:08.052][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:16:10.052][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:10.053][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "31",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 98,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758197771,
    "version": "1.5.6"
}

[25-09-18 20:16:12.054][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:12.054][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:16:14.055][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:14.055][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:16:16.056][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:16.057][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:16:18.057][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:18.057][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:16:20.058][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:20.059][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:16:22.059][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:22.060][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-18 20:16:24.060][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:24.061][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-56] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:98 capcity:4128mV curent:-14nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-18 20:16:26.061][bt_ble_rpc][operator()     92][thread1724][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-18 20:16:26.062][bt_ble_rpc][operator()    223][thread1732][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "32",
    "params": {
        "baseStation": {
            "accessType": 0

```
