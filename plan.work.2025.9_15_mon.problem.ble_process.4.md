---
id: pwdmfiscjwp7pqrmi8ju4ql
title: '4'
desc: ''
updated: 1760424025043
created: 1758160486566
---
## 场景：触发扫描以后，没有扫描结果返回，但可以反复触发，可以正常接收

触发时程序日志

```bash
sStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1 isGpySync:0}
[25-09-18 09:54:07.762][my_btgatt_][att_debug_cb  213][thread1707][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0x398de8) ATT receiv
ed: 23

[25-09-18 09:54:07.763][my_btgatt_][att_debug_cb  213][thread1707][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0x398de8) ATT PDU re
ceived: 0x12

[25-09-18 09:54:07.763][my_btgatt_][gatt_debug_cb 220][thread1707][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - handle:
0x000d

enter rkbt_write_cb, data: />|20|WIFILISTV2|~|#2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#`lu
[rkbt_write_cb] offset: 0, len: 20
Data: 2F 3E 7C 32 30 7C 57 49 46 49 4C 49 53 54 56 32 7C 7E 7C 23
[25-09-18 09:54:07.763][rk_bt_c_ad][operator()     65][thread1707][I] receive msg, uuid: [00009999-0000-1000-8000-00805F9B34FB], msg: [/>|20|WIFILISTV2|~|#2|100|#1~1~1~1~1~1~1~7076~2~徐佳飞-smart-奥特之章重生~-1~1|#`lu], len
: [20]
[25-09-18 09:54:07.763][my_btgatt_][gatt_debug_cb 220][thread1707][D] server: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write Compl
ete: err 0

[25-09-18 09:54:07.763][my_btgatt_][att_debug_cb  213][thread1707][D] att: /home/xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0x398de8) ATT op
 0x13

[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|20|WIFILISTV2|~|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:2(CTL_NETWORK_SCAN_APS) args:1
m_isScan= true
[25-09-18 09:54:07.765][my_btgatt_][att_debug_cb  213][thread1707][D] att: < 13                                               .

```

## 对应的strace

```bash
root@Rockchip:/oem$ strace -p 1707 -tt -T -s 200 -e trace=epoll_wait,epoll_ctl,r
ead,writev
strace: Process 1707 attached
09:53:02.064118 epoll_wait(9, [{EPOLLIN, {u32=3948520, u64=3948520}}], 10, -1) = 1 <2.546317>
09:53:04.611541 read(43, "\22\r\0/>|20|WIFILISTV2|~|#", 185) = 23 <0.000063>
09:53:04.613120 read(35, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33\0\0\0\21\0\0\0\3\0\0\0\f\200\0\0\0\310\\\1\200\310\372'p\311\325\16\200\312\333Z\360\36\2726\r", 68) = 68 <0.000051>
09:53:04.615007 read(35, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0\0\0\0U\223-\231\0\0\0\32\0\0\0\0XhF\232\0\0\0\33\0\0\0\0\0\0\nCST-8\n", 68) = 68 <0.000228>
09:53:04.619767 epoll_ctl(9, EPOLL_CTL_ADD, 35, {EPOLLIN|EPOLLONESHOT, {u32=5298400, u64=5298400}}) = 0 <0.000267>
09:53:04.625754 epoll_ctl(9, EPOLL_CTL_DEL, 35, NULL) = 0 <0.000294>
09:53:04.628975 epoll_ctl(9, EPOLL_CTL_MOD, 43, {EPOLLIN|EPOLLOUT|EPOLLRDHUP, {u32=3948520, u64=3948520}}) = 0 <0.000049>
09:53:04.632125 epoll_wait(9, [{EPOLLOUT, {u32=3948520, u64=3948520}}], 10, -1) = 1 <0.000045>
09:53:04.636766 writev(43, [{iov_base="\23", iov_len=1}], 1) = 1 <0.000405>
09:53:04.641147 epoll_wait(9, [{EPOLLOUT, {u32=3948520, u64=3948520}}], 10, -1) = 1 <0.000303>
09:53:04.643919 epoll_ctl(9, EPOLL_CTL_MOD, 43, {EPOLLIN|EPOLLRDHUP, {u32=3948520, u64=3948520}}) = 0 <0.000230>
09:53:04.646676 epoll_wait(9,
上面是在刷新列表是时，接收到message fd43，但是发送不出去，并没有报错^Cstrace: Process 1707 detached
 <detached ...>


```

## 进程fd状况

```bash
root@Rockchip:/oem$ ls -l /proc/1628/task/1707/fd
lrwx------    1 root     root            64 66 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 55 -> /dev/ttyGS0
lrwx------    1 root     root            64 48 -> /tmp/LCK..ttyGS0
lrwx------    1 root     root            64 46 -> anon_inode:[pidfd]
lr-x------    1 root     root            64 45 -> pipe:[79916]
lr-x------    1 root     root            64 44 -> pipe:[68514]
lrwx------    1 root     root            64 43 -> socket:[79746]
lr-x------    1 root     root            64 42 -> pipe:[60859]
lrwx------    1 root     root            64 41 -> socket:[61634]
lrwx------    1 root     root            64 40 -> socket:[73459]
lr-x------    1 root     root            64 39 -> pipe:[60858]
lrwx------    1 root     root            64 38 -> anon_inode:[eventfd]
l-wx------    1 root     root            64 37 -> pipe:[60857]
lrwx------    1 root     root            64 34 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 33 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 32 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 31 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 30 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 29 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 28 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 27 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 26 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 25 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 24 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 23 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 22 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 21 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 20 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 19 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 18 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 17 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 16 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 15 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 14 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 13 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 12 -> anon_inode:[eventfd]
lr-x------    1 root     root            64 11 -> /dev/input/event0
lrwx------    1 root     root            64 10 -> /dev/ttyS1
lrwx------    1 root     root            64 9 -> anon_inode:[eventpoll]
lrwx------    1 root     root            64 8 -> socket:[60022]
lrwx------    1 root     root            64 7 -> socket:[60021]
lrwx------    1 root     root            64 6 -> /dev/tty0
lrwx------    1 root     root            64 5 -> /dev/fb0
lrwx------    1 root     root            64 4 -> anon_inode:[eventfd]
lr-x------    1 root     root            64 3 -> /dev/urandom
lrwx------    1 root     root            64 2 -> /dev/pts/1
lrwx------    1 root     root            64 1 -> /dev/pts/1
lrwx------    1 root     root            64 0 -> /dev/pts/1

```

```bash
root@Rockchip:/oem$ lsof
1       /bin/busybox    /dev/console
1       /bin/busybox    /dev/console
1       /bin/busybox    /dev/console
90      /sbin/udevd     /dev/null
90      /sbin/udevd     /dev/null
90      /sbin/udevd     /dev/null
90      /sbin/udevd     socket:[4552]
90      /sbin/udevd     socket:[4553]
90      /sbin/udevd     /dev/kmsg
90      /sbin/udevd     anon_inode:inotify
90      /sbin/udevd     anon_inode:[signalfd]
90      /sbin/udevd     socket:[4556]
90      /sbin/udevd     socket:[4557]
90      /sbin/udevd     anon_inode:[eventpoll]
175     /usr/bin/adbd   /dev/null
175     /usr/bin/adbd   /dev/null
175     /usr/bin/adbd   /dev/null
175     /usr/bin/adbd   socket:[4951]
175     /usr/bin/adbd   socket:[4952]
175     /usr/bin/adbd   socket:[4953]
175     /usr/bin/adbd   socket:[4954]
175     /usr/bin/adbd   socket:[4955]
175     /usr/bin/adbd   socket:[4956]
175     /usr/bin/adbd   /dev/usb-ffs/adb/ep0
175     /usr/bin/adbd   /dev/usb-ffs/adb/ep1
175     /usr/bin/adbd   /dev/usb-ffs/adb/ep2
175     /usr/bin/adbd   socket:[4959]
175     /usr/bin/adbd   socket:[4960]
175     /usr/bin/adbd   socket:[4961]
175     /usr/bin/adbd   /dev/ptmx
175     /usr/bin/adbd   /dev/ptmx
175     /usr/bin/adbd   /dev/ptmx
190     /oem/usr/sbin/fcgiwrap  socket:[4984]
190     /oem/usr/sbin/fcgiwrap  /dev/null
190     /oem/usr/sbin/fcgiwrap  /dev/null
217     /oem/usr/sbin/nginx     /dev/null
217     /oem/usr/sbin/nginx     /dev/null
217     /oem/usr/sbin/nginx     /tmp/nginx/error.log
217     /oem/usr/sbin/nginx     socket:[5036]
217     /oem/usr/sbin/nginx     /tmp/nginx/access.log
217     /oem/usr/sbin/nginx     /tmp/nginx/error.log
217     /oem/usr/sbin/nginx     socket:[5032]
217     /oem/usr/sbin/nginx     socket:[5033]
217     /oem/usr/sbin/nginx     socket:[5037]
219     /oem/usr/sbin/nginx     /dev/null
219     /oem/usr/sbin/nginx     /dev/null
219     /oem/usr/sbin/nginx     /tmp/nginx/error.log
219     /oem/usr/sbin/nginx     /tmp/nginx/access.log
219     /oem/usr/sbin/nginx     /tmp/nginx/error.log
219     /oem/usr/sbin/nginx     socket:[5032]
219     /oem/usr/sbin/nginx     socket:[5033]
219     /oem/usr/sbin/nginx     socket:[5037]
219     /oem/usr/sbin/nginx     anon_inode:[eventpoll]
219     /oem/usr/sbin/nginx     anon_inode:[eventfd]
223     /bin/led_watchdog       /dev/null
223     /bin/led_watchdog       /dev/console
223     /bin/led_watchdog       /dev/console
232     /bin/busybox    /dev/null
232     /bin/busybox    /dev/null
232     /bin/busybox    /dev/null
232     /bin/busybox    socket:[5077]
238     /sbin/udevd     /dev/null
238     /sbin/udevd     /dev/null
238     /sbin/udevd     /dev/null
238     /sbin/udevd     socket:[5084]
238     /sbin/udevd     socket:[5085]
238     /sbin/udevd     /dev/kmsg
238     /sbin/udevd     anon_inode:inotify
238     /sbin/udevd     anon_inode:[signalfd]
238     /sbin/udevd     socket:[5091]
238     /sbin/udevd     socket:[5092]
238     /sbin/udevd     anon_inode:[eventpoll]
325     /oem/usr/bin/rkaiq_3A_server    /dev/null
325     /oem/usr/bin/rkaiq_3A_server    /dev/console
325     /oem/usr/bin/rkaiq_3A_server    /dev/console
325     /oem/usr/bin/rkaiq_3A_server    /dev/video20
325     /oem/usr/bin/rkaiq_3A_server    /tmp/aiq0.lock
325     /oem/usr/bin/rkaiq_3A_server    /dev/v4l-subdev2
325     /oem/usr/bin/rkaiq_3A_server    /dev/v4l-subdev3
325     /oem/usr/bin/rkaiq_3A_server    /dev/video19
325     /oem/usr/bin/rkaiq_3A_server    /dev/video20
325     /oem/usr/bin/rkaiq_3A_server    /dev/v4l-subdev0
325     /oem/usr/bin/rkaiq_3A_server    /dev/video0
325     /oem/usr/bin/rkaiq_3A_server    /dev/video1
325     /oem/usr/bin/rkaiq_3A_server    /dev/video18
325     /oem/usr/bin/rkaiq_3A_server    /dev/video17
325     /oem/usr/bin/rkaiq_3A_server    socket:[5332]
325     /oem/usr/bin/rkaiq_3A_server    pipe:[5333]
325     /oem/usr/bin/rkaiq_3A_server    pipe:[5333]
325     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
325     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
325     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
325     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
325     /oem/usr/bin/rkaiq_3A_server    pipe:[5342]
325     /oem/usr/bin/rkaiq_3A_server    pipe:[5342]
325     /oem/usr/bin/rkaiq_3A_server    /dev/video20
325     /oem/usr/bin/rkaiq_3A_server    pipe:[5345]
325     /oem/usr/bin/rkaiq_3A_server    pipe:[5345]
325     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
325     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
325     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
325     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
325     /oem/usr/bin/rkaiq_3A_server    pipe:[5352]
325     /oem/usr/bin/rkaiq_3A_server    pipe:[5352]
346     /bin/busybox    /dev/null
346     /bin/busybox    /tmp/moblienet
346     /bin/busybox    /tmp/moblienet
346     /bin/busybox    /usr/bin/rpdzkj-mobilenet.sh
347     /bin/busybox    /dev/console
347     /bin/busybox    /dev/console
347     /bin/busybox    /dev/console
347     /bin/busybox    /dev/tty
374     /usr/bin/rtk_hciattach  /dev/ttyS0
384     /bin/busybox    /dev/pts/0
384     /bin/busybox    /dev/pts/0
384     /bin/busybox    /dev/pts/0
384     /bin/busybox    /dev/tty
684     /bin/busybox    /dev/pts/2
684     /bin/busybox    /dev/pts/2
684     /bin/busybox    /dev/pts/2
684     /bin/busybox    /dev/tty
1502    /bin/busybox    /dev/pts/1
1502    /bin/busybox    /dev/pts/1
1502    /bin/busybox    /dev/pts/1
1502    /bin/busybox    /dev/tty
1628    /oem/ydn-rk     /dev/pts/1
1628    /oem/ydn-rk     /dev/pts/1
1628    /oem/ydn-rk     /dev/pts/1
1628    /oem/ydn-rk     /dev/urandom
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     /dev/fb0
1628    /oem/ydn-rk     /dev/tty0
1628    /oem/ydn-rk     socket:[60021]
1628    /oem/ydn-rk     socket:[60022]
1628    /oem/ydn-rk     anon_inode:[eventpoll]
1628    /oem/ydn-rk     /dev/ttyS1
1628    /oem/ydn-rk     /dev/input/event0
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     pipe:[60857]
1628    /oem/ydn-rk     anon_inode:[eventfd]
1628    /oem/ydn-rk     pipe:[60858]
1628    /oem/ydn-rk     socket:[73459]
1628    /oem/ydn-rk     socket:[61634]
1628    /oem/ydn-rk     pipe:[60859]
1628    /oem/ydn-rk     socket:[79746]
1628    /oem/ydn-rk     pipe:[68514]
1628    /oem/ydn-rk     pipe:[79916]
1628    /oem/ydn-rk     anon_inode:[pidfd]
1628    /oem/ydn-rk     /tmp/LCK..ttyGS0
1628    /oem/ydn-rk     /dev/ttyGS0
1628    /oem/ydn-rk     anon_inode:[eventfd]
1740    /usr/bin/wpa_supplicant /dev/null
1740    /usr/bin/wpa_supplicant /dev/null
1740    /usr/bin/wpa_supplicant /dev/null
1740    /usr/bin/wpa_supplicant socket:[70438]
1740    /usr/bin/wpa_supplicant socket:[60698]
1740    /usr/bin/wpa_supplicant socket:[60699]
1740    /usr/bin/wpa_supplicant socket:[60700]
1740    /usr/bin/wpa_supplicant socket:[60021]
1740    /usr/bin/wpa_supplicant socket:[60022]
1740    /usr/bin/wpa_supplicant socket:[60701]
1740    /usr/bin/wpa_supplicant /dev/ttyS1
1740    /usr/bin/wpa_supplicant /dev/input/event0
1740    /usr/bin/wpa_supplicant /dev/rfkill
1740    /usr/bin/wpa_supplicant socket:[60705]
1740    /usr/bin/wpa_supplicant socket:[60706]
1740    /usr/bin/wpa_supplicant socket:[60715]
1740    /usr/bin/wpa_supplicant socket:[60582]
1775    /usr/bin/pppd   pipe:[60857]
1775    /usr/bin/pppd   pipe:[60858]
1775    /usr/bin/pppd   pipe:[60859]
1775    /usr/bin/pppd   /dev/null
1775    /usr/bin/pppd   socket:[60913]
1775    /usr/bin/pppd   /run/pppd2.tdb
1775    /usr/bin/pppd   pipe:[60917]
1775    /usr/bin/pppd   socket:[60021]
1775    /usr/bin/pppd   socket:[60022]
1775    /usr/bin/pppd   pipe:[60917]
1775    /usr/bin/pppd   /dev/ttyS1
1775    /usr/bin/pppd   /dev/input/event0
1775    /usr/bin/pppd   /dev/ttyS4
1775    /usr/bin/pppd   /dev/ppp
1775    /usr/bin/pppd   /dev/ppp
1775    /usr/bin/pppd   socket:[60582]
2799    /bin/busybox    /dev/null
2799    /bin/busybox    /tmp/moblienet
2799    /bin/busybox    /tmp/moblienet

```
