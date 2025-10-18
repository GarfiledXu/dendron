---
id: 18kvdky84g1wd3beeho8r6i
title: '6'
desc: ''
updated: 1758249623986
created: 1758249590860
---

## 排查网络autotype设置1问题

```bash
[25-09-19 09:35:08.626][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:08.626][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:-256] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:
0 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:35:10.627][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:10.628][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:12.628][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:12.629][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:14.629][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:14.630][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:16.631][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:16.631][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:-256] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:
0 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:35:18.632][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:18.632][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:20.633][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:20.634][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:22.635][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:22.635][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:24.636][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:24.637][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] ****************Client Connection Lost {cause: ()}****************
[MQTT] Failed to publish message, return code -1

+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:-256] isDeviceLock:0 mqttStatus:1 isCharge:1 energy:100 capcity:4124mV curent:-38nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:
0 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
[25-09-19 09:35:26.637][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:26.638][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:26.861][my_btgatt_][att_debug_cb  213][thread1248][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT receive
d: 46

[25-09-19 09:35:26.862][my_btgatt_][att_debug_cb  213][thread1248][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeeac8) ATT PDU rec
eived: 0x12

[25-09-19 09:35:26.862][my_btgatt_][gatt_debug_cb 220][thread1248][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|43|WIFISETTINGV2|1~1~Libawall~11223344|#76~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share
[rkbt_write_cb] offset: 0, len: 43
Data: 2F 3E 7C 34 33 7C 57 49 46 49 53 45 54 54 49 4E 47 56 32 7C 31 7E 31 7E 4C 69 62 61 77 61 6C 6C 7E 31 31 32 32 33 33 34 34 7C 23
[25-09-19 09:35:26.862][bt_ble_rpc][operator()    239][thread1248][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|43|WIFISETTINGV2|1~1~Libawall~11223344|#76~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share], len: [43]
[25-09-19 09:35:26.862][rk_bt_c_ad][operator()    252][thread1243][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|43|WIFISETTINGV2|1~1~Libawall~11223344|#, len: 43
[COMM] Handle One Message( m_msgId=2 [type:1] topic:ble datas:/>|43|WIFISETTINGV2|1~1~Libawall~11223344|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:1|1|Libawall|11223344|2
[25-09-19 09:35:26.867][bt_ble_rpc][bluez_call_re 395][thread1248][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [4]
[25-09-19 09:35:26.867][my_btgatt_][gatt_debug_cb 220][thread1248][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-19 09:35:26.867][my_btgatt_][att_debug_cb  213][thread1248][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeeac8) ATT op
0x13

[25-09-19 09:35:26.867][my_btgatt_][att_debug_cb  213][thread1248][D] att: < 13                                               .

[25-09-19 09:35:26.904][rk_wifi_c_][Mock_RK_WifiC  66][thread1218][I] [===>] RK_WifiConnect
[25-09-19 09:35:26.905][rk_wifi_c_][Mock_RK_WifiC  70][thread1218][I] the matched wpa_network_config_list count: [0], with ssid: [Libawall], pask: [11223344]
[25-09-19 09:35:26.912][rk_wifi_c_][Mock_RK_WifiC  85][thread1218][D] WifiMngWpaImpl::ins().add_network, cur_network_id: [0]
[25-09-19 09:35:26.919][rk_wifi_c_][Mock_RK_WifiC  87][thread1218][D] WifiMngWpaImpl::ins().set_network_ssid: [true]
+++++m_lostDTime+++++wait 6 seconds
[25-09-19 09:35:27.113][rk_wifi_c_][Mock_RK_WifiC  89][thread1218][D] WifiMngWpaImpl::ins().set_network_psk: [true]
[25-09-19 09:35:27.124][rk_wifi_c_][Mock_RK_WifiC  91][thread1218][D] WifiMngWpaImpl::ins().save_config: [true]
[25-09-19 09:35:27.135][rk_wifi_c_][Mock_RK_WifiC  93][thread1218][D] WifiMngWpaImpl::ins().enable_network: [true]
[25-09-19 09:35:27.148][rk_wifi_c_][Mock_RK_WifiC  95][thread1218][D] WifiMngWpaImpl::ins().select_network: [true]
[25-09-19 09:35:27.148][rk_wifi_c_][Mock_RK_WifiC  97][thread1218][I] [exit] RK_WifiConnect
+++++m_lostDTime+++++wait 6 seconds
[25-09-19 09:35:27.330][wifi_mng_w][operator()   1073][thread1223][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-19 09:35:27.330][wifi_mng_w][operator()   1075][thread1223][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
[25-09-19 09:35:28.639][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:28.639][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
[25-09-19 09:35:30.641][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:30.641][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
[25-09-19 09:35:31.369][wifi_mng_w][operator()   1073][thread1223][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-19 09:35:31.369][wifi_mng_w][operator()   1075][thread1223][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
+++++m_lostDTime+++++wait 6 seconds
+++++m_lostDTime+++++wait 6 seconds
[MQTT] Start Connect Server [network=[2]WIFI] [env:3] [ip_port:115.238.152.230:1887] [clientId:ba3dcc5d-a807-48ad-8379-960a0d814fa1|time=1756625455521] [mqttuser:YDAS250701000005&yda] [mqttpwd:0VyxiXaa61MSEW0emXq/Ww==]!
[25-09-19 09:35:32.642][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:32.642][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:-256] isDeviceLock:0 mqttStatus:1 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:
0 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:35:34.643][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:34.644][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] HAS CONNECT OK!!!
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/event/fingerprint_login/post_reply]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/fingerprint_entering/invoke]
[25-09-19 09:35:35.398][wifi_mng_w][operator()   1073][thread1223][I] [on loop] start_polling_status_rk, enter callback, rk_state: [CONNECTED]
[NETWK] CB WIFI STATUS : 4
=== WPA Network List (count: 1) ===
Index: 0
  ID:     0
  SSID:   Libawall
  BSSID:  any
  FLAGS:        [CURRENT]
-----------------------------------
[25-09-19 09:35:35.406][rk_wifi_c_][print_saved_i 124][thread1223][I] Total saved networks: 1
[25-09-19 09:35:35.406][rk_wifi_c_][print_saved_i 134][thread1223][I]   [#0] SSID: 'Libawall', BSSID: 'any', State: '[CURRENT]'
[25-09-19 09:35:35.406][rk_wifi_c_][Mock_RK_WifiG 142][thread1223][I] [exit] RK_WifiGetSavedInfo: [0]
[NETWK] **********************
[NETWK] updateSaveApInfo {id=0 ssid=Libawall bssid=any state:[CURRENT]}
[NETWK] **********************
[NETWK] CONNETED WIFI SSID: (initAutoConnType:1) ssid:Libawall mac:94:ba:06:91:04:0b ip:192.168.49.26
[25-09-19 09:35:35.406][wlancontro][stCallbackRec 182][thread1223][W] set initAutoConnType 98
[NETWK] WIFI SET UDHCP (type:2) metric:98
[glSetUdhcpcOnMetric] IP output: "192.168.49.26"
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/change_seal/invoke]
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 98
[25-09-19 09:35:35.474][wlancontro][stCallbackRec 226][thread1223][W] will reset m_systemStatus->initAutoConnType to 0, current: [1]
[25-09-19 09:35:35.474][wlancontro][stCallbackRec 228][thread1223][W] set m_systemStatus->initAutoConnType to 0
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/change_seal_finish/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/begin_seal/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/user_login/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/user_logout/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/equipment_lock/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/examine_result/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/cancel_operation/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/event/document_state_uploading_event/post_reply]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/ota/firmware/upgrade]
[25-09-19 09:35:36.492][wifi_mng_w][operator()   1075][thread1223][I] [on loop] start_polling_status_rk, break callback, rk_state: [CONNECTED]
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/ota/firmware/request_reply]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/begin_ink/invoke]
[25-09-19 09:35:36.644][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:36.645][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/begin_ink_result/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/with_ink/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/fingerprint_import/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/event/fingerprint_import/post_reply]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/stop_privilege_seal/invoke]
[MQTT] Success to scribeTopic [/sys/yda/YDAS250701000005/service/voice/invoke]
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
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/fingerprint_import/post) Msg:{
    "id": "30",
    "params": {
        "init": true
    }
}

[U**I] onHandleMainUICommand :  57
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/ota/firmware/request) Msg:{
    "id": "31",
    "params": {
    },
    "time": 1758245738,
    "version": "1.5.6"
}

[MQTT] Message arrived [recvcnt:1] [mainqueues_size:0] [len:361] topicName: [<X॰�7]
[MQTT] Message arrived [recvcnt:2] [mainqueues_size:0] [len:97] topicName: []
[COMM] Handle One Message( m_msgId=093537883 [type:2] topic:/sys/yda/YDAS250701000005/service/reset/invoke datas:{"id":"093537883","time":1758245737883,"params":{"voice":true,"wifiPriority":1,"sealId":7076,"sealPattern":1,"u
serMobileNetwork":1,"reconnection":false,"sealType":2,"sealName":"徐佳飞-smart-奥特之章重生","useWifi":1,"useBluetooth":1,"gyroParameter":1,"needConvertVideo":true,"remoteWake":-1,"reset":false,"sleepTime":3,"offTime":-1,"re
moteLock":false}} )
[COMM] Sync Date Time(2025-09-19 09:35:38) to Local! Get Local(2025-09-19 09:35:38)
[25-09-19 09:35:38.018][commandcon][syncLocalDate 118][thread1162][W] Server epoch: [1758245737883]
[25-09-19 09:35:38.019][commandcon][syncLocalDate 119][thread1162][W] UTC time: [2025-09-19 01:35:37]
[25-09-19 09:35:38.019][commandcon][syncLocalDate 120][thread1162][W] Local time: [2025-09-19 09:35:38]
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/service/reset/invoke_reply) Msg:{
    "code": 0,
    "data": {
        "status": 1
    },
    "id": "93537883",
    "success": true,
    "time": 1758245738
}

[EVENT] Module Recv Commands (send: CommandController cmd:21))
[25-09-19 09:35:38.023][commandcon][handleSetDevi 945][thread1162][W] emit MainSignalManager::instance()->handleNetworkModuleCommand(name(), MDSCOMMAND_REQUEST_SET_WIFI_ENABLE
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:5(CTL_NETWORK_SET_WIFI_ENABLE) args:1
[25-09-19 09:35:38.024][wlancontro][onHandleWlanC  92][thread1218][W] set initAutoConnType from: [0], to: [1]
[NETWK] Already Init BT!
[NETWK] Current Device Use New BLE!!!
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

[25-09-19 09:35:38.065][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:38.066][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "32",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245738,
    "version": "1.5.6"
}

[COMM] Handle One Message( m_msgId=30 [type:2] topic:/sys/yda/YDAS250701000005/event/fingerprint_import/post_reply datas:{"code":0,"data":{"fingerprint":[],"privilege":[]},"success":true,"id":"30","time":1758245737948} )
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "33",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245738,
    "version": "1.5.6"
}

[25-09-19 09:35:40.066][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:40.067][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-60] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4126mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:35:42.066][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:42.067][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:44.067][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:44.068][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:46.069][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:46.069][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:48.070][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:48.071][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-59] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:35:50.072][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:50.072][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:52.073][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:52.074][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "34",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245753,
    "version": "1.5.6"
}

[25-09-19 09:35:54.074][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:54.075][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:35:56.075][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:56.076][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-60] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:35:58.076][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:35:58.077][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:00.077][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:00.078][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:02.079][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:02.080][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:04.080][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:04.081][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-60] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:36:06.082][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:06.083][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:08.083][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:08.084][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "35",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245768,
    "version": "1.5.6"
}

[25-09-19 09:36:10.084][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:10.085][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:12.086][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:12.086][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-59] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:36:14.086][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:14.087][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:16.087][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:16.088][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:18.089][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:18.089][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:20.090][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:20.090][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-59] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:36:22.091][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:22.091][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "36",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245783,
    "version": "1.5.6"
}

[25-09-19 09:36:24.092][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:24.093][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:26.094][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:26.094][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:28.095][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:28.095][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-60] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:36:30.096][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:30.097][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:32.097][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:32.097][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:34.098][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:34.099][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:36.100][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:36.100][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-60] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:36:38.101][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:38.102][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "37",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245798,
    "version": "1.5.6"
}

[25-09-19 09:36:40.103][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:40.103][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:42.103][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:42.104][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:44.104][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:44.105][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-60] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:36:46.105][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:46.106][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:48.107][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:48.107][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:50.108][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:50.109][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:52.110][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:52.110][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-60] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "38",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245813,
    "version": "1.5.6"
}

[25-09-19 09:36:54.111][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:54.112][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:56.113][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:56.113][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:36:58.113][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:36:58.114][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:00.114][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:00.115][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-62] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:37:02.116][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:02.116][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:04.116][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:04.117][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:06.117][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:06.118][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:08.119][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:08.119][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "39",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245828,
    "version": "1.5.6"
}

[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-59] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:37:10.120][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:10.121][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:12.121][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:12.122][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:14.123][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:14.123][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:16.124][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:16.124][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-59] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:37:18.125][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:18.126][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:20.126][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:20.127][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:20.373][my_btgatt_][att_debug_cb  213][thread1248][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:disconnect_cb() Channel 0xeeac8 disconnect
ed: Connection reset by peer

[25-09-19 09:37:20.374][my_btgatt_][att_disconnec 203][thread1248][D] Device disconnected: Connection reset by peer, 2799154352

[25-09-19 09:37:20.374][my_btgatt_][ble_server_ru 822][thread1248][D] [exit] mainloop run
[NETWK] GET BT STATUS : 2 [DISCONNECTED] [name:YDAS250701000005 addr:94:BA:06:91:04:0C]
[25-09-19 09:37:20.376][rk_bt_c_ad][operator()    248][thread1243][W] [BtBleRpcClient] State changed, addr: 94:BA:06:91:04:0C, dev: YDAS250701000005, state: 2
[NETWK] Update Network Status (3(NSTA_BT) value:0)
[NETWK] Init Wait WIFI/4G AutoConnect State=[1] !(0: NULL 1:Wlan 2:4G)!
[STAMP] USER LOGOUT AS !
[25-09-19 09:37:20.379][bt_ble_rpc][bluez_call_st 424][thread1248][I] bluez_call_state_sync_ timeout for cmd: [ble_state], ret code: [0], cost time ms: [5]
[EVENT] Module Recv Commands (send: StampController cmd:3))
[25-09-19 09:37:20.382][my_btgatt_][ble_server_ru 832][thread1248][D] delete mainloop_notify_fd_: [12]
[25-09-19 09:37:20.382][my_btgatt_][stop          343][thread1248][D] [===>] L2CapAttChannelListener::stop
[25-09-19 09:37:20.382][my_btgatt_][stop          347][thread1248][D] mainloop_remove_fd and close listen_sock_: [13]
[25-09-19 09:37:20.382][my_btgatt_][stop          354][thread1248][D] close accepted_sock_: [14]
[25-09-19 09:37:20.382][my_btgatt_][stop          360][thread1248][D] [exit] L2CapAttChannelListener::stop
[25-09-19 09:37:20.382][my_btgatt_][ble_server_ru 840][thread1248][D] destroy the core_pure_c_server_handle_ handle for [att] [gap] [gatt] resouce
[25-09-19 09:37:20.382][my_btgatt_][ble_server_ru 844][thread1248][D] [exit] server loop, count: [1]
[25-09-19 09:37:20.382][my_btgatt_][ble_server_ru 709][thread1248][D] [===>] server loop, count: [2]
[25-09-19 09:37:20.382][my_btgatt_][ble_server_ru 720][thread1248][D] set_advertising_data, adverting_device_name: [YDAS250701000005], advertisng_service_uuid: [0000180A-0000-1000-8000-00805F9B34FB]
[25-09-19 09:37:20.384][my_btgatt_][set_advertisi 150][thread1248][D] after bt_string_to_uuid, uuid type: 16
"[EVENT] Handle Finger Controller {md:FingerModule cmd:11(CTL_FINGER_CANCEL) arg:}"
[U**I] onHandleMainUICommand :  3
[25-09-19 09:37:20.389][my_btgatt_][ble_server_ru 740][thread1248][D] Attempt 1: set_advertising_data result: 0
[25-09-19 09:37:20.390][my_btgatt_][ble_server_ru 753][thread1248][D] set_device_name: [YDAS250701000005]
[25-09-19 09:37:20.390][my_btgatt_][ble_server_ru 756][thread1248][D] mainloop_init
[25-09-19 09:37:20.390][my_btgatt_][ble_server_ru 761][thread1248][D] create mainloop_notify_fd_: [12]
[25-09-19 09:37:20.390][my_btgatt_][ble_server_ru 776][thread1248][D] success, add mainloop_notify_fd_ to mainloop_fd, re: [0]
[25-09-19 09:37:20.390][my_btgatt_][start_listen_ 269][thread1248][D] [===>] start_listen_and_accept
[25-09-19 09:37:20.390][my_btgatt_][create_socket  55][thread1248][D] [===>] create_socket_
[25-09-19 09:37:20.390][my_btgatt_][create_socket  66][thread1248][D] create listen_sock_: [13]
[25-09-19 09:37:20.390][my_btgatt_][create_socket  79][thread1248][D] bind listen_sock_: [13]
[25-09-19 09:37:20.390][my_btgatt_][create_socket  88][thread1248][D] setsockopt listen_sock_: [13]
[25-09-19 09:37:20.390][my_btgatt_][create_socket  90][thread1248][D] start listen_sock_: [13]
[25-09-19 09:37:20.390][my_btgatt_][create_socket  97][thread1248][D] [exit] create_socket_
[25-09-19 09:37:20.390][my_btgatt_][start_listen_ 319][thread1248][D] mainloop_add_fd listen_sock_: [13]
[25-09-19 09:37:20.390][my_btgatt_][start_listen_ 332][thread1248][D] [exit] start_listen_and_accept
[25-09-19 09:37:20.390][my_btgatt_][ble_server_ru 820][thread1248][D] [===>] mainloop run
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-59] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
killall: rk_mpi_ao_test: no process killed
[SYS] Audio Cmd End:  "rk_mpi_ao_test -i /usr/audio/10024.wav --sound_card_name=hw:0,0 --device_ch=2 --device_rate=48000 --input_rate=48001 --input_ch=2 --set_volume=50 &"
cmd parse result:
input  file name      : /usr/audio/10024.wav
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
RTVersion        01:37:20-732 {dump              :064} ---------------------------------------------------------
RTVersion        01:37:20-732 {dump              :065} rockit version: git-7324009fa Fri Sep 1 14:55:53 2023 +0800
RTVersion        01:37:20-733 {dump              :066} rockit building: built- 2023-09-01 14:59:16
RTVersion        01:37:20-733 {dump              :067} ---------------------------------------------------------
(null)           01:37:20-733 {log_level_init    :206}

 please use echo name=level > /tmp/rt_log_level set log level
        name: all cmpi mb sys vdec venc rgn vpss vgs tde avs wbc vo vi ai ao aenc adec
        log_level: 0 1 2 3 4 5 6

rockit default level 4, can use export rt_log_level=x, x=0,1,2,3,4,5,6 change
(null)           01:37:20-734 {read_log_level    :097} text is all=4
(null)           01:37:20-734 {read_log_level    :099} module is all, log_level is 4
cmpi             01:37:20-738 {main              :823} start running loop count  = 0
(null)           01:37:20-741 {monitor_log_level :148} #Start monitor_log_level thread, arg:(nil)
cmpi             01:37:20-764 {test_init_mpi_ao  :226} Set volume curve type: 0
cmpi             01:37:20-764 {commandThread     :376} test info : mute = 0, volume = 50
cmpi             01:37:20-765 {sendDataThread    :309} params->s32ChnIndex : 0
[MEDIA] Handle Controller Cmd: send:MediaModule type:5 args:
[MEDIA] Set Photo Process: enable:0 Current(isTakeStart:0 isTakeEnable:0)
[MEDIA] Already Disable Takeing...!!!
[MEDIA] Set Video Process: enable:0  Current(isVideoStart:0 isVideoEnable:0)
[MEDIA] Already Disable Recording...!!!
[25-09-19 09:37:22.127][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:22.128][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
cmpi             01:37:22-257 {sendDataThread    :352} eof
cmpi             01:37:22-284 {main              :828} end running loop count  = 0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "40",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245842,
    "version": "1.5.6"
}

RKSockServer     01:37:22-739 {start             :162} accept failed
(null)           01:37:22-743 {monitor_log_level :189} monitor_log_level quit
[25-09-19 09:37:24.129][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:24.129][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:26.130][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:26.130][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:28.131][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:28.132][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-60] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4127mV curent:-18nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:37:30.132][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:30.133][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "41",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245850,
    "version": "1.5.6"
}

[25-09-19 09:37:32.133][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:32.134][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:34.134][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:34.135][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:36.135][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:36.136][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-61] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:37:38.137][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:38.138][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "42",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245858,
    "version": "1.5.6"
}

[25-09-19 09:37:40.139][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:40.139][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:42.140][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:42.140][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:44.141][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:44.141][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-60] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:37:46.142][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:46.143][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "43",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "bleMac": "4C:04:91:06:BA:94",
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 2,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758245866,
    "version": "1.5.6"
}

[25-09-19 09:37:48.143][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:48.144][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:50.145][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:50.146][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 09:37:52.146][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:52.147][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:0(idy:0) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-60] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4128mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 09:37:54.147][bt_ble_rpc][operator()     92][thread1235][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 09:37:54.148][bt_ble_rpc][operator()    223][thread1243][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "44",

```
