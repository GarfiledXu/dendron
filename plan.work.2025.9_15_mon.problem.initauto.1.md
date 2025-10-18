---
id: 8lisyd9kbb3sp0eee66sr2j
title: '1'
desc: ''
updated: 1758271591064
created: 1758271378301
---

## 场景，忽略网络后接收连接，

```bash
[25-09-19 16:41:24.637][bt_ble_rpc][operator()    223][thread2838][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 16:41:24.928][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:24.948][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:25.926][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:25.950][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:26.638][bt_ble_rpc][operator()     92][thread2830][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 16:41:26.639][bt_ble_rpc][operator()    223][thread2838][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 16:41:26.907][my_btgatt_][att_debug_cb  213][thread2843][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeff58) ATT receive
d: 46

[25-09-19 16:41:26.907][my_btgatt_][att_debug_cb  213][thread2843][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xeff58) ATT PDU rec
eived: 0x12

[25-09-19 16:41:26.907][my_btgatt_][gatt_debug_cb 220][thread2843][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|43|WIFISETTINGV2|1~1~Libawall~11223344|#76~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shareY║
[rkbt_write_cb] offset: 0, len: 43
Data: 2F 3E 7C 34 33 7C 57 49 46 49 53 45 54 54 49 4E 47 56 32 7C 31 7E 31 7E 4C 69 62 61 77 61 6C 6C 7E 31 31 32 32 33 33 34 34 7C 23
[25-09-19 16:41:26.908][bt_ble_rpc][operator()    239][thread2843][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|43|WIFISETTINGV2|1~1~Libawall~11223344|#76~2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shareY║], len: [43]
[25-09-19 16:41:26.908][rk_bt_c_ad][operator()    252][thread2838][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|43|WIFISETTINGV2|1~1~Libawall~11223344|#, len: 43
[COMM] Handle One Message( m_msgId=52 [type:1] topic:ble datas:/>|43|WIFISETTINGV2|1~1~Libawall~11223344|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:1|1|Libawall|11223344|52
[25-09-19 16:41:26.911][bt_ble_rpc][bluez_call_re 395][thread2843][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [3]
[25-09-19 16:41:26.912][my_btgatt_][gatt_debug_cb 220][thread2843][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-19 16:41:26.912][my_btgatt_][att_debug_cb  213][thread2843][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xeff58) ATT op
0x13

[25-09-19 16:41:26.914][my_btgatt_][att_debug_cb  213][thread2843][D] att: < 13                                               .

[25-09-19 16:41:26.950][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:26.957][global_api][glGetCurrentS2108][thread2813][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:26.992][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:27.000][rk_wifi_c_][Mock_RK_WifiC  66][thread2813][I] [===>] RK_WifiConnect
[25-09-19 16:41:27.003][rk_wifi_c_][Mock_RK_WifiC  70][thread2813][I] the matched wpa_network_config_list count: [0], with ssid: [Libawall], pask: [11223344]
[25-09-19 16:41:27.013][rk_wifi_c_][Mock_RK_WifiC  85][thread2813][D] WifiMngWpaImpl::ins().add_network, cur_network_id: [0]
[25-09-19 16:41:27.021][rk_wifi_c_][Mock_RK_WifiC  87][thread2813][D] WifiMngWpaImpl::ins().set_network_ssid: [true]
[25-09-19 16:41:27.140][rk_wifi_c_][Mock_RK_WifiC  89][thread2813][D] WifiMngWpaImpl::ins().set_network_psk: [true]
[25-09-19 16:41:27.152][rk_wifi_c_][Mock_RK_WifiC  91][thread2813][D] WifiMngWpaImpl::ins().save_config: [true]
[25-09-19 16:41:27.164][rk_wifi_c_][Mock_RK_WifiC  93][thread2813][D] WifiMngWpaImpl::ins().enable_network: [true]
[25-09-19 16:41:27.188][rk_wifi_c_][Mock_RK_WifiC  95][thread2813][D] WifiMngWpaImpl::ins().select_network: [true]
[25-09-19 16:41:27.188][rk_wifi_c_][Mock_RK_WifiC  97][thread2813][I] [exit] RK_WifiConnect
[25-09-19 16:41:27.930][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:27.951][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:28.156][wifi_mng_w][operator()   1073][thread2818][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-19 16:41:28.156][wifi_mng_w][operator()   1075][thread2818][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[25-09-19 16:41:28.639][bt_ble_rpc][operator()     92][thread2830][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 16:41:28.639][bt_ble_rpc][operator()    223][thread2838][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 16:41:28.930][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:28.949][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:29.926][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:29.947][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:30.641][bt_ble_rpc][operator()     92][thread2830][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 16:41:30.642][bt_ble_rpc][operator()    223][thread2838][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 16:41:30.930][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:30.949][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:31.178][wifi_mng_w][operator()   1073][thread2818][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-19 16:41:31.179][wifi_mng_w][operator()   1075][thread2818][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[25-09-19 16:41:31.927][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:31.949][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:-256] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4157mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:
0 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}
[25-09-19 16:41:32.185][wifi_mng_w][operator()   1073][thread2818][I] [on loop] start_polling_status_rk, enter callback, rk_state: [CONNECTED]
[NETWK] CB WIFI STATUS : 4
=== WPA Network List (count: 1) ===
Index: 0
  ID:     0
  SSID:   Libawall
  BSSID:  any
  FLAGS:        [CURRENT]
-----------------------------------
[25-09-19 16:41:32.193][rk_wifi_c_][print_saved_i 124][thread2818][I] Total saved networks: 1
[25-09-19 16:41:32.193][rk_wifi_c_][print_saved_i 134][thread2818][I]   [#0] SSID: 'Libawall', BSSID: 'any', State: '[CURRENT]'
[25-09-19 16:41:32.193][rk_wifi_c_][Mock_RK_WifiG 142][thread2818][I] [exit] RK_WifiGetSavedInfo: [0]
[NETWK] **********************
[NETWK] updateSaveApInfo {id=0 ssid=Libawall bssid=any state:[CURRENT]}
[NETWK] **********************
[NETWK] CONNETED WIFI SSID: (initAutoConnType:1) ssid:Libawall mac:94:ba:06:91:04:0b ip:192.168.49.26
[25-09-19 16:41:32.193][wlancontro][stCallbackRec 183][thread2818][W] set initAutoConnType 98
[NETWK] WIFI SET UDHCP (type:2) metric:98
[glSetUdhcpcOnMetric] IP output: "192.168.49.26"
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 98
[25-09-19 16:41:32.262][wlancontro][stCallbackRec 227][thread2818][W] will reset m_systemStatus->initAutoConnType to 0, current: [1]
[25-09-19 16:41:32.263][wlancontro][stCallbackRec 229][thread2818][W] set m_systemStatus->initAutoConnType to 0
[25-09-19 16:41:32.643][bt_ble_rpc][operator()     92][thread2830][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 16:41:32.643][bt_ble_rpc][operator()    223][thread2838][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 16:41:32.927][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:32.946][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:33.282][wifi_mng_w][operator()   1075][thread2818][I] [on loop] start_polling_status_rk, break callback, rk_state: [CONNECTED]
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[25-09-19 16:41:33.303][global_api][glGetCurrentS2108][thread2757][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:33.343][global_api][glGetCurrentS2108][thread2757][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:33.927][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:33.948][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:34.643][bt_ble_rpc][operator()     92][thread2830][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 16:41:34.644][bt_ble_rpc][operator()    223][thread2838][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 16:41:34.929][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:34.948][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:35.927][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:35.949][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:36.645][bt_ble_rpc][operator()     92][thread2830][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 16:41:36.645][bt_ble_rpc][operator()    223][thread2838][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 16:41:36.926][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:36.945][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "85",
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
    "time": 1758271296,
    "version": "1.5.6"
}

[25-09-19 16:41:37.925][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:37.946][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:38.646][bt_ble_rpc][operator()     92][thread2830][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-19 16:41:38.647][bt_ble_rpc][operator()    223][thread2838][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-19 16:41:38.927][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:38.950][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[25-09-19 16:41:39.926][global_api][glGetCurrentS2108][thread2748][W] glGetNetworkRouteMetric return metric: [98]
[25-09-19 16:41:39.947][devcheckco][checkRunningD 328][thread2748][W] get useNetwork: [2]
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:-52] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4157mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0
 isStampLock:0 useNetwork:2 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0 mediaUpNums:0}

```
