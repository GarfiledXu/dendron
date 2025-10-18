---
id: rn5ehbykljv0cl19rj3y6m1
title: Ble_process
desc: ''
updated: 1758029279258
created: 1758028633751
---

现场日志，依旧是有概率触发链接失效，在
```bash
[NETWK] GET BT STATUS : 1 [CONNECTED] [name:YDAS250701000005 addr:94:BA:06:91:04:0C]
[25-09-16 21:07:15.634][rk_bt_c_ad][operator()    246][thread2668][W] [BtBleRpcClient] State changed, addr: 94:BA:06:91:04:0C, dev: YDAS250701000005, state: 1
[25-09-16 21:07:15.635][bt_ble_rpc][bluez_call_st 424][thread2671][I] bluez_call_state_sync_ timeout for cmd: [ble_state], ret code: [0], cost time ms: [1]
[25-09-16 21:07:15.635][my_btgatt_][operator()    805][thread2671][D] [exit] l2cap_att_channel_listener_::accept_callback
[25-09-16 21:07:15.635][my_btgatt_][operator()    304][thread2671][D] end execute accept_callback_
[25-09-16 21:07:15.635][my_btgatt_][operator()    313][thread2671][D] [exit] l2cap listen socket cb, fd: [13], event: [1]
[NETWK] Update Network Status (3(NSTA_BT) value:1)
[25-09-16 21:07:15.686][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 3

[25-09-16 21:07:15.686][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x02

[25-09-16 21:07:15.686][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:exchange_mtu_cb() MTU exchange complete, with MTU: 185

[25-09-16 21:07:15.686][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x03

[25-09-16 21:07:15.687][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 03 00 02                                         ...

[25-09-16 21:07:15.747][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 7

[25-09-16 21:07:15.747][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x10

[25-09-16 21:07:15.747][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:read_by_grp_type_cb() Read By Grp Type - start: 0x0001 end: 0xffff

[25-09-16 21:07:15.747][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x11

[25-09-16 21:07:15.748][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 11 06 01 00 06 00 00 18 07 00 0a 00 01 18 0b 00  ................

[25-09-16 21:07:15.748][my_btgatt_][att_debug_cb  213][thread2671][D] att:   10 00 0a 18                                      ....

[25-09-16 21:07:15.837][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 7

[25-09-16 21:07:15.837][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x10

[25-09-16 21:07:15.837][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:read_by_grp_type_cb() Read By Grp Type - start: 0x0011 end: 0xffff

[25-09-16 21:07:15.837][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x01

[25-09-16 21:07:15.838][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 01 10 11 00 0a                                   .....

[25-09-16 21:07:15.897][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 7

[25-09-16 21:07:15.897][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x08

[25-09-16 21:07:15.897][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:read_by_type_cb() Read By Type - start: 0x0007 end: 0x000a

[25-09-16 21:07:15.897][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x09

[25-09-16 21:07:15.898][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 09 07 08 00 22 09 00 05 2a                       ...."...*

[25-09-16 21:07:15.957][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 5

[25-09-16 21:07:15.957][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x04

[25-09-16 21:07:15.957][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:find_info_cb() Find Info - start: 0x000a end: 0x000a

[25-09-16 21:07:15.957][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x05

[25-09-16 21:07:15.958][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 05 01 0a 00 02 29                                .....)

[25-09-16 21:07:16.017][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 5

[25-09-16 21:07:16.017][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x12

[25-09-16 21:07:16.017][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_cb() Write Req - handle: 0x000a

[25-09-16 21:07:16.017][my_btgatt_][gatt_svc_chng 345][thread2671][D] Service Changed CCC Write called

[25-09-16 21:07:16.017][my_btgatt_][gatt_svc_chng 365][thread2671][D] Service Changed Enabled: true

[25-09-16 21:07:16.017][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_complete_cb() Write Complete: err 0

[25-09-16 21:07:16.017][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x13

[25-09-16 21:07:16.018][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 13                                               .

[25-09-16 21:07:16.077][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 7

[25-09-16 21:07:16.077][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x08

[25-09-16 21:07:16.077][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:read_by_type_cb() Read By Type - start: 0x000b end: 0x0010

[25-09-16 21:07:16.078][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x09

[25-09-16 21:07:16.078][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 09 07 0c 00 1a 0d 00 99 99                       .........

[25-09-16 21:07:16.137][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 7

[25-09-16 21:07:16.137][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x08

[25-09-16 21:07:16.137][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:read_by_type_cb() Read By Type - start: 0x000e end: 0x0010

[25-09-16 21:07:16.138][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x09

[25-09-16 21:07:16.139][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 09 15 0e 00 1a 0f 00 f9 47 dc e3 8b eb 48 82 f7  ........G....H..

[25-09-16 21:07:16.139][my_btgatt_][att_debug_cb  213][thread2671][D] att:   47 10 18 6e 41 d4 df                             G..nA..

[25-09-16 21:07:16.197][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 7

[25-09-16 21:07:16.197][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x08

[25-09-16 21:07:16.197][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:read_by_type_cb() Read By Type - start: 0x000b end: 0x0010

[25-09-16 21:07:16.197][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x01

[25-09-16 21:07:16.198][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 01 08 0b 00 0a                                   .....

[25-09-16 21:07:16.257][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 5

[25-09-16 21:07:16.257][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x04

[25-09-16 21:07:16.257][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:find_info_cb() Find Info - start: 0x0010 end: 0x0010

[25-09-16 21:07:16.257][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x05

[25-09-16 21:07:16.258][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 05 01 10 00 02 29                                .....)

[25-09-16 21:07:16.406][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 5

[25-09-16 21:07:16.406][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x12

[25-09-16 21:07:16.406][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_cb() Write Req - handle: 0x0010

Wifi: Service Changed CCC Write called [1:0]
Wifi: Service Changed Enabled: true
[25-09-16 21:07:16.407][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_complete_cb() Write Complete: err 0

[25-09-16 21:07:16.407][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x13

[25-09-16 21:07:16.407][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 13                                               .

[25-09-16 21:07:17.098][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 87

[25-09-16 21:07:17.099][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x12

[25-09-16 21:07:17.099][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_cb() Write Req - handle: 0x000d

enter rkbt_write_cb, data: />|78|DEVICEMATCHV2|徐佳飞~83185~1758028036~c736e1a8d5b90b6383d882e2628316e9~3~|#
[rkbt_write_cb] offset: 0, len: 84
Data: 2F 3E 7C 37 38 7C 44 45 56 49 43 45 4D 41 54 43 48 56 32 7C E5 BE 90 E4 BD B3 E9 A3 9E 7E 38 33 31 38 35 7E 31 37 35 38 30 32 38 30 33 36 7E 63 37 33 36 65 31 61 38 64 35 62 39 30 62 3
6 33 38 33 64 38 38 32 65 32 36 32 38 33 31 36 65 39 7E 33 7E 7C 23
[25-09-16 21:07:17.099][bt_ble_rpc][operator()    239][thread2671][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|78|DEVICEMATCHV2|徐佳飞~83185~1758028036~c736e1a8d5b
90b6383d882e2628316e9~3~|#], len: [84]
[25-09-16 21:07:17.100][rk_bt_c_ad][operator()    250][thread2668][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|78|DEVICEMATCHV2|徐佳飞~83185~175802
8036~c736e1a8d5b90b6383d882e2628316e9~3~|#, len: 84
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|78|DEVICEMATCHV2|徐佳飞~83185~1758028036~c736e1a8d5b90b6383d882e2628316e9~3~|# )
[25-09-16 21:07:17.105][bt_ble_rpc][bluez_call_re 395][thread2671][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [5]
[25-09-16 21:07:17.105][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_complete_cb() Write Complete: err 0

[25-09-16 21:07:17.105][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x13

[25-09-16 21:07:17.106][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 13                                               .

[COMM] Sync Date Time(2025-09-16 21:07:17) to Local! Get Local(2025-09-16 21:07:17)
[25-09-16 21:07:17.018][commandcon][syncLocalDate 118][thread2603][W] Server epoch: [1758028036]
[25-09-16 21:07:17.019][commandcon][syncLocalDate 119][thread2603][W] UTC time: [1970-01-21 08:20:28]
[25-09-16 21:07:17.019][commandcon][syncLocalDate 120][thread2603][W] Local time: [2025-09-16 21:07:17]
[COMM] USER LOGIN (staffId:83185 staffName:徐佳飞) !
[U**I] onHandleMainUICommand :  2
[25-09-16 21:07:17.039][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|30|DEVICEMATCHV2|OK~0|徐佳飞~83185~4
|#]
[25-09-16 21:07:17.040][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:17.040][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [44]
[25-09-16 21:07:17.040][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:07:17.041][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 30 7c 44 45 56 49 43 45 4d  .../>|30|DEVICEM

[25-09-16 21:07:17.041][my_btgatt_][att_debug_cb  213][thread2671][D] att:   41 54 43 48 56 32 7c 4f 4b 7e 30 7c e5 be 90 e4  ATCHV2|OK~0|....

[25-09-16 21:07:17.041][my_btgatt_][att_debug_cb  213][thread2671][D] att:   bd b3 e9 a3 9e 7e 38 33 31 38 35 7e 34 7c 23     .....~83185~4|#

[25-09-16 21:07:17.042][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|30|DEVICEMATCHV2|OK~0|徐佳飞~83185~4|#]
[25-09-16 21:07:17.042][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|30|DEVICEMATCHV2|OK~0|徐佳飞~83185~4
|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|30|DEVICEMATCHV2|OK~0|徐佳飞~83185~4|#][size:0 ver:2]
[25-09-16 21:07:17.077][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 92

[25-09-16 21:07:17.077][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x12

[25-09-16 21:07:17.077][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_cb() Write Req - handle: 0x000d

enter rkbt_write_cb, data: />|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#
[rkbt_write_cb] offset: 0, len: 89
Data: 2F 3E 7C 37 31 7C 44 45 56 49 43 45 49 4E 46 4F 53 45 54 56 32 7C 2D 31 7E 33 7E 31 7E 31 7E 31 7E 31 7E 31 7E 31 7E 31 7E 37 30 37 36 7E 32 7E E5 BE 90 E4 BD B3 E9 A3 9E 2D 73 6D 61 7
2 74 2D E5 A5 A5 E7 89 B9 E4 B9 8B E7 AB A0 E9 87 8D E7 94 9F 7E 2D 31 7E 31 7C 23
[25-09-16 21:07:17.077][bt_ble_rpc][operator()    239][thread2671][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~1~7076~2~徐佳飞-
smart-奥特之章重生~-1~1|#], len: [89]
[25-09-16 21:07:17.078][rk_bt_c_ad][operator()    250][thread2668][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~
1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#, len: 89
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|71|DEVICEINFOSETV2|-1~3~1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|# )
[25-09-16 21:07:17.081][bt_ble_rpc][bluez_call_re 395][thread2671][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [2]
[25-09-16 21:07:17.081][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_complete_cb() Write Complete: err 0

[25-09-16 21:07:17.081][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x13

[25-09-16 21:07:17.082][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 13                                               .

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
VERSION:1.5.1
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

[25-09-16 21:07:17.107][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:17.107][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
killall: rk_mpi_ao_test: no process killed
[SYS] Audio Cmd End:  "rk_mpi_ao_test -i /usr/audio/10012.wav --sound_card_name=hw:0,0 --device_ch=2 --device_rate=48000 --input_rate=48001 --input_ch=2 --set_volume=50 &"
[25-09-16 21:07:17.233][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|25|DEVICEINFOSETV2|OK~0|#]
[25-09-16 21:07:17.234][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:17.234][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [28]
[25-09-16 21:07:17.234][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:07:17.235][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 32 35 7c 44 45 56 49 43 45 49  .../>|25|DEVICEI

[25-09-16 21:07:17.235][my_btgatt_][att_debug_cb  213][thread2671][D] att:   4e 46 4f 53 45 54 56 32 7c 4f 4b 7e 30 7c 23     NFOSETV2|OK~0|#

[25-09-16 21:07:17.235][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|25|DEVICEINFOSETV2|OK~0|#]
[25-09-16 21:07:17.236][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|25|DEVICEINFOSETV2|OK~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|25|DEVICEINFOSETV2|OK~0|#][size:0 ver:2]
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
RTVersion        13:07:17-250 {dump              :064} ---------------------------------------------------------
RTVersion        13:07:17-250 {dump              :065} rockit version: git-7324009fa Fri Sep 1 14:55:53 2023 +0800
RTVersion        13:07:17-250 {dump              :066} rockit building: built- 2023-09-01 14:59:16
RTVersion        13:07:17-250 {dump              :067} ---------------------------------------------------------
(null)           13:07:17-251 {log_level_init    :206}

 please use echo name=level > /tmp/rt_log_level set log level
        name: all cmpi mb sys vdec venc rgn vpss vgs tde avs wbc vo vi ai ao aenc adec
        log_level: 0 1 2 3 4 5 6

rockit default level 4, can use export rt_log_level=x, x=0,1,2,3,4,5,6 change
(null)           13:07:17-251 {read_log_level    :097} text is all=4
(null)           13:07:17-252 {read_log_level    :099} module is all, log_level is 4
cmpi             13:07:17-256 {main              :823} start running loop count  = 0
(null)           13:07:17-258 {monitor_log_level :148} #Start monitor_log_level thread, arg:(nil)
cmpi             13:07:17-282 {test_init_mpi_ao  :226} Set volume curve type: 0
cmpi             13:07:17-283 {commandThread     :376} test info : mute = 0, volume = 50
cmpi             13:07:17-283 {sendDataThread    :309} params->s32ChnIndex : 0
[25-09-16 21:07:17.288][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 30

[25-09-16 21:07:17.288][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x12

[25-09-16 21:07:17.288][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_cb() Write Req - handle: 0x000d

enter rkbt_write_cb, data: />|27|PRIVILEGEDATAV2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#
[rkbt_write_cb] offset: 0, len: 27
Data: 2F 3E 7C 32 37 7C 50 52 49 56 49 4C 45 47 45 44 41 54 41 56 32 7C 31 30 30 7C 23
[25-09-16 21:07:17.288][bt_ble_rpc][operator()    239][thread2671][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|27|PRIVILEGEDATAV2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-
smart-奥特之章重生~-1~1|#], len: [27]
[25-09-16 21:07:17.289][rk_bt_c_ad][operator()    250][thread2668][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|27|PRIVILEGEDATAV2|100|#, len: 27
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|27|PRIVILEGEDATAV2|100|# )
[25-09-16 21:07:17.290][bt_ble_rpc][bluez_call_re 395][thread2671][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [2]
[25-09-16 21:07:17.291][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_complete_cb() Write Complete: err 0

[25-09-16 21:07:17.291][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x13

[25-09-16 21:07:17.292][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 13                                               .

[25-09-16 21:07:17.427][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|99|PRIVILEGEDATAV2|OK~0|0~3~0~[]|#]
[25-09-16 21:07:17.427][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:17.427][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [37]
[25-09-16 21:07:17.428][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:07:17.428][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 39 39 7c 50 52 49 56 49 4c 45  .../>|99|PRIVILE

[25-09-16 21:07:17.428][my_btgatt_][att_debug_cb  213][thread2671][D] att:   47 45 44 41 54 41 56 32 7c 4f 4b 7e 30 7c 30 7e  GEDATAV2|OK~0|0~

[25-09-16 21:07:17.429][my_btgatt_][att_debug_cb  213][thread2671][D] att:   33 7e 30 7e 5b 5d 7c 23                          3~0~[]|#

[25-09-16 21:07:17.429][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|99|PRIVILEGEDATAV2|OK~0|0~3~0~[]|#]
[25-09-16 21:07:17.430][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|99|PRIVILEGEDATAV2|OK~0|0~3~0~[]|#],
 ret: [0]
[NETWK] BT Write Data(ret=0): [/>|99|PRIVILEGEDATAV2|OK~0|0~3~0~[]|#][size:0 ver:2]
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4155mV curent:-45nA isLessDisk:0 remainSize:2354
9(less:200)MB isLow:0 infrared:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
cmpi             13:07:18-635 {sendDataThread    :352} eof
cmpi             13:07:18-662 {main              :828} end running loop count  = 0
[25-09-16 21:07:18.724][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 7

[25-09-16 21:07:18.725][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x08

[25-09-16 21:07:18.725][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:read_by_type_cb() Read By Type - start: 0x0001 end: 0x0006

[25-09-16 21:07:18.725][my_btgatt_][gap_device_na 233][thread2671][D] GAP Device Name Read called, 2798847152

[25-09-16 21:07:18.725][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x09

[25-09-16 21:07:18.726][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 09 13 03 00 59 44 41 53 32 35 30 37 30 31 30 30  ....YDAS25070100

[25-09-16 21:07:18.726][my_btgatt_][att_debug_cb  213][thread2671][D] att:   30 30 30 35 00                                   0005.

[25-09-16 21:07:18.875][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 23

[25-09-16 21:07:18.875][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x12

[25-09-16 21:07:18.875][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_cb() Write Req - handle: 0x000d

enter rkbt_write_cb, data: />|20|WIFILISTV2|~|#2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#
[rkbt_write_cb] offset: 0, len: 20
Data: 2F 3E 7C 32 30 7C 57 49 46 49 4C 49 53 54 56 32 7C 7E 7C 23
[25-09-16 21:07:18.875][bt_ble_rpc][operator()    239][thread2671][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|20|WIFILISTV2|~|#2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-
smart-奥特之章重生~-1~1|#], len: [20]
[25-09-16 21:07:18.875][rk_bt_c_ad][operator()    250][thread2668][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|20|WIFILISTV2|~|#, len: 20
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|20|WIFILISTV2|~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:2(CTL_NETWORK_SCAN_APS) args:1
m_isScan= false
[25-09-16 21:07:18.877][bt_ble_rpc][bluez_call_re 395][thread2671][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [2]
[25-09-16 21:07:18.878][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_complete_cb() Write Complete: err 0

[25-09-16 21:07:18.878][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x13

[25-09-16 21:07:18.879][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 13                                               .

[25-09-16 21:07:18.889][rk_wifi_c_][Mock_RK_WifiS  41][thread2654][I] RK_WifiScan, ret: [true]
Run RK_WifiScan :  true
[25-09-16 21:07:19.107][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:19.108][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
RKSockServer     13:07:19-258 {start             :162} accept failed
(null)           13:07:19-261 {monitor_log_level :189} monitor_log_level quit
[NETWK] CB WIFI STATUS : 8
Scan complete in 545 ms, found networks:
e2:c3:22:6a:b0:8f       5745    -49     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
cc:d8:43:09:96:52       5180    -60     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ceshi_5G
08:9b:4b:42:a9:62       5240    -46     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:a9:62       5240    -46     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:b3:d3:79       5180    -57     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:b3:d3:79       5180    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:b3:d3:79       5180    -57     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:ab:27       5200    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b1:54       5240    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:42:ab:27       5200    -59     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:bb:b1:54       5240    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:a9:56       5220    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:a9:56       5220    -66     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:bb:b1:68       5200    -67     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:bb:b1:68       5200    -66     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:ab:18       5220    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:ab:18       5220    -68     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:ab:18       5220    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
14:d8:64:8d:f4:1b       2412    -56     [WPA2-PSK-CCMP][ESS]    TP-LINK_F41B
b8:08:d7:18:81:bc       2462    -46     [WPA2-PSK-CCMP][ESS]    zzwifi
e2:c3:22:6a:b0:8b       2412    -46     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
08:9b:4b:42:a9:61       2412    -54     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
0e:9b:4b:42:a9:61       2412    -48     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:61       2412    -48     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:b3:d3:78       2437    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:b3:d3:78       2437    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:b3:d3:78       2437    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
0e:9b:4b:bb:b1:70       5220    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:70       5220    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:bb:b1:70       5220    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:ab:12       5745    -74     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:12       5745    -74     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:b3:d3:7f       5765    -73     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:ab:12       5745    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:b3:d3:7f       5765    -73     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:b3:d3:7f       5765    -73     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:b4:d8       5200    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:b4:d8       5200    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:55       2437    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b1:53       2462    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
12:9b:4b:42:b4:de       5180    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:b4:de       5180    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
c6:d8:d5:2d:82:7e       2412    -82     [WPA2-PSK-CCMP+TKIP][ESS]       SLLWC
0e:9b:4b:bb:b1:53       2462    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:bb:b1:67       2462    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:ab:26       2462    -57     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
12:9b:4b:bb:b1:53       2462    -63     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:b4:de       5180    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:bb:b2:20       5765    -76     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b2:20       5765    -76     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b2:20       5765    -76     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
cc:d8:43:09:96:51       2422    -60     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ceshi
12:9b:4b:42:ab:17       2412    -82     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:ab:17       2412    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
88:2a:5e:85:40:31       5220    -85     [WPA2-PSK-CCMP][ESS]    IToIP
12:9b:4b:42:ab:26       2462    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:ab:0c       5805    -86     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
88:2a:5e:85:7c:c1       5260    -86     [WPA2-PSK-CCMP][ESS]    IToIP
[25-09-16 21:07:20.439][rk_wifi_c_][Mock_RK_WifiS  50][thread2672][I] get scan result success
[25-09-16 21:07:20.466][rk_wifi_c_][Mock_RK_WifiS  60][thread2672][I] RK_WifiScanResults, result: [{"ssid":"ChinaNet-TNT","rssi":-49,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","c
urrlink":0,"save":0}
{"ssid":"ceshi_5G","rssi":-60,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-46,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-46,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-57,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-57,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-59,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-66,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-67,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-66,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-68,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"TP-LINK_F41B","rssi":-56,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"zzwifi","rssi":-46,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-46,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-54,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-48,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-48,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-74,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-74,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-73,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-73,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-73,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"SLLWC","rssi":-82,"flags":"[WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-57,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-63,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-76,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-76,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-76,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ceshi","rssi":-60,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-82,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"IToIP","rssi":-85,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-86,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"IToIP","rssi":-86,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
]
[NETWK] Start Parse Wifi Scan Info! (linkSSID:
[25-09-16 21:07:20.480][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-16 21:07:20.480][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[25-09-16 21:07:20.490][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#
]
[25-09-16 21:07:20.490][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:20.490][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-16 21:07:20.491][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:07:20.491][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 4c 49 53  .../>|33|WIFILIS

[25-09-16 21:07:20.492][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 30 7e 31 7e 75 6e 6b  TV2|OK~0|0~1~unk

[25-09-16 21:07:20.492][my_btgatt_][att_debug_cb  213][thread2671][D] att:   6e 6f 77 6e 7e 30 7e 30 7c 23                    nown~0~0|#

[25-09-16 21:07:20.492][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#]
[25-09-16 21:07:20.493][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#
], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#][size:0 ver:2]
[25-09-16 21:07:20.704][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-46~1~Libawall-5G
~0~0|#]
[25-09-16 21:07:20.704][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:20.705][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [45]
[25-09-16 21:07:20.705][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:07:20.705][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 39 7c 57 49 46 49 4c 49 53  .../>|39|WIFILIS

[25-09-16 21:07:20.706][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 36 7e 31 7e 4c  TV2|OK~0|-46~1~L

[25-09-16 21:07:20.706][my_btgatt_][att_debug_cb  213][thread2671][D] att:   69 62 61 77 61 6c 6c 2d 35 47 7e 30 7e 30 7c 23  ibawall-5G~0~0|#

[25-09-16 21:07:20.706][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|39|WIFILISTV2|OK~0|-46~1~Libawall-5G~0~0|#]
[25-09-16 21:07:20.706][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-46~1~Libawall-5G
~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|39|WIFILISTV2|OK~0|-46~1~Libawall-5G~0~0|#][size:0 ver:2]
[25-09-16 21:07:20.916][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-46~1~device~0~0|
#]
[25-09-16 21:07:20.917][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:20.917][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-16 21:07:20.917][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:07:20.918][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 34 7c 57 49 46 49 4c 49 53  .../>|34|WIFILIS

[25-09-16 21:07:20.918][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 36 7e 31 7e 64  TV2|OK~0|-46~1~d

[25-09-16 21:07:20.918][my_btgatt_][att_debug_cb  213][thread2671][D] att:   65 76 69 63 65 7e 30 7e 30 7c 23                 evice~0~0|#

[25-09-16 21:07:20.919][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|34|WIFILISTV2|OK~0|-46~1~device~0~0|#]
[25-09-16 21:07:20.919][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-46~1~device~0~0|
#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-46~1~device~0~0|#][size:0 ver:2]
[25-09-16 21:07:21.108][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:21.109][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:21.130][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-46~1~zzwifi~0~0|
#]
[25-09-16 21:07:21.130][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:21.130][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-16 21:07:21.131][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:07:21.131][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 34 7c 57 49 46 49 4c 49 53  .../>|34|WIFILIS

[25-09-16 21:07:21.131][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 36 7e 31 7e 7a  TV2|OK~0|-46~1~z

[25-09-16 21:07:21.131][my_btgatt_][att_debug_cb  213][thread2671][D] att:   7a 77 69 66 69 7e 30 7e 30 7c 23                 zwifi~0~0|#

[25-09-16 21:07:21.132][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|34|WIFILISTV2|OK~0|-46~1~zzwifi~0~0|#]
[25-09-16 21:07:21.132][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-46~1~zzwifi~0~0|
#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-46~1~zzwifi~0~0|#][size:0 ver:2]
[25-09-16 21:07:21.343][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-46~1~ChinaNet-TN
T~0~0|#]
[25-09-16 21:07:21.343][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:21.343][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-16 21:07:21.344][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [0], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|40|WIFILISTV2|OK~0|-46~1~ChinaNet-TNT~0~0|#]
[25-09-16 21:07:21.344][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-46~1~ChinaNet-TN
T~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-46~1~ChinaNet-TNT~0~0|#][size:0 ver:2]
[25-09-16 21:07:21.555][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#
]
[25-09-16 21:07:21.555][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:21.556][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-16 21:07:21.556][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#]
[25-09-16 21:07:21.556][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#
], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-48~1~guest~0~0|#][size:0 ver:2]
[25-09-16 21:07:21.766][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-54~1~Libawall~0~
0|#]
[25-09-16 21:07:21.767][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:21.767][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-16 21:07:21.768][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [0], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|36|WIFILISTV2|OK~0|-54~1~Libawall~0~0|#]
[25-09-16 21:07:21.768][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-54~1~Libawall~0~
0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-54~1~Libawall~0~0|#][size:0 ver:2]
[25-09-16 21:07:21.979][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-56~1~TP-LINK_F41
B~0~0|#]
[25-09-16 21:07:21.979][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:21.979][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-16 21:07:21.980][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [0], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|40|WIFILISTV2|OK~0|-56~1~TP-LINK_F41B~0~0|#]
[25-09-16 21:07:21.980][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-56~1~TP-LINK_F41
B~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-56~1~TP-LINK_F41B~0~0|#][size:0 ver:2]
[25-09-16 21:07:22.190][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-60~1~ceshi_5G~0~
0|#]
[25-09-16 21:07:22.191][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:22.191][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-16 21:07:22.191][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|36|WIFILISTV2|OK~0|-60~1~ceshi_5G~0~0|#]
[25-09-16 21:07:22.192][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-60~1~ceshi_5G~0~
0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-60~1~ceshi_5G~0~0|#][size:0 ver:2]
[25-09-16 21:07:22.390][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-60~1~ceshi~0~0|#
]
[25-09-16 21:07:22.392][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:22.392][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-16 21:07:22.395][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [5], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|33|WIFILISTV2|OK~0|-60~1~ceshi~0~0|#]
[25-09-16 21:07:22.395][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-60~1~ceshi~0~0|#
], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-60~1~ceshi~0~0|#][size:0 ver:2]
[SYS] >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>KEY_PRESS! 0
[25-09-16 21:07:22.497][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter callback, rk_state: [OPEN]
[NETWK] CB WIFI STATUS : 6
=== WPA Network List (count: 0) ===
[25-09-16 21:07:22.504][rk_wifi_c_][print_saved_i 117][thread2672][I] Total saved networks: 0
[25-09-16 21:07:22.505][rk_wifi_c_][Mock_RK_WifiG 135][thread2672][I] [exit] RK_WifiGetSavedInfo: [0]
[25-09-16 21:07:22.505][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break callback, rk_state: [OPEN]
[SYS] >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>KEY_RESET! [oper:0 (54 ms)]!
[25-09-16 21:07:23.108][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:23.109][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:25.108][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:25.109][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4164mV curent:-12nA isLessDisk:0 remainSize:2354
9(less:200)MB isLow:0 infrared:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[25-09-16 21:07:26.586][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 49

[25-09-16 21:07:26.586][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x12

[25-09-16 21:07:26.586][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_cb() Write Req - handle: 0x000d

enter rkbt_write_cb, data: />|46|WIFISETTINGV2|1~1~Libawall-5G~11223344|#2~徐佳飞-smart-奥特之章重生~-1~1|#
[rkbt_write_cb] offset: 0, len: 46
Data: 2F 3E 7C 34 36 7C 57 49 46 49 53 45 54 54 49 4E 47 56 32 7C 31 7E 31 7E 4C 69 62 61 77 61 6C 6C 2D 35 47 7E 31 31 32 32 33 33 34 34 7C 23
[25-09-16 21:07:26.586][bt_ble_rpc][operator()    239][thread2671][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|46|WIFISETTINGV2|1~1~Libawall-5G~11223344|#2~徐佳飞-
smart-奥特之章重生~-1~1|#], len: [46]
[25-09-16 21:07:26.587][rk_bt_c_ad][operator()    250][thread2668][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|46|WIFISETTINGV2|1~1~Libawall-5G~112
23344|#, len: 46
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|46|WIFISETTINGV2|1~1~Libawall-5G~11223344|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:1|1|Libawall-5G|11223344|1
[25-09-16 21:07:26.591][bt_ble_rpc][bluez_call_re 395][thread2671][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [4]
[25-09-16 21:07:26.591][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_complete_cb() Write Complete: err 0

[25-09-16 21:07:26.627][rk_wifi_c_][Mock_RK_WifiC  66][thread2654][I] [===>] RK_WifiConnect
[25-09-16 21:07:26.627][rk_wifi_c_][Mock_RK_WifiC  70][thread2654][I] the matched wpa_network_config_list count: [0], with ssid: [Libawall-5G], pask: [11223344]
[25-09-16 21:07:26.780][rk_wifi_c_][Mock_RK_WifiC  90][thread2654][I] [exit] RK_WifiConnect
[25-09-16 21:07:27.110][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:27.110][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:27.542][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter callback, rk_state: [CONNECTED]
[NETWK] CB WIFI STATUS : 4
=== WPA Network List (count: 1) ===
Index: 0
  ID:     0
  SSID:   Libawall-5G
  BSSID:  any
  FLAGS:        [CURRENT]
-----------------------------------
[25-09-16 21:07:27.549][rk_wifi_c_][print_saved_i 117][thread2672][I] Total saved networks: 1
[25-09-16 21:07:27.549][rk_wifi_c_][print_saved_i 127][thread2672][I]   [#0] SSID: 'Libawall-5G', BSSID: 'any', State: '[CURRENT]'
[25-09-16 21:07:27.549][rk_wifi_c_][Mock_RK_WifiG 135][thread2672][I] [exit] RK_WifiGetSavedInfo: [0]
[NETWK] **********************
[NETWK] updateSaveApInfo {id=0 ssid=Libawall-5G bssid=any state:[CURRENT]}
[NETWK] **********************
[NETWK] CONNETED WIFI SSID: (initAutoConnType:0) ssid:Libawall-5G mac:94:ba:06:91:04:0b ip:192.168.48.60
[NETWK] CONNETED pthis->cfgType: 1 cfgPreNetRoute: 1
[NETWK] WIFI SET UDHCP (type:2) metric:100
[glSetUdhcpcOnMetric] IP output: "192.168.48.60"
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 100
[25-09-16 21:07:27.689][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|30|WIFISETTINGV2|OK~0|1~connect|#]
[25-09-16 21:07:27.690][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:07:27.690][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [36]
[25-09-16 21:07:27.691][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [0], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|30|WIFISETTINGV2|OK~0|1~connect|#]
[25-09-16 21:07:27.691][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|30|WIFISETTINGV2|OK~0|1~connect|#],
ret: [0]
[NETWK] BT Write Data(ret=0): [/>|30|WIFISETTINGV2|OK~0|1~connect|#][size:0 ver:2]
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "16",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 1,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758028048,
    "version": "1.5.1"
}

[25-09-16 21:07:28.629][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break callback, rk_state: [CONNECTED]
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[25-09-16 21:07:29.111][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:29.112][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:31.112][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:31.112][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:33.113][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:33.114][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:0] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4160mV curent:-36nA isLessDisk:0 remainSize:2354
9(less:200)MB isLow:0 infrared:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[25-09-16 21:07:35.115][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:35.116][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:37.115][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:37.116][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:39.116][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:39.117][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:41.118][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:41.118][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:0] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4164mV curent:-12nA isLessDisk:0 remainSize:2354
9(less:200)MB isLow:0 infrared:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[25-09-16 21:07:43.118][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:43.119][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "17",
    "params": {
        "baseStation": {
            "accessType": 0
        },
        "energy": 100,
        "energyType": 2,
        "locateMode": 0,
        "network": 1,
        "plank": "TX_SMART_MB_V05",
        "sealId": 7076,
        "sealName": "徐佳飞-smart-奥特之章重生",
        "sim": "89860855102481537262"
    },
    "systemVersion": "YDN_1.1",
    "time": 1758028063,
    "version": "1.5.1"
}

[25-09-16 21:07:45.120][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:45.120][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:47.121][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:47.122][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:49.122][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:49.122][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:0] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4164mV curent:-22nA isLessDisk:0 remainSize:2354
9(less:200)MB isLow:0 infrared:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[25-09-16 21:07:51.123][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:51.123][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:53.124][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:53.125][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:55.126][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:07:55.126][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:07:56.586][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:disconne
ct_cb() Channel 0xede38 disconnected: Connection reset by peer

[25-09-16 21:07:56.586][my_btgatt_][att_disconnec 203][thread2671][D] Device disconnected: Connection reset by peer, 2798847152

[25-09-16 21:07:56.586][my_btgatt_][ble_server_ru 822][thread2671][D] [exit] mainloop run
[NETWK] GET BT STATUS : 2 [DISCONNECTED] [name:YDAS250701000005 addr:94:BA:06:91:04:0C]
[NETWK] Update Network Status (3(NSTA_BT) value:0)
[STAMP] USER LOGOUT AS !
[25-09-16 21:07:56.588][rk_bt_c_ad][operator()    246][thread2668][W] [BtBleRpcClient] State changed, addr: 94:BA:06:91:04:0C, dev: YDAS250701000005, state: 2
[25-09-16 21:07:56.590][bt_ble_rpc][bluez_call_st 424][thread2671][I] bluez_call_state_sync_ timeout for cmd: [ble_state], ret code: [0], cost time ms: [3]
[25-09-16 21:07:56.591][my_btgatt_][ble_server_ru 832][thread2671][D] delete mainloop_notify_fd_: [12]
[25-09-16 21:07:56.591][my_btgatt_][stop          343][thread2671][D] [===>] L2CapAttChannelListener::stop
[25-09-16 21:07:56.591][my_btgatt_][stop          347][thread2671][D] mainloop_remove_fd and close listen_sock_: [13]
[25-09-16 21:07:56.591][my_btgatt_][stop          354][thread2671][D] close accepted_sock_: [14]
[25-09-16 21:07:56.591][my_btgatt_][stop          360][thread2671][D] [exit] L2CapAttChannelListener::stop
[25-09-16 21:07:56.591][my_btgatt_][ble_server_ru 840][thread2671][D] destroy the core_pure_c_server_handle_ handle for [att] [gap] [gatt] resouce
[25-09-16 21:07:56.591][my_btgatt_][ble_server_ru 844][thread2671][D] [exit] server loop, count: [3]
[25-09-16 21:07:56.592][my_btgatt_][ble_server_ru 709][thread2671][D] [===>] server loop, count: [4]
[25-09-16 21:07:56.592][my_btgatt_][ble_server_ru 720][thread2671][D] set_advertising_data, adverting_device_name: [YDAS250701000005], advertisng_service_uuid: [0000180A-0000-1000-8000-00805
F9B34FB]
[25-09-16 21:07:56.593][my_btgatt_][set_advertisi 150][thread2671][D] after bt_string_to_uuid, uuid type: 16

```



接口返回失败，并且后续均不能正常发送
[25-09-16 21:24:16.051][my_btgatt_][ble_server_no 465][thread2664][E] bt_gatt_server_send_notification, ret: [false]
```bash
-09-16 21:24:13.133][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
=== WPA Network List (count: 0) ===
[25-09-16 21:24:13.140][rk_wifi_c_][print_saved_i 117][thread2672][I] Total saved networks: 0
[25-09-16 21:24:13.140][rk_wifi_c_][Mock_RK_WifiG 135][thread2672][I] [exit] RK_WifiGetSavedInfo: [0]
[25-09-16 21:24:13.140][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break callback, rk_state: [DISCONNECTED]
[25-09-16 21:24:13.205][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT received: 23

[25-09-16 21:24:13.205][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read
_data() (chan 0xede38) ATT PDU received: 0x12

[25-09-16 21:24:13.205][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_cb() Write Req - handle: 0x000d

enter rkbt_write_cb, data: />|20|WIFILISTV2|~|#2~Libawall~|#11223344|#4|#2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5
.77/src/share
[rkbt_write_cb] offset: 0, len: 20
Data: 2F 3E 7C 32 30 7C 57 49 46 49 4C 49 53 54 56 32 7C 7E 7C 23
[25-09-16 21:24:13.205][bt_ble_rpc][operator()    239][thread2671][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|20|WIFILISTV2|~|#2~Libawall~|#11223344|#4|#2~徐佳飞-
smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share], len: [20]
[25-09-16 21:24:13.205][rk_bt_c_ad][operator()    250][thread2668][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|20|WIFILISTV2|~|#, len: 20
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|20|WIFILISTV2|~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:2(CTL_NETWORK_SCAN_APS) args:1
m_isScan= false
[25-09-16 21:24:13.207][bt_ble_rpc][bluez_call_re 395][thread2671][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [2]
[25-09-16 21:24:13.208][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server
.c:write_complete_cb() Write Complete: err 0

[25-09-16 21:24:13.208][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x13

[25-09-16 21:24:13.209][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 13                                               .

[25-09-16 21:24:13.217][rk_wifi_c_][Mock_RK_WifiS  41][thread2654][I] RK_WifiScan, ret: [true]
Run RK_WifiScan :  true
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4161mV curent:-13nA isLessDisk:0 remainSize:2354
9(less:200)MB isLow:0 infrared:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[NETWK] CB WIFI STATUS : 8
Scan complete in 508 ms, found networks:
08:9b:4b:42:a9:62       5240    -44     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
14:d8:64:8d:f4:1d       5180    -44     [WPA2-PSK-CCMP][ESS]    TP-LINK_5G_F41B
e2:c3:22:6a:b0:8f       5745    -45     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
cc:d8:43:09:96:52       5180    -62     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ceshi_5G
0e:9b:4b:42:a9:62       5240    -44     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:62       5240    -44     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:b3:d3:79       5180    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:b3:d3:79       5180    -57     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:bb:b1:54       5240    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:b3:d3:79       5180    -57     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:ab:27       5200    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b1:54       5240    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:54       5240    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:27       5200    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:ab:27       5200    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:42:a9:56       5220    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:a9:56       5220    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:a9:56       5220    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b1:68       5200    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:68       5200    -63     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
14:d8:64:8d:f4:1b       2412    -50     [WPA2-PSK-CCMP][ESS]    TP-LINK_F41B
b8:08:d7:18:81:bc       2462    -46     [WPA2-PSK-CCMP][ESS]    zzwifi
e2:c3:22:6a:b0:8b       2412    -46     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
0e:9b:4b:bb:b1:53       2462    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:42:a9:61       2412    -52     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:ab:18       5220    -68     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:bb:b1:68       5200    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:ab:18       5220    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:b3:d3:78       2437    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
12:9b:4b:42:ab:18       5220    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
cc:d8:43:09:96:51       2422    -56     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ceshi
08:9b:4b:b3:d3:7f       5765    -73     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:b3:d3:7f       5765    -73     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:bb:b1:70       5220    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b1:70       5220    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:b3:d3:7f       5765    -73     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:42:ab:12       5745    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:ab:12       5745    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:42:b4:de       5180    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:ab:12       5745    -74     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:b4:d8       5200    -74     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:b4:d8       5200    -74     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:b4:d8       5200    -74     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:42:b4:de       5180    -74     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:b4:de       5180    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b2:20       5765    -78     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:bb:b2:20       5765    -77     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b2:20       5765    -77     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:ab:0c       5805    -84     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:ab:0c       5805    -84     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:0c       5805    -84     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:aa:d0       5805    -91     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:aa:d0       5805    -89     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:aa:d0       5805    -90     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
88:2a:5e:85:40:31       5220    -83     [WPA2-PSK-CCMP][ESS]    IToIP
88:2a:5e:85:40:30       5220    -86     [ESS]   H3C-Guest
12:9b:4b:b3:d3:78       2437    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
[25-09-16 21:24:14.665][rk_wifi_c_][Mock_RK_WifiS  50][thread2672][I] get scan result success
[25-09-16 21:24:14.675][rk_wifi_c_][Mock_RK_WifiS  60][thread2672][I] RK_WifiScanResults, result: [{"ssid":"Libawall-5G","rssi":-44,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,
"save":0}
{"ssid":"TP-LINK_5G_F41B","rssi":-44,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-45,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"ceshi_5G","rssi":-62,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-44,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-44,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-57,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-57,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-63,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"TP-LINK_F41B","rssi":-50,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"zzwifi","rssi":-46,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-46,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-52,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-68,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ceshi","rssi":-56,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-73,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-73,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-73,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-74,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-74,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-74,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-74,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-74,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-78,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-77,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-77,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-84,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-84,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-84,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-91,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-89,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-90,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"IToIP","rssi":-83,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"H3C-Guest","rssi":-86,"flags":"[ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
]
[NETWK] Start Parse Wifi Scan Info! (linkSSID:
[25-09-16 21:24:14.682][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter callback, rk_state: [OPEN]
[NETWK] CB WIFI STATUS : 6
=== WPA Network List (count: 0) ===
[25-09-16 21:24:14.689][rk_wifi_c_][print_saved_i 117][thread2672][I] Total saved networks: 0
[25-09-16 21:24:14.689][rk_wifi_c_][Mock_RK_WifiG 135][thread2672][I] [exit] RK_WifiGetSavedInfo: [0]
[25-09-16 21:24:14.689][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break callback, rk_state: [OPEN]
[25-09-16 21:24:14.799][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#
]
[25-09-16 21:24:14.799][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:14.799][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-16 21:24:14.800][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:24:14.800][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 4c 49 53  .../>|33|WIFILIS

[25-09-16 21:24:14.800][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 30 7e 31 7e 75 6e 6b  TV2|OK~0|0~1~unk

[25-09-16 21:24:14.800][my_btgatt_][att_debug_cb  213][thread2671][D] att:   6e 6f 77 6e 7e 30 7e 30 7c 23                    nown~0~0|#

[25-09-16 21:24:14.801][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#]
[25-09-16 21:24:14.801][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#
], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#][size:0 ver:2]
[25-09-16 21:24:15.012][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|43|WIFILISTV2|OK~0|-44~1~TP-LINK_5G_
F41B~0~0|#]
[25-09-16 21:24:15.013][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:15.013][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [49]
[25-09-16 21:24:15.013][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:24:15.014][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 34 33 7c 57 49 46 49 4c 49 53  .../>|43|WIFILIS

[25-09-16 21:24:15.014][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 34 7e 31 7e 54  TV2|OK~0|-44~1~T

[25-09-16 21:24:15.014][my_btgatt_][att_debug_cb  213][thread2671][D] att:   50 2d 4c 49 4e 4b 5f 35 47 5f 46 34 31 42 7e 30  P-LINK_5G_F41B~0

[25-09-16 21:24:15.014][my_btgatt_][att_debug_cb  213][thread2671][D] att:   7e 30 7c 23                                      ~0|#

[25-09-16 21:24:15.014][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|43|WIFILISTV2|OK~0|-44~1~TP-LINK_5G_F41B~0~0|#]
[25-09-16 21:24:15.015][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|43|WIFILISTV2|OK~0|-44~1~TP-LINK_5G_
F41B~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|43|WIFILISTV2|OK~0|-44~1~TP-LINK_5G_F41B~0~0|#][size:0 ver:2]
[25-09-16 21:24:15.133][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-16 21:24:15.133][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-16 21:24:15.225][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-44~1~guest~0~0|#
]
[25-09-16 21:24:15.225][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:15.225][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-16 21:24:15.226][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:24:15.226][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 4c 49 53  .../>|33|WIFILIS

[25-09-16 21:24:15.226][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 34 7e 31 7e 67  TV2|OK~0|-44~1~g

[25-09-16 21:24:15.226][my_btgatt_][att_debug_cb  213][thread2671][D] att:   75 65 73 74 7e 30 7e 30 7c 23                    uest~0~0|#

[25-09-16 21:24:15.226][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|33|WIFILISTV2|OK~0|-44~1~guest~0~0|#]
[25-09-16 21:24:15.227][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-44~1~guest~0~0|#
], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-44~1~guest~0~0|#][size:0 ver:2]
[25-09-16 21:24:15.438][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-44~1~device~0~0|
#]
[25-09-16 21:24:15.439][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:15.439][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-16 21:24:15.439][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:24:15.440][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 34 7c 57 49 46 49 4c 49 53  .../>|34|WIFILIS

[25-09-16 21:24:15.440][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 34 7e 31 7e 64  TV2|OK~0|-44~1~d

[25-09-16 21:24:15.440][my_btgatt_][att_debug_cb  213][thread2671][D] att:   65 76 69 63 65 7e 30 7e 30 7c 23                 evice~0~0|#

[25-09-16 21:24:15.440][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|34|WIFILISTV2|OK~0|-44~1~device~0~0|#]
[25-09-16 21:24:15.441][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-44~1~device~0~0|
#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-44~1~device~0~0|#][size:0 ver:2]
[25-09-16 21:24:15.648][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TN
T~0~0|#]
[25-09-16 21:24:15.649][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:15.649][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-16 21:24:15.649][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:24:15.650][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 34 30 7c 57 49 46 49 4c 49 53  .../>|40|WIFILIS

[25-09-16 21:24:15.650][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 35 7e 31 7e 43  TV2|OK~0|-45~1~C

[25-09-16 21:24:15.650][my_btgatt_][att_debug_cb  213][thread2671][D] att:   68 69 6e 61 4e 65 74 2d 54 4e 54 7e 30 7e 30 7c  hinaNet-TNT~0~0|

[25-09-16 21:24:15.650][my_btgatt_][att_debug_cb  213][thread2671][D] att:   23                                               #

[25-09-16 21:24:15.650][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TNT~0~0|#]
[25-09-16 21:24:15.651][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TN
T~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TNT~0~0|#][size:0 ver:2]
[25-09-16 21:24:15.849][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-46~1~zzwifi~0~0|
#]
[25-09-16 21:24:15.851][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:15.851][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-16 21:24:15.852][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:24:15.852][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 34 7c 57 49 46 49 4c 49 53  .../>|34|WIFILIS

[25-09-16 21:24:15.852][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 36 7e 31 7e 7a  TV2|OK~0|-46~1~z

[25-09-16 21:24:15.852][my_btgatt_][att_debug_cb  213][thread2671][D] att:   7a 77 69 66 69 7e 30 7e 30 7c 23                 zwifi~0~0|#

[25-09-16 21:24:15.856][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [6], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|34|WIFILISTV2|OK~0|-46~1~zzwifi~0~0|#]
[25-09-16 21:24:15.856][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-46~1~zzwifi~0~0|
#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-46~1~zzwifi~0~0|#][size:0 ver:2]
[25-09-16 21:24:16.049][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-50~1~TP-LINK_F41
B~0~0|#]
[25-09-16 21:24:16.050][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:16.050][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-16 21:24:16.050][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_c
han_write() (chan 0xede38) ATT op 0x1b

[25-09-16 21:24:16.051][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 34 30 7c 57 49 46 49 4c 49 53  .../>|40|WIFILIS

[25-09-16 21:24:16.051][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 35 30 7e 31 7e 54  TV2|OK~0|-50~1~T

[25-09-16 21:24:16.051][my_btgatt_][att_debug_cb  213][thread2671][D] att:   50 2d 4c 49 4e 4b 5f 46 34 31 42 7e 30 7e 30 7c  P-LINK_F41B~0~0|

[25-09-16 21:24:16.051][my_btgatt_][att_debug_cb  213][thread2671][D] att:   23                                               #

[25-09-16 21:24:16.051][my_btgatt_][ble_server_no 465][thread2664][E] bt_gatt_server_send_notification, ret: [false]
[25-09-16 21:24:16.052][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [3], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|40|WIFILISTV2|OK~0|-50~1~TP-LINK_F41B~0~0|#]
[25-09-16 21:24:16.052][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-50~1~TP-LINK_F41
B~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-50~1~TP-LINK_F41B~0~0|#][size:0 ver:2]
[25-09-16 21:24:16.248][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#
]
[25-09-16 21:24:16.249][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:16.249][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-16 21:24:16.250][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [0], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#]
[25-09-16 21:24:16.250][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#
], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#][size:0 ver:2]
[25-09-16 21:24:16.449][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-58~1~Libawall-5G
~0~0|#]
[25-09-16 21:24:16.449][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:16.449][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [45]
[25-09-16 21:24:16.450][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|39|WIFILISTV2|OK~0|-58~1~Libawall-5G~0~0|#]
[25-09-16 21:24:16.450][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-58~1~Libawall-5G
~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|39|WIFILISTV2|OK~0|-58~1~Libawall-5G~0~0|#][size:0 ver:2]
[25-09-16 21:24:16.649][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-60~1~Libawall~0~
0|#]
[25-09-16 21:24:16.651][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:16.651][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-16 21:24:16.651][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|36|WIFILISTV2|OK~0|-60~1~Libawall~0~0|#]
[25-09-16 21:24:16.652][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-60~1~Libawall~0~
0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-60~1~Libawall~0~0|#][size:0 ver:2]
[25-09-16 21:24:16.848][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-62~1~ceshi_5G~0~
0|#]
[25-09-16 21:24:16.849][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:24:16.849][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-16 21:24:16.853][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [4], uuid: [dfd4416e-1
810-47f7-8248-eb8be3dc47f9], message: [/>|36|WIFILISTV2|OK~0|-62~1~ceshi_5G~0~0|#]
[25-09-16 21:24:16.853][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-62~1~ceshi_5G~0~
0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-62~1~ceshi_5G~0~0|#][size:0 ver:2]

```
