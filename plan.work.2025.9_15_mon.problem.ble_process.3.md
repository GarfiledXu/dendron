---
id: 9g6s7aqouxm9lqbwopj16ps
title: '3'
desc: ''
updated: 1758099698577
created: 1758094032559
---


## 连接上蓝牙时, 多出的fd15就是通讯的fd, fd14是监听等待连接的fd，fd10是epoll使用的fd

```bash
root@Rockchip:/$ ls -l  /proc/605/task/612/fd
lrwx------    1 root     root            64 14 -> socket:[414077]
lrwx------    1 root     root            64 13 -> anon_inode:[eventfd]
lr-x------    1 root     root            64 12 -> /dev/input/event0
lrwx------    1 root     root            64 11 -> /dev/ttyS1
lrwx------    1 root     root            64 10 -> anon_inode:[eventpoll]
lrwx------    1 root     root            64 9 -> socket:[392265]
lrwx------    1 root     root            64 8 -> socket:[392264]
lrwx------    1 root     root            64 7 -> /dev/null
lrwx------    1 root     root            64 6 -> socket:[393190]
lrwx------    1 root     root            64 5 -> socket:[393185]
lrwx------    1 root     root            64 4 -> anon_inode:[eventpoll]
lrwx------    1 root     root            64 3 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 2 -> /dev/pts/0
lrwx------    1 root     root            64 1 -> /dev/pts/0
lrwx------    1 root     root            64 0 -> /dev/pts/0
root@Rockchip:/$ ls -l  /proc/605/task/612/fd
lrwx------    1 root     root            64 15 -> socket:[473814]
lrwx------    1 root     root            64 14 -> socket:[414077]
lrwx------    1 root     root            64 13 -> anon_inode:[eventfd]
lr-x------    1 root     root            64 12 -> /dev/input/event0
lrwx------    1 root     root            64 11 -> /dev/ttyS1
lrwx------    1 root     root            64 10 -> anon_inode:[eventpoll]
lrwx------    1 root     root            64 9 -> socket:[392265]
lrwx------    1 root     root            64 8 -> socket:[392264]
lrwx------    1 root     root            64 7 -> /dev/null
lrwx------    1 root     root            64 6 -> socket:[393190]
lrwx------    1 root     root            64 5 -> socket:[393185]
lrwx------    1 root     root            64 4 -> anon_inode:[eventpoll]
lrwx------    1 root     root            64 3 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 2 -> /dev/pts/0
lrwx------    1 root     root            64 1 -> /dev/pts/0
lrwx------    1 root     root            64 0 -> /dev/pts/0

```

## 连接上并且进行读写时的strace

root@Rockchip:/oem$ strace -p 612 -tt -T -s 200 -e trace=epoll_wait,epoll_ctl,writ

```bash
oot@Rockchip:/oem$ strace -p 612 -tt -T -s 200 -e trace=epoll_wait,epoll_ctl,wr
itev,read
strace: Process 612 attached
15:30:45.121140 epoll_wait(10, [{EPOLLIN, {u32=979240, u64=979240}}], 10, -1) = 1 <2.661361>
15:30:47.783653 read(15, "\22\r\0/>|20|WIFILISTV2|~|#", 185) = 23 <0.000471>
15:30:47.787006 read(16, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33\0\0\0\21\0\0\0\3\0\0\0\f\200\0\0\0\310\\\1\200\310\372'p\311\325\16\200\312\333Z\360\36\2726\r", 68) = 68 <0.000280>
15:30:47.789181 read(16, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0\0\0\0U\223-\231\0\0\0\32\0\0\0\0XhF\232\0\0\0\33\0\0\0\0\0\0\nCST-8\n", 68) = 68 <0.000262>
15:30:47.795715 epoll_ctl(10, EPOLL_CTL_ADD, 16, {EPOLLIN|EPOLLONESHOT, {u32=983848, u64=983848}}) = 0 <0.000049>
15:30:47.802506 epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=974660, u64=974660}}) = 0 <0.000356>
15:30:47.809363 epoll_ctl(10, EPOLL_CTL_DEL, 16, NULL) = 0 <0.000295>
15:30:47.816524 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLOUT|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000170>
15:30:47.820254 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000048>
15:30:47.832194 writev(15, [{iov_base="\23", iov_len=1}], 1) = 1 <0.001438>
15:30:47.841906 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000161>
15:30:47.850519 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000492>
15:30:47.858962 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <4.509383>
15:30:52.372462 writev(15, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#", iov_len=42}], 1) = 42 <0.001178>
15:30:52.376625 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000270>
15:30:52.379323 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000312>
15:30:52.383011 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.180838>
15:30:52.566285 writev(15, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|-44~1~guest~0~0|#", iov_len=42}], 1) = 42 <0.000475>
15:30:52.572946 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000047>
15:30:52.575852 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000319>
15:30:52.578211 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.177193>
15:30:52.759835 writev(15, [{iov_base="\33\17\0/>|39|WIFILISTV2|OK~0|-46~1~Libawall-5G~0~0|#", iov_len=48}], 1) = 48 <0.001554>
15:30:52.765280 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000049>
15:30:52.767567 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000108>
15:30:52.769890 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.182264>
15:30:52.954054 writev(15, [{iov_base="\33\17\0/>|34|WIFILISTV2|OK~0|-46~1~zzwifi~0~0|#", iov_len=43}], 1) = 43 <0.001537>
15:30:52.959601 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000088>
15:30:52.961779 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000048>
15:30:52.964592 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.182644>
15:30:53.149517 writev(15, [{iov_base="\33\17\0/>|40|WIFILISTV2|OK~0|-48~1~TP-LINK_F41B~0~0|#", iov_len=49}], 1) = 49 <0.001832>
15:30:53.155160 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000048>
15:30:53.157551 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000259>
15:30:53.159832 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.182182>
15:30:53.344042 writev(15, [{iov_base="\33\17\0/>|40|WIFILISTV2|OK~0|-49~1~ChinaNet-TNT~0~0|#", iov_len=49}], 1) = 49 <0.000430>
15:30:53.350488 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000116>
15:30:53.353124 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000049>
15:30:53.355940 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.181524>
15:30:53.539534 writev(15, [{iov_base="\33\17\0/>|36|WIFILISTV2|OK~0|-50~1~Libawall~0~0|#", iov_len=45}], 1) = 45 <0.000405>
15:30:53.546357 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000310>
15:30:53.549537 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000044>
15:30:53.551672 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.181322>
15:30:53.737460 writev(15, [{iov_base="\33\17\0/>|36|WIFILISTV2|OK~0|-55~1~ceshi_5G~0~0|#", iov_len=45}], 1) = 45 <0.000353>
15:30:53.743275 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000349>
15:30:53.745611 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000343>
15:30:53.748749 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.179399>
15:30:53.931709 writev(15, [{iov_base="\33\17\0/>|34|WIFILISTV2|OK~0|-55~1~device~0~0|#", iov_len=43}], 1) = 43 <0.000649>
15:30:53.937440 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000090>
15:30:53.940395 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000278>
15:30:53.943023 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.179716>
15:30:54.125295 writev(15, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|-56~1~ceshi~0~0|#", iov_len=42}], 1) = 42 <0.000714>
15:30:54.131348 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000049>
15:30:54.134875 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000296>
15:30:54.137532 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.182985>
15:30:54.322487 writev(15, [{iov_base="\33\17\0/>|54|WIFILISTV2|OK~0|-58~1~DIRECT-54-HP M232 LaserJet~0~0|#", iov_len=63}], 1) = 63 <0.000463>
15:30:54.329313 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000079>
15:30:54.332555 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000300>
15:30:54.335861 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.188501>
15:30:54.525970 writev(15, [{iov_base="\33\17\0/>|38|WIFILISTV2|OK~0|-72~1~Dy-5G-1708~0~0|#", iov_len=47}], 1) = 47 <0.000387>
15:30:54.530539 epoll_wait(10, [{EPOLLOUT, {u32=979240, u64=979240}}], 10, -1) = 1 <0.000270>
15:30:54.533854 epoll_ctl(10, EPOLL_CTL_MOD, 15, {EPOLLIN|EPOLLRDHUP, {u32=979240, u64=979240}}) = 0 <0.000345>
15:30:54.537533 epoll_wait(10,

以上是正常读写===========

```

## 正常时fd的信息

```bash
root@Rockchip:/$ ls -l /proc/605/task/612/fd/15
lrwx------    1 root     root            64 /proc/605/task/612/fd/15 -> socket:[473814]
root@Rockchip:/$ cat /proc/605/fdinfo/15
pos:    0
flags:  02
mnt_id: 8
ino:    473814
root@Rockchip:/$ cat /proc/605/fdinfo/10
pos:    0
flags:  02000002
mnt_id: 11
ino:    80
tfd:       13 events:       19 data:            e80c8  pos:0 ino:50 sdev:a
tfd:       14 events:       19 data:            eee28  pos:0 ino:6517d sdev:7
tfd:       15 events:     2019 data:            ef128  pos:0 ino:73ad6 sdev:7
root@Rockchip:/$ cat /proc/605/fdinfo/4
pos:    0
flags:  02000002
mnt_id: 11
ino:    80
tfd:        5 events: 8000001b data:            ee020  pos:0 ino:5ffe1 sdev:7
tfd:        6 events: 8000001b data:            ee128  pos:0 ino:5ffe6 sdev:7
tfd:        3 events: 80000019 data:            edf44  pos:0 ino:50 sdev:a

```

## 忘记网络后继续连接，连接成功，传输了一半断开

```bash

[25-09-17 15:46:41.871][my_btgatt_][gatt_debug_cb 220][thread 612][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|46|WIFISETTINGV2|1~1~Libawall-5G~11223344|#2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/sharea╝
[rkbt_write_cb] offset: 0, len: 46
Data: 2F 3E 7C 34 36 7C 57 49 46 49 53 45 54 54 49 4E 47 56 32 7C 31 7E 31 7E 4C 69 62 61 77 61 6C 6C 2D 35 47 7E 31 31 32 32 33 33 34 34 7C 23
[25-09-17 15:46:41.871][bt_ble_rpc][operator()    239][thread 612][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|46|WIFISETTINGV2|1~1~Libawall-5G~11223344|#2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/sharea╝], len: [46]
[25-09-17 15:46:41.872][rk_bt_c_ad][operator()    252][thread 609][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|46|WIFISETTINGV2|1~1~Libawall-5G~11223344|#, len: 46
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|46|WIFISETTINGV2|1~1~Libawall-5G~11223344|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:1|1|Libawall-5G|11223344|1
[25-09-17 15:46:41.877][bt_ble_rpc][bluez_call_re 395][thread 612][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [5]
[25-09-17 15:46:41.878][my_btgatt_][gatt_debug_cb 220][thread 612][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-17 15:46:41.878][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0750) ATT op
0x13

[25-09-17 15:46:41.879][my_btgatt_][att_debug_cb  213][thread 612][D] att: < 13                                               .

[25-09-17 15:46:41.917][rk_wifi_c_][Mock_RK_WifiC  66][thread 596][I] [===>] RK_WifiConnect
[25-09-17 15:46:41.917][rk_wifi_c_][Mock_RK_WifiC  70][thread 596][I] the matched wpa_network_config_list count: [0], with ssid: [Libawall-5G], pask: [11223344]
[25-09-17 15:46:42.083][rk_wifi_c_][Mock_RK_WifiC  90][thread 596][I] [exit] RK_WifiConnect
[25-09-17 15:46:42.193][wifi_mng_w][operator()   1073][thread 613][I] [on loop] start_polling_status_rk, enter callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-17 15:46:42.193][wifi_mng_w][operator()   1075][thread 613][I] [on loop] start_polling_status_rk, break callback, rk_state: [IDLE]
[25-09-17 15:46:42.550][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:46:42.551][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4162mV curent:-44nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0 i
sStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[25-09-17 15:46:44.553][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:46:44.554][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-17 15:46:46.220][wifi_mng_w][operator()   1073][thread 613][I] [on loop] start_polling_status_rk, enter callback, rk_state: [CONNECTED]
[NETWK] CB WIFI STATUS : 4
=== WPA Network List (count: 1) ===
Index: 0
  ID:     0
  SSID:   Libawall-5G
  BSSID:  any
  FLAGS:        [CURRENT]
-----------------------------------
[25-09-17 15:46:46.227][rk_wifi_c_][print_saved_i 117][thread 613][I] Total saved networks: 1
[25-09-17 15:46:46.227][rk_wifi_c_][print_saved_i 127][thread 613][I]   [#0] SSID: 'Libawall-5G', BSSID: 'any', State: '[CURRENT]'
[25-09-17 15:46:46.228][rk_wifi_c_][Mock_RK_WifiG 135][thread 613][I] [exit] RK_WifiGetSavedInfo: [0]
[NETWK] **********************
[NETWK] updateSaveApInfo {id=0 ssid=Libawall-5G bssid=any state:[CURRENT]}
[NETWK] **********************
[NETWK] CONNETED WIFI SSID: (initAutoConnType:0) ssid:Libawall-5G mac:94:ba:06:91:04:0b ip:192.168.49.26
[NETWK] CONNETED pthis->cfgType: 1 cfgPreNetRoute: 1
[NETWK] WIFI SET UDHCP (type:2) metric:100
[glSetUdhcpcOnMetric] IP output: "192.168.49.26"
[glSetUdhcpcOnMetric] Default route for "wlan0" via "192.168.48.1" set to metric 100
[25-09-17 15:46:46.420][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|30|WIFISETTINGV2|OK~0|1~connect|#]
[25-09-17 15:46:46.421][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:46.421][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [36]
[25-09-17 15:46:46.421][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0750) ATT op
0x1b

[25-09-17 15:46:46.422][my_btgatt_][att_debug_cb  213][thread 612][D] att: < 1b 0f 00 2f 3e 7c 33 30 7c 57 49 46 49 53 45 54  .../>|30|WIFISET

[25-09-17 15:46:46.422][my_btgatt_][att_debug_cb  213][thread 612][D] att:   54 49 4e 47 56 32 7c 4f 4b 7e 30 7c 31 7e 63 6f  TINGV2|OK~0|1~co

[25-09-17 15:46:46.422][my_btgatt_][att_debug_cb  213][thread 612][D] att:   6e 6e 65 63 74 7c 23                             nnect|#

[25-09-17 15:46:46.422][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|30|WIFISETTINGV2|OK~0|1~connect|#]
[25-09-17 15:46:46.422][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|30|WIFISETTINGV2|OK~0|1~connect|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|30|WIFISETTINGV2|OK~0|1~connect|#][size:0 ver:2]
[25-09-17 15:46:46.490][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xf0750) ATT receive
d: 23

[25-09-17 15:46:46.490][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xf0750) ATT PDU rec
eived: 0x12

[25-09-17 15:46:46.490][my_btgatt_][gatt_debug_cb 220][thread 612][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|20|WIFILISTV2|~|#1~1~Libawall-5G~11223344|#2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share╔╚
[rkbt_write_cb] offset: 0, len: 20
Data: 2F 3E 7C 32 30 7C 57 49 46 49 4C 49 53 54 56 32 7C 7E 7C 23
[25-09-17 15:46:46.491][bt_ble_rpc][operator()    239][thread 612][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|20|WIFILISTV2|~|#1~1~Libawall-5G~11223344|#2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share╔╚], len: [20]
[25-09-17 15:46:46.491][rk_bt_c_ad][operator()    252][thread 609][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|20|WIFILISTV2|~|#, len: 20
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|20|WIFILISTV2|~|# )
[25-09-17 15:46:46.493][bt_ble_rpc][bluez_call_re 395][thread 612][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [1]
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:2(CTL_NETWORK_SCAN_APS) args:1
m_isScan= false
[25-09-17 15:46:46.494][my_btgatt_][gatt_debug_cb 220][thread 612][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-17 15:46:46.495][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0750) ATT op
0x13

[25-09-17 15:46:46.496][my_btgatt_][att_debug_cb  213][thread 612][D] att: < 13                                               .

[25-09-17 15:46:46.503][rk_wifi_c_][Mock_RK_WifiS  41][thread 596][I] RK_WifiScan, ret: [true]
Run RK_WifiScan :  true
[25-09-17 15:46:46.554][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:46:46.554][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-17 15:46:47.307][wifi_mng_w][operator()   1075][thread 613][I] [on loop] start_polling_status_rk, break callback, rk_state: [CONNECTED]
[NETWK] Update Network Status (4(NSTA_PING) value:0)
[NETWK] Update Network Status (2(NSTA_WIFI) value:1)
[NETWK] CB WIFI STATUS : 8
[25-09-17 15:46:48.554][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:46:48.555][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
Scan complete in 508 ms, found networks:
14:d8:64:8d:f4:1d       5180    -42     [WPA2-PSK-CCMP][ESS]    TP-LINK_5G_F41B
e2:c3:22:6a:b0:8f       5745    -52     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
08:9b:4b:42:a9:62       5240    -49     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:a9:62       5240    -50     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:62       5240    -49     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:bb:b1:70       5220    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:b3:d3:79       5180    -59     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:bb:b1:70       5220    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:b3:d3:79       5180    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:70       5220    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:27       5200    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:ab:27       5200    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:a9:56       5220    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b1:54       5240    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:ab:27       5200    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:bb:b1:54       5240    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:a9:56       5220    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:bb:b1:54       5240    -65     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:a9:56       5220    -66     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
14:d8:64:8d:f4:1b       2412    -45     [WPA2-PSK-CCMP][ESS]    TP-LINK_F41B
b8:08:d7:18:81:bc       2437    -49     [WPA2-PSK-CCMP][ESS]    zzwifi
e2:c3:22:6a:b0:8b       2412    -45     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
0e:9b:4b:42:a9:61       2412    -53     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:a9:61       2412    -52     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
12:9b:4b:42:a9:61       2412    -54     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:ab:18       5220    -68     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:b3:d3:7f       5240    -68     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:18       5220    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:68       5200    -68     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:b3:d3:7f       5240    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
cc:d8:43:09:96:51       2422    -57     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ceshi
12:9b:4b:42:a9:55       2437    -56     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:a9:55       2437    -56     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
08:9b:4b:42:ab:12       5745    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b1:68       5200    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:ab:12       5745    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:42:ab:18       5220    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:ab:12       5745    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:bb:b1:6f       2462    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:42:b4:d8       5200    -74     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:b4:d8       5200    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
56:f1:5f:8b:74:87       5180    -70     [WPA2-PSK-CCMP][ESS]    Dy-5G-1708
08:9b:4b:b3:d3:78       2437    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
88:2a:5e:85:40:31       5220    -77     [WPA2-PSK-CCMP][ESS]    IToIP
12:9b:4b:bb:b1:53       2462    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:b4:de       5180    -77     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:bb:b2:1f       2412    -66     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
0e:9b:4b:bb:b2:1f       2412    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:bb:b2:1f       2412    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b2:20       5765    -79     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b2:20       5765    -79     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:bb:b1:53       2462    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:53       2462    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
08:9b:4b:bb:b1:6f       2462    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
08:9b:4b:42:ab:0c       5805    -81     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:0c       5805    -81     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:42:b4:de       5180    -79     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
[25-09-17 15:46:48.831][rk_wifi_c_][Mock_RK_WifiS  50][thread 613][I] get scan result success
[25-09-17 15:46:48.842][rk_wifi_c_][Mock_RK_WifiS  60][thread 613][I] RK_WifiScanResults, result: [{"ssid":"TP-LINK_5G_F41B","rssi":-42,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-52,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-49,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-50,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-49,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-59,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-65,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-66,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"TP-LINK_F41B","rssi":-45,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"zzwifi","rssi":-49,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-45,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-53,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-52,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-54,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-68,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-68,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-68,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ceshi","rssi":-57,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-56,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-56,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-74,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Dy-5G-1708","rssi":-70,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"IToIP","rssi":-77,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-77,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-66,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-79,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-79,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-81,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-81,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-79,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
]
[NETWK] Start Parse Wifi Scan Info! (linkSSID:Libawall-5G
[25-09-17 15:46:48.869][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#]
[25-09-17 15:46:48.870][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:48.870][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-17 15:46:48.871][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0750) ATT op
0x1b

[25-09-17 15:46:48.871][my_btgatt_][att_debug_cb  213][thread 612][D] att: < 1b 0f 00 2f 3e 7c 33 33 7c 57 49 46 49 4c 49 53  .../>|33|WIFILIS

[25-09-17 15:46:48.871][my_btgatt_][att_debug_cb  213][thread 612][D] att:   54 56 32 7c 4f 4b 7e 30 7c 30 7e 31 7e 75 6e 6b  TV2|OK~0|0~1~unk

[25-09-17 15:46:48.871][my_btgatt_][att_debug_cb  213][thread 612][D] att:   6e 6f 77 6e 7e 30 7e 30 7c 23                    nown~0~0|#

[25-09-17 15:46:48.872][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#]
[25-09-17 15:46:48.872][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#][size:0 ver:2]
[25-09-17 15:46:49.082][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TNT~0~0|#]
[25-09-17 15:46:49.083][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:49.083][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-17 15:46:49.084][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0750) ATT op
0x1b

[25-09-17 15:46:49.084][my_btgatt_][att_debug_cb  213][thread 612][D] att: < 1b 0f 00 2f 3e 7c 34 30 7c 57 49 46 49 4c 49 53  .../>|40|WIFILIS

[25-09-17 15:46:49.084][my_btgatt_][att_debug_cb  213][thread 612][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 35 7e 31 7e 43  TV2|OK~0|-45~1~C

[25-09-17 15:46:49.084][my_btgatt_][att_debug_cb  213][thread 612][D] att:   68 69 6e 61 4e 65 74 2d 54 4e 54 7e 30 7e 30 7c  hinaNet-TNT~0~0|

[25-09-17 15:46:49.084][my_btgatt_][att_debug_cb  213][thread 612][D] att:   23                                               #

[25-09-17 15:46:49.085][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TNT~0~0|#]
[25-09-17 15:46:49.085][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TNT~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TNT~0~0|#][size:0 ver:2]
[25-09-17 15:46:49.296][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-45~1~TP-LINK_F41B~0~0|#]
[25-09-17 15:46:49.297][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:49.297][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-17 15:46:49.297][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0750) ATT op
0x1b

[25-09-17 15:46:49.298][my_btgatt_][att_debug_cb  213][thread 612][D] att: < 1b 0f 00 2f 3e 7c 34 30 7c 57 49 46 49 4c 49 53  .../>|40|WIFILIS

[25-09-17 15:46:49.298][my_btgatt_][att_debug_cb  213][thread 612][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 35 7e 31 7e 54  TV2|OK~0|-45~1~T

[25-09-17 15:46:49.298][my_btgatt_][att_debug_cb  213][thread 612][D] att:   50 2d 4c 49 4e 4b 5f 46 34 31 42 7e 30 7e 30 7c  P-LINK_F41B~0~0|

[25-09-17 15:46:49.298][my_btgatt_][att_debug_cb  213][thread 612][D] att:   23                                               #

[25-09-17 15:46:49.298][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|40|WIFILISTV2|OK~0|-45~1~TP-LINK_F41B~0~0|#]
[25-09-17 15:46:49.299][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-45~1~TP-LINK_F41B~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-45~1~TP-LINK_F41B~0~0|#][size:0 ver:2]
[25-09-17 15:46:49.509][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-49~1~Libawall-5G~1~1|#]
[25-09-17 15:46:49.509][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:49.510][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [45]
[25-09-17 15:46:49.510][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0750) ATT op
0x1b

[25-09-17 15:46:49.510][my_btgatt_][att_debug_cb  213][thread 612][D] att: < 1b 0f 00 2f 3e 7c 33 39 7c 57 49 46 49 4c 49 53  .../>|39|WIFILIS

[25-09-17 15:46:49.510][my_btgatt_][att_debug_cb  213][thread 612][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34 39 7e 31 7e 4c  TV2|OK~0|-49~1~L

[25-09-17 15:46:49.510][my_btgatt_][att_debug_cb  213][thread 612][D] att:   69 62 61 77 61 6c 6c 2d 35 47 7e 31 7e 31 7c 23  ibawall-5G~1~1|#

[25-09-17 15:46:49.511][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [2], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|39|WIFILISTV2|OK~0|-49~1~Libawall-5G~1~1|#]
[25-09-17 15:46:49.511][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-49~1~Libawall-5G~1~1|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|39|WIFILISTV2|OK~0|-49~1~Libawall-5G~1~1|#][size:0 ver:2]
[25-09-17 15:46:49.720][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-49~1~device~0~0|#]
[25-09-17 15:46:49.721][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:49.721][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-17 15:46:49.721][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|34|WIFILISTV2|OK~0|-49~1~device~0~0|#]
[25-09-17 15:46:49.721][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-49~1~device~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-49~1~device~0~0|#][size:0 ver:2]
[25-09-17 15:46:49.920][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-49~1~zzwifi~0~0|#]
[25-09-17 15:46:49.921][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:49.921][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-17 15:46:49.921][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|34|WIFILISTV2|OK~0|-49~1~zzwifi~0~0|#]
[25-09-17 15:46:49.922][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-49~1~zzwifi~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-49~1~zzwifi~0~0|#][size:0 ver:2]
[25-09-17 15:46:50.120][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-50~1~guest~0~0|#]
[25-09-17 15:46:50.120][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:50.120][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-17 15:46:50.122][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|-50~1~guest~0~0|#]
[25-09-17 15:46:50.122][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-50~1~guest~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-50~1~guest~0~0|#][size:0 ver:2]
[25-09-17 15:46:50.320][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#]
[25-09-17 15:46:50.320][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:50.320][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-17 15:46:50.321][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#]
[25-09-17 15:46:50.321][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#][size:0 ver:2]
[25-09-17 15:46:50.520][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-57~1~ceshi~0~0|#]
[25-09-17 15:46:50.524][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:50.524][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-17 15:46:50.526][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [5], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|-57~1~ceshi~0~0|#]
[25-09-17 15:46:50.526][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-57~1~ceshi~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-57~1~ceshi~0~0|#][size:0 ver:2]
[25-09-17 15:46:50.555][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:46:50.556][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:0] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4167mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0 i
sStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[25-09-17 15:46:50.720][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|38|WIFILISTV2|OK~0|-70~1~Dy-5G-1708~0~0|#]
[25-09-17 15:46:50.720][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:46:50.721][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [44]
[25-09-17 15:46:50.721][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|38|WIFILISTV2|OK~0|-70~1~Dy-5G-1708~0~0|#]
[25-09-17 15:46:50.721][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|38|WIFILISTV2|OK~0|-70~1~Dy-5G-1708~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|38|WIFILISTV2|OK~0|-70~1~Dy-5G-1708~0~0|#][size:0 ver:2]
[25-09-17 15:46:52.557][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:46:52.558][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-17 15:46:54.557][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:46:54.558][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "221",
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
    "time": 1758095215,
    "version": "1.5.1"
}

[25-09-17 15:46:56.558][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:46:56.558][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-17 15:46:58.559][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:46:58.560][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:0] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4167mV curent:-22nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0 i
sStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[25-09-17 15:47:00.561][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:47:00.561][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-17 15:47:02.561][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:47:02.562][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-17 15:47:04.562][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:47:04.563][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-17 15:47:05.182][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xf0750) ATT receive
d: 23

[25-09-17 15:47:05.185][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xf0750) ATT PDU rec
eived: 0x12

[25-09-17 15:47:05.185][my_btgatt_][gatt_debug_cb 220][thread 612][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|20|WIFILISTV2|~|#1~1~Libawall-5G~11223344|#2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share)
[rkbt_write_cb] offset: 0, len: 20
Data: 2F 3E 7C 32 30 7C 57 49 46 49 4C 49 53 54 56 32 7C 7E 7C 23
[25-09-17 15:47:05.189][bt_ble_rpc][operator()    239][thread 612][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|20|WIFILISTV2|~|#1~1~Libawall-5G~11223344|#2~徐佳飞-smart-奥特之章重生~-1~1|#me/xjf112
7/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share)], len: [20]
[25-09-17 15:47:05.191][rk_bt_c_ad][operator()    252][thread 609][I] [BtBleRpcClient] Receive data, uuid: 00009999-0000-1000-8000-00805F9B34FB, data: />|20|WIFILISTV2|~|#, len: 20
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|20|WIFILISTV2|~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:2(CTL_NETWORK_SCAN_APS) args:1
m_isScan= false
[25-09-17 15:47:05.196][bt_ble_rpc][bluez_call_re 395][thread 612][I] bluez_call_receive_sync_ timeout for cmd: [ble_client_receive], ret code: [0], cost time ms: [6]
[25-09-17 15:47:05.202][my_btgatt_][gatt_debug_cb 220][thread 612][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-17 15:47:05.210][rk_wifi_c_][Mock_RK_WifiS  41][thread 596][I] RK_WifiScan, ret: [true]
Run RK_WifiScan :  true
[NETWK] CB WIFI STATUS : 8
Scan complete in 508 ms, found networks:
14:d8:64:8d:f4:1d       5180    -42     [WPA2-PSK-CCMP][ESS]    TP-LINK_5G_F41B
e2:c3:22:6a:b0:8f       5745    -52     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
08:9b:4b:42:a9:62       5240    -47     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:a9:62       5240    -50     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:a9:62       5240    -49     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:bb:b1:70       5220    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:b3:d3:79       5180    -59     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:bb:b1:70       5220    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:b3:d3:79       5180    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:70       5220    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:27       5200    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:ab:27       5200    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:a9:56       5220    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b1:54       5240    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:ab:27       5200    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:bb:b1:54       5240    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
12:9b:4b:42:a9:56       5220    -64     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:bb:b1:54       5240    -65     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:a9:56       5220    -66     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
14:d8:64:8d:f4:1b       2412    -45     [WPA2-PSK-CCMP][ESS]    TP-LINK_F41B
b8:08:d7:18:81:bc       2437    -49     [WPA2-PSK-CCMP][ESS]    zzwifi
e2:c3:22:6a:b0:8b       2412    -45     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ChinaNet-TNT
0e:9b:4b:42:a9:61       2412    -53     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:a9:61       2412    -52     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
12:9b:4b:42:a9:61       2412    -54     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:ab:18       5220    -68     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:b3:d3:7f       5240    -68     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:18       5220    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:68       5200    -68     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:b3:d3:7f       5240    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
cc:d8:43:09:96:51       2422    -57     [WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]    ceshi
12:9b:4b:42:a9:55       2437    -56     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:a9:55       2437    -56     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
08:9b:4b:42:ab:12       5745    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b1:68       5200    -70     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:42:ab:12       5745    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
12:9b:4b:42:ab:18       5220    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
0e:9b:4b:42:ab:12       5745    -72     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:bb:b1:6f       2462    -58     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:42:b4:d8       5200    -74     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:42:b4:d8       5200    -75     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
56:f1:5f:8b:74:87       5180    -70     [WPA2-PSK-CCMP][ESS]    Dy-5G-1708
08:9b:4b:b3:d3:78       2437    -61     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
88:2a:5e:85:40:31       5220    -77     [WPA2-PSK-CCMP][ESS]    IToIP
12:9b:4b:bb:b1:53       2462    -60     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:42:b4:de       5180    -77     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
08:9b:4b:bb:b2:1f       2412    -66     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
0e:9b:4b:bb:b2:1f       2412    -71     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
12:9b:4b:bb:b2:1f       2412    -69     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      device
08:9b:4b:bb:b2:20       5765    -79     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:bb:b2:20       5765    -79     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:bb:b1:53       2462    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
08:9b:4b:bb:b1:53       2462    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
08:9b:4b:bb:b1:6f       2462    -62     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall
08:9b:4b:42:ab:0c       5805    -81     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      Libawall-5G
0e:9b:4b:42:ab:0c       5805    -81     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
0e:9b:4b:42:b4:de       5180    -79     [WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]      guest
[25-09-17 15:47:06.494][rk_wifi_c_][Mock_RK_WifiS  50][thread 613][I] get scan result success
[25-09-17 15:47:06.509][rk_wifi_c_][Mock_RK_WifiS  60][thread 613][I] RK_WifiScanResults, result: [{"ssid":"TP-LINK_5G_F41B","rssi":-42,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-52,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-47,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-50,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-49,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-59,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-64,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-65,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-66,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"TP-LINK_F41B","rssi":-45,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"zzwifi","rssi":-49,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ChinaNet-TNT","rssi":-45,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-53,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-52,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-54,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-68,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-68,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-68,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"ceshi","rssi":-57,"flags":"[WPA-PSK-CCMP+TKIP][WPA2-PSK-CCMP+TKIP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-56,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-56,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-70,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-72,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-58,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-74,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-75,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Dy-5G-1708","rssi":-70,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-61,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"IToIP","rssi":-77,"flags":"[WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-60,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-77,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-66,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-71,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"device","rssi":-69,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-79,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-79,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall","rssi":-62,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"Libawall-5G","rssi":-81,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-81,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
{"ssid":"guest","rssi":-79,"flags":"[WPA-PSK-CCMP][WPA2-PSK-CCMP][ESS]","currlink":0,"save":0}
]
[NETWK] Start Parse Wifi Scan Info! (linkSSID:Libawall-5G
[25-09-17 15:47:06.520][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#]
[25-09-17 15:47:06.524][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:47:06.524][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-17 15:47:06.525][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [4], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#]
[25-09-17 15:47:06.526][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#][size:0 ver:2]
[25-09-17 15:47:06.562][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:47:06.563][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 1 sig:0] isDeviceLock:0 mqttStatus:3 isCharge:1 energy:100 capcity:4167mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared:0 i
sStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[25-09-17 15:47:06.720][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TNT~0~0|#]
[25-09-17 15:47:06.721][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:47:06.721][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-17 15:47:06.721][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TNT~0~0|#]
[25-09-17 15:47:06.722][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TNT~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-45~1~ChinaNet-TNT~0~0|#][size:0 ver:2]
[25-09-17 15:47:06.920][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-45~1~TP-LINK_F41B~0~0|#]
[25-09-17 15:47:06.921][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:47:06.921][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-17 15:47:06.922][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|40|WIFILISTV2|OK~0|-45~1~TP-LINK_F41B~0~0|#]
[25-09-17 15:47:06.922][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-45~1~TP-LINK_F41B~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-45~1~TP-LINK_F41B~0~0|#][size:0 ver:2]
[25-09-17 15:47:07.120][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-47~1~Libawall-5G~1~1|#]
[25-09-17 15:47:07.121][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:47:07.121][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [45]
[25-09-17 15:47:07.122][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|39|WIFILISTV2|OK~0|-47~1~Libawall-5G~1~1|#]
[25-09-17 15:47:07.122][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|39|WIFILISTV2|OK~0|-47~1~Libawall-5G~1~1|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|39|WIFILISTV2|OK~0|-47~1~Libawall-5G~1~1|#][size:0 ver:2]
[25-09-17 15:47:07.320][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-49~1~device~0~0|#]
[25-09-17 15:47:07.320][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:47:07.321][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-17 15:47:07.321][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|34|WIFILISTV2|OK~0|-49~1~device~0~0|#]
[25-09-17 15:47:07.321][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-49~1~device~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-49~1~device~0~0|#][size:0 ver:2]
[25-09-17 15:47:07.526][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-49~1~zzwifi~0~0|#]
[25-09-17 15:47:07.526][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:47:07.526][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-17 15:47:07.527][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|34|WIFILISTV2|OK~0|-49~1~zzwifi~0~0|#]
[25-09-17 15:47:07.527][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-49~1~zzwifi~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-49~1~zzwifi~0~0|#][size:0 ver:2]
[25-09-17 15:47:07.720][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-50~1~guest~0~0|#]
[25-09-17 15:47:07.720][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:47:07.720][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-17 15:47:07.722][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|-50~1~guest~0~0|#]
[25-09-17 15:47:07.722][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-50~1~guest~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-50~1~guest~0~0|#][size:0 ver:2]
[25-09-17 15:47:07.920][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#]
[25-09-17 15:47:07.921][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:47:07.921][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-17 15:47:07.922][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#]
[25-09-17 15:47:07.922][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#][size:0 ver:2]
[25-09-17 15:47:08.120][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-57~1~ceshi~0~0|#]
[25-09-17 15:47:08.120][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:47:08.121][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [39]
[25-09-17 15:47:08.122][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|33|WIFILISTV2|OK~0|-57~1~ceshi~0~0|#]
[25-09-17 15:47:08.122][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|33|WIFILISTV2|OK~0|-57~1~ceshi~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|33|WIFILISTV2|OK~0|-57~1~ceshi~0~0|#][size:0 ver:2]
[25-09-17 15:47:08.320][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|38|WIFILISTV2|OK~0|-70~1~Dy-5G-1708~0~0|#]
[25-09-17 15:47:08.321][my_btgatt_][ble_server_no 429][thread 605][D] enter ble_server_notify
[25-09-17 15:47:08.321][my_btgatt_][ble_server_no 460][thread 605][D] send gatt notification, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [44]
[25-09-17 15:47:08.322][bt_ble_rpc][notify_sync   175][thread 596][W] [BtBleRpcClient] [notify_sync] timeout for cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], messa
ge: [/>|38|WIFILISTV2|OK~0|-70~1~Dy-5G-1708~0~0|#]
[25-09-17 15:47:08.322][rk_bt_c_ad][Mock_RK_BleWr 291][thread 596][D] [end] Mock_RK_BleWriteData, uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], data: [/>|38|WIFILISTV2|OK~0|-70~1~Dy-5G-1708~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|38|WIFILISTV2|OK~0|-70~1~Dy-5G-1708~0~0|#][size:0 ver:2]
[25-09-17 15:47:08.563][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:47:08.564][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0

[25-09-17 15:47:10.564][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:47:10.564][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "222",

[25-09-17 15:47:32.582][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-17 15:47:34.583][bt_ble_rpc][operator()     92][thread 605][D] [BtBleRpcServer] receive BLE_CLIENT_HEARTBEAT
[25-09-17 15:47:34.584][bt_ble_rpc][operator()    212][thread 609][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HEARTBEAT, rsp=0
[25-09-17 15:47:35.124][my_btgatt_][att_debug_cb  213][thread 612][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:disconnect_cb() Channel 0xf0750 disconnect
ed: Connection reset by peer

[25-09-17 15:47:35.131][my_btgatt_][att_disconnec 203][thread 612][D] Device disconnected: Connection reset by peer, 2799584432

[25-09-17 15:47:35.136][my_btgatt_][ble_server_ru 822][thread 612][D] [exit] mainloop run
[NETWK] GET BT STATUS : 2 [DISCONNECTED] [name:YDAS250701000005 addr:94:BA:06:91:04:0C]
[25-09-17 15:47:35.138][rk_bt_c_ad][operator()    248][thread 609][W] [BtBleRpcClient] State changed, addr: 94:BA:06:91:04:0C, dev: YDAS250701000005, state: 2
[NETWK] Update Network Status (3(NSTA_BT) value:0)
[STAMP] USER LOGOUT AS !
[25-09-17 15:47:35.143][bt_ble_rpc][bluez_call_st 424][thread 612][I] bluez_call_state_sync_ timeout for cmd: [ble_state], ret code: [0], cost time ms: [6]
[25-09-17 15:47:35.144][my_btgatt_][ble_server_ru 832][thread 612][D] delete mainloop_notify_fd_: [13]
[25-09-17 15:47:35.144][my_btgatt_][stop          343][thread 612][D] [===>] L2CapAttChannelListener::stop
[25-09-17 15:47:35.147][my_btgatt_][stop          347][thread 612][D] mainloop_remove_fd and close listen_sock_: [14]
[25-09-17 15:47:35.147][my_btgatt_][stop          354][thread 612][D] close accepted_sock_: [15]
[25-09-17 15:47:35.148][my_btgatt_][stop          360][thread 612][D] [exit] L2CapAttChannelListener::stop
[25-09-17 15:47:35.148][my_btgatt_][ble_server_ru 840][thread 612][D] destroy the core_pure_c_server_handle_ handle for [att] [gap] [gatt] resouce
[25-09-17 15:47:35.149][my_btgatt_][ble_server_ru 844][thread 612][D] [exit] server loop, count: [3]
[25-09-17 15:47:35.149][my_btgatt_][ble_server_ru 709][thread 612][D] [===>] server loop, count: [4]
[25-09-17 15:47:35.150][my_btgatt_][ble_server_ru 720][thread 612][D] set_advertising_data, adverting_device_name: [YDAS250701000005], advertisng_service_uuid: [0000180A-0000-1000-8000-00805F9B34FB]
[25-09-17 15:47:35.162][my_btgatt_][set_advertisi 150][thread 612][D] after bt_string_to_uuid, uuid type: 16
[25-09-17 15:47:35.180][my_btgatt_][ble_server_ru 740][thread 612][D] Attempt 1: set_advertising_data result: 0
[25-09-17 15:47:35.180][my_btgatt_][ble_server_ru 753][thread 612][D] set_device_name: [YDAS250701000005]
[25-09-17 15:47:35.181][my_btgatt_][ble_server_ru 756][thread 612][D] mainloop_init
[25-09-17 15:47:35.182][my_btgatt_][ble_server_ru 761][thread 612][D] create mainloop_notify_fd_: [13]
[25-09-17 15:47:35.187][my_btgatt_][ble_server_ru 776][thread 612][D] success, add mainloop_notify_fd_ to mainloop_fd, re: [0]
[25-09-17 15:47:35.187][my_btgatt_][start_listen_ 269][thread 612][D] [===>] start_listen_and_accept
[25-09-17 15:47:35.188][my_btgatt_][create_socket  55][thread 612][D] [===>] create_socket_
[25-09-17 15:47:35.188][my_btgatt_][create_socket  66][thread 612][D] create listen_sock_: [14]
[25-09-17 15:47:35.189][my_btgatt_][create_socket  79][thread 612][D] bind listen_sock_: [14]
[25-09-17 15:47:35.189][my_btgatt_][create_socket  88][thread 612][D] setsockopt listen_sock_: [14]
[EVENT] Module Recv Commands (send: StampController cmd:3))
"[EVENT] Handle Finger Controller {md:FingerModule cmd:11(CTL_FINGER_CANCEL) arg:}"
[U**I] onHandleMainUICommand :  3
[25-09-17 15:47:35.194][my_btgatt_][create_socket  90][thread 612][D] start listen_sock_: [14]
[25-09-17 15:47:35.195][my_btgatt_][create_socket  97][thread 612][D] [exit] create_socket_
[25-09-17 15:47:35.195][my_btgatt_][start_listen_ 319][thread 612][D] mainloop_add_fd listen_sock_: [14]
[25-09-17 15:47:35.200][my_btgatt_][start_listen_ 332][thread 612][D] [exit] start_listen_and_accept
[25-09-17 15:47:35.201][my_btgatt_][ble_server_ru 820][thread 612][D] [===>] mainloop run
killall: rk_mpi_ao_test: no process killed
[SYS] Audio Cmd End:  "rk_mpi_ao_test -i /usr/audio/10024.wav --sound_card_name=hw:0,0 --device_ch=2 --device_rate=48000 --input_rate=48001 --input_ch=2 --set_volume=50 &"
cmd parse result:

```

## 上面错误发生时，进入starce，还是读成功的

```bash
root@Rockchip:/oem$ strace -p 612 -tt -T -s 200 -e trace=epoll_wait,epoll_ctl,wr
itev,read
strace: Process 612 attached
15:46:59.473126 epoll_wait(10, [{EPOLLIN, {u32=979240, u64=979240}}], 10, -1) = 1 <5.707519>
15:47:05.181803 read(15, "\22\r\0/>|20|WIFILISTV2|~|#", 185) = 23 <0.000062>
15:47:05.182683 read(16, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33\0\0\0\21\0\0\0\3\0\0\0\f\200\0\0\0\310\\\1\200\310\372'p\311\325\16\200\312\333Z\360\36\2726\r", 68) = 68 <0.000066>
15:47:05.183647 read(16, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0\0\0\0U\223-\231\0\0\0\32\0\0\0\0XhF\232\0\0\0\33\0\0\0\0\0\0\nCST-8\n", 68) = 68 <0.000032>
15:47:05.186679 epoll_ctl(10, EPOLL_CTL_ADD, 16, {EPOLLIN|EPOLLONESHOT, {u32=984776, u64=984776}}) = 0 <0.000047>
15:47:05.193515 epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=974660, u64=974660}}) = 0 <0.000074>
15:47:05.197324 epoll_ctl(10, EPOLL_CTL_DEL, 16, NULL) = 0 <0.000062>
15:47:05.203431 epoll_wait(10, [{EPOLLERR|EPOLLHUP, {u32=979240, u64=979240}}], 10, -1) = 1 <29.916374>
15:47:35.125080 read(16, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33\0\0\0\21\0\0\0\3\0\0\0\f\200\0\0\0\310\\\1\200\310\372'p\311\325\16\200\312\333Z\360\36\2726\r", 68) = 68 <0.001045>
15:47:35.126923 read(16, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0\0\0\0U\223-\231\0\0\0\32\0\0\0\0XhF\232\0\0\0\33\0\0\0\0\0\0\nCST-8\n", 68) = 68 <0.000306>
15:47:35.129862 epoll_ctl(10, EPOLL_CTL_DEL, 15, NULL) = 0 <0.000322>
15:47:35.132366 epoll_ctl(10, EPOLL_CTL_DEL, 13, NULL) = 0 <0.000051>
15:47:35.134299 epoll_ctl(10, EPOLL_CTL_DEL, 14, NULL) = 0 <0.000305>
15:47:35.140808 epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=974660, u64=974660}}) = 0 <0.000120>
15:47:35.151130 writev(10, [{iov_base="\1", iov_len=1}, {iov_base="\n \1", iov_len=3}, {iov_base="\0", iov_len=1}], 3) = 5 <0.000284>
15:47:35.155651 read(10, "\4\16\4\2\n \0", 260) = 7 <0.000051>
15:47:35.158088 writev(10, [{iov_base="\1", iov_len=1}, {iov_base="\6 \17", iov_len=3}, {iov_base="\20\0\25\0\0\0\0\0\0\0\0\0\0\7\0", iov_len=15}], 3) = 19 <0.000444>
15:47:35.162527 writev(10, [{iov_base="\1", iov_len=1}, {iov_base="\10  ", iov_len=3}, {iov_base="\25\2\1\6\21\7\3734\233_\200\0\0\200\0\20\0\0\n\30\0\0\0\0\0\0\0\0\0\0\0\0", iov_len=32}], 3) = 36 <0.000373>
15:47:35.166676 writev(10, [{iov_base="\1", iov_len=1}, {iov_base="\t  ", iov_len=3}, {iov_base="\22\21\tYDAS250701000005\0\0\0\0\0\0\0\0\0\0\0\0\0", iov_len=32}], 3) = 36 <0.000308>
15:47:35.170895 read(10, "\4\16\4\2\t \0", 260) = 7 <0.000052>
15:47:35.173500 writev(10, [{iov_base="\1", iov_len=1}, {iov_base="\n \1", iov_len=3}, {iov_base="\1", iov_len=1}], 3) = 5 <0.000440>
15:47:35.177465 read(10, "\4\16\4\2\n \0", 260) = 7 <0.000050>
15:47:35.183165 epoll_ctl(10, EPOLL_CTL_ADD, 13, {EPOLLIN, {u32=978472, u64=978472}}) = 0 <0.000356>
15:47:35.196202 epoll_ctl(10, EPOLL_CTL_ADD, 14, {EPOLLIN, {u32=950472, u64=950472}}) = 0 <0.000110>
15:47:35.204787 epoll_wait(10,

```

## 时间点整理

```bash
[25-09-17 15:46:49.720][rk_bt_c_ad][Mock_RK_BleWr 288][thread 596][D] [==>] Mock_RK_BleWriteData
开始失效，但是可以读，但是不能写，直到
[25-09-17 15:47:05.189][bt_ble_rpc][operator()    239][thread 612][I] receive msg, uuid
此时还read收到消息，要求重新返回列表
[25-09-17 15:47:35.124][my_btgatt_][att_debug_cb  213][thread 612][D]
链接被对方断开


15:46:59.473126 epoll_wait(10, [{EPOLLIN, {u32=979240, u64=979240}}], 10, -1) = 1 <5.707519>
15:47:05.181803 read(15, "\22\r\0/>|20|WIFILISTV2|~|#", 185) = 23 <0.000062>
15:47:05.186679 epoll_ctl(10, EPOLL_CTL_ADD, 16, {EPOLLIN|EPOLLONESHOT, {u32=984776, u64=984776}}) = 0 <0.000047>
15:47:05.197324 epoll_ctl(10, EPOLL_CTL_DEL, 16, NULL) = 0 <0.000062>
15:47:05.203431 epoll_wait(10, [{EPOLLERR|EPOLLHUP, {u32=979240, u64=979240}}], 10, -1) = 1 <29.916374>
15:47:35.129862 epoll_ctl(10, EPOLL_CTL_DEL, 15, NULL) = 0 <0.000322>
```

## 其他描述符

```bash
[25-09-17 15:09:15.895][my_btgatt_][ble_server_ru 832][thread 612][D] delete mainloop_notify_fd_: [13]
[25-09-17 15:09:15.895][my_btgatt_][stop          343][thread 612][D] [===>] L2CapAttChannelListener::stop
[25-09-17 15:09:15.895][my_btgatt_][stop          347][thread 612][D] mainloop_remove_fd and close listen_sock_: [14]
[25-09-17 15:09:15.895][my_btgatt_][stop          354][thread 612][D] close accepted_sock_: [15]
[25-09-17 15:09:15.895][my_btgatt_][stop          360][thread 612][D] [exit] L2CapAttChannelListener::stop
[25-09-17 15:09:15.895][my_btgatt_][ble_server_ru 840][thread 612][D] destroy the core_pure_c_server_handle_ handle for [att] [gap] [gatt] resouce
[25-09-17 15:09:15.895][my_btgatt_][ble_server_ru 844][thread 612][D] [exit] server loop, count: [1]
[25-09-17 15:09:15.895][my_btgatt_][ble_server_ru 709][thread 612][D] [===>] server loop, count: [2]
[25-09-17 15:09:15.895][my_btgatt_][ble_server_ru 720][thread 612][D] set_advertising_data, adverting_device_name: [YDAS250701000005], advertisng_service_uuid: [0000180A-0000-1000-8000-00805F9B34FB]
[EVENT] Module Recv Commands (send: StampController cmd:3))

```

