---
id: mkzof1vark7jz2h7au4evlr
title: '2'
desc: ''
updated: 1758077331833
created: 1758029393384
---

思路梳理:

```bash
确定蓝牙进程和蓝牙io线程

通过strace确定蓝牙使用的fd编号

通过查看 /proc/<pid>/fd 以及 /proc/<pid>/task/<threadid>/fd
确定 fd编号与内核inode的映射
但目前两个命令输出的都是进程所在fd

lsof -p <pid> busybox版本还是会罗列所有的fd



```


现场排查

```bash
root@Rockchip:/oem$ ls -l /proc/2664/task/2671/fd
lrwx------    1 root     root            64 14 -> socket:[140275]
lrwx------    1 root     root            64 13 -> socket:[139915]
lrwx------    1 root     root            64 12 -> anon_inode:[eventfd]
lr-x------    1 root     root            64 11 -> /dev/input/event0
lrwx------    1 root     root            64 10 -> /dev/ttyS1
lrwx------    1 root     root            64 9 -> anon_inode:[eventpoll]
lrwx------    1 root     root            64 8 -> socket:[80274]
lrwx------    1 root     root            64 7 -> socket:[80273]
lrwx------    1 root     root            64 6 -> socket:[80882]
lrwx------    1 root     root            64 5 -> socket:[80873]
lrwx------    1 root     root            64 4 -> anon_inode:[eventpoll]
lrwx------    1 root     root            64 3 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 2 -> /dev/pts/0
lrwx------    1 root     root            64 1 -> /dev/pts/0
lrwx------    1 root     root            64 0 -> /dev/pts/0



```


```bash
root@Rockchip:/oem/test$ lsof -p 2664
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
130     /sbin/udevd     /dev/null
130     /sbin/udevd     /dev/null
130     /sbin/udevd     /dev/null
130     /sbin/udevd     anon_inode:[signalfd]
130     /sbin/udevd     anon_inode:[eventpoll]
130     /sbin/udevd     /dev/kmsg
130     /sbin/udevd     anon_inode:inotify
130     /sbin/udevd     socket:[4557]
130     /sbin/udevd     socket:[4651]
175     /usr/bin/adbd   /dev/null
175     /usr/bin/adbd   /dev/null
175     /usr/bin/adbd   /dev/null
175     /usr/bin/adbd   socket:[4951]
175     /usr/bin/adbd   socket:[4952]
175     /usr/bin/adbd   socket:[4953]
175     /usr/bin/adbd   socket:[4954]
175     /usr/bin/adbd   socket:[4955]
175     /usr/bin/adbd   socket:[4956]
175     /usr/bin/adbd   socket:[4957]
175     /usr/bin/adbd   /dev/usb-ffs/adb/ep0
175     /usr/bin/adbd   /dev/usb-ffs/adb/ep1
175     /usr/bin/adbd   /dev/usb-ffs/adb/ep2
175     /usr/bin/adbd   socket:[4961]
175     /usr/bin/adbd   socket:[4962]
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
220     /sbin/udevd     /dev/null
220     /sbin/udevd     /dev/null
220     /sbin/udevd     /dev/null
220     /sbin/udevd     anon_inode:[signalfd]
220     /sbin/udevd     anon_inode:[eventpoll]
220     /sbin/udevd     /dev/kmsg
220     /sbin/udevd     anon_inode:inotify
220     /sbin/udevd     socket:[4557]
220     /sbin/udevd     socket:[5045]
221     /sbin/udevd     /dev/null
221     /sbin/udevd     /dev/null
221     /sbin/udevd     /dev/null
221     /sbin/udevd     anon_inode:[signalfd]
221     /sbin/udevd     anon_inode:[eventpoll]
221     /sbin/udevd     /dev/kmsg
221     /sbin/udevd     anon_inode:inotify
221     /sbin/udevd     socket:[4557]
221     /sbin/udevd     socket:[5046]
222     /sbin/udevd     /dev/null
222     /sbin/udevd     /dev/null
222     /sbin/udevd     /dev/null
222     /sbin/udevd     anon_inode:[signalfd]
222     /sbin/udevd     anon_inode:[eventpoll]
222     /sbin/udevd     /dev/kmsg
222     /sbin/udevd     anon_inode:inotify
222     /sbin/udevd     socket:[4557]
222     /sbin/udevd     socket:[5047]
223     /bin/led_watchdog       /dev/null
223     /bin/led_watchdog       /dev/console
223     /bin/led_watchdog       /dev/console
232     /bin/busybox    /dev/null
232     /bin/busybox    /dev/null
232     /bin/busybox    /dev/null
232     /bin/busybox    socket:[5079]
240     /sbin/udevd     /dev/null
240     /sbin/udevd     /dev/null
240     /sbin/udevd     /dev/null
240     /sbin/udevd     socket:[5086]
240     /sbin/udevd     socket:[5087]
240     /sbin/udevd     /dev/kmsg
240     /sbin/udevd     anon_inode:inotify
240     /sbin/udevd     anon_inode:[signalfd]
240     /sbin/udevd     socket:[5096]
240     /sbin/udevd     socket:[5097]
240     /sbin/udevd     anon_inode:[eventpoll]
329     /oem/usr/bin/rkaiq_3A_server    /dev/null
329     /oem/usr/bin/rkaiq_3A_server    /dev/console
329     /oem/usr/bin/rkaiq_3A_server    /dev/console
329     /oem/usr/bin/rkaiq_3A_server    /dev/video20
329     /oem/usr/bin/rkaiq_3A_server    /tmp/aiq0.lock
329     /oem/usr/bin/rkaiq_3A_server    /dev/v4l-subdev2
329     /oem/usr/bin/rkaiq_3A_server    /dev/v4l-subdev3
329     /oem/usr/bin/rkaiq_3A_server    /dev/video19
329     /oem/usr/bin/rkaiq_3A_server    /dev/video20
329     /oem/usr/bin/rkaiq_3A_server    /dev/v4l-subdev0
329     /oem/usr/bin/rkaiq_3A_server    /dev/video0
329     /oem/usr/bin/rkaiq_3A_server    /dev/video1
329     /oem/usr/bin/rkaiq_3A_server    /dev/video18
329     /oem/usr/bin/rkaiq_3A_server    /dev/video17
329     /oem/usr/bin/rkaiq_3A_server    socket:[5374]
329     /oem/usr/bin/rkaiq_3A_server    pipe:[5375]
329     /oem/usr/bin/rkaiq_3A_server    pipe:[5375]
329     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
329     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
329     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
329     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
329     /oem/usr/bin/rkaiq_3A_server    pipe:[5384]
329     /oem/usr/bin/rkaiq_3A_server    pipe:[5384]
329     /oem/usr/bin/rkaiq_3A_server    /dev/video20
329     /oem/usr/bin/rkaiq_3A_server    pipe:[5387]
329     /oem/usr/bin/rkaiq_3A_server    pipe:[5387]
329     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
329     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
329     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
329     /oem/usr/bin/rkaiq_3A_server    /dmabuf:
329     /oem/usr/bin/rkaiq_3A_server    pipe:[5394]
329     /oem/usr/bin/rkaiq_3A_server    pipe:[5394]
365     /bin/busybox    /dev/null
365     /bin/busybox    /tmp/moblienet
365     /bin/busybox    /tmp/moblienet
365     /bin/busybox    /usr/bin/rpdzkj-mobilenet.sh
366     /bin/busybox    /dev/console
366     /bin/busybox    /dev/console
366     /bin/busybox    /dev/console
366     /bin/busybox    /dev/tty
393     /usr/bin/rtk_hciattach  /dev/ttyS0
701     /bin/busybox    /dev/pts/0
701     /bin/busybox    /dev/pts/0
701     /bin/busybox    /dev/pts/0
701     /bin/busybox    /dev/tty
856     /bin/busybox    /dev/pts/1
856     /bin/busybox    /dev/pts/1
856     /bin/busybox    /dev/pts/1
856     /bin/busybox    /dev/tty
2582    /oem/ydn-rk     /dev/pts/0
2582    /oem/ydn-rk     /dev/pts/0
2582    /oem/ydn-rk     /dev/pts/0
2582    /oem/ydn-rk     /dev/urandom
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     /dev/fb0
2582    /oem/ydn-rk     /dev/tty0
2582    /oem/ydn-rk     socket:[80273]
2582    /oem/ydn-rk     socket:[80274]
2582    /oem/ydn-rk     pipe:[83860]
2582    /oem/ydn-rk     /dev/ttyS1
2582    /oem/ydn-rk     /dev/input/event0
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     anon_inode:[eventpoll]
2582    /oem/ydn-rk     socket:[80881]
2582    /oem/ydn-rk     pipe:[81146]
2582    /oem/ydn-rk     pipe:[81147]
2582    /oem/ydn-rk     socket:[82554]
2582    /oem/ydn-rk     pipe:[81148]
2582    /oem/ydn-rk     pipe:[140420]
2582    /oem/ydn-rk     pipe:[90837]
2582    /oem/ydn-rk     pipe:[93157]
2582    /oem/ydn-rk     anon_inode:[pidfd]
2582    /oem/ydn-rk     /tmp/LCK..ttyGS0
2582    /oem/ydn-rk     pipe:[102055]
2582    /oem/ydn-rk     pipe:[136233]
2582    /oem/ydn-rk     anon_inode:[eventfd]
2582    /oem/ydn-rk     /dev/ttyGS0
2664    /oem/test/bt_mock_server        /dev/pts/0
2664    /oem/test/bt_mock_server        /dev/pts/0
2664    /oem/test/bt_mock_server        /dev/pts/0
2664    /oem/test/bt_mock_server        anon_inode:[eventfd]
2664    /oem/test/bt_mock_server        anon_inode:[eventpoll]
2664    /oem/test/bt_mock_server        socket:[80873]
2664    /oem/test/bt_mock_server        socket:[80882]
2664    /oem/test/bt_mock_server        socket:[80273]
2664    /oem/test/bt_mock_server        socket:[80274]
2664    /oem/test/bt_mock_server        anon_inode:[eventpoll]
2664    /oem/test/bt_mock_server        /dev/ttyS1
2664    /oem/test/bt_mock_server        /dev/input/event0
2664    /oem/test/bt_mock_server        anon_inode:[eventfd]
2664    /oem/test/bt_mock_server        socket:[139915]
2664    /oem/test/bt_mock_server        socket:[140275]
2688    /usr/bin/wpa_supplicant /dev/null
2688    /usr/bin/wpa_supplicant /dev/null
2688    /usr/bin/wpa_supplicant /dev/null
2688    /usr/bin/wpa_supplicant socket:[162901]
2688    /usr/bin/wpa_supplicant socket:[80926]
2688    /usr/bin/wpa_supplicant socket:[80927]
2688    /usr/bin/wpa_supplicant socket:[80928]
2688    /usr/bin/wpa_supplicant socket:[80273]
2688    /usr/bin/wpa_supplicant socket:[80274]
2688    /usr/bin/wpa_supplicant socket:[80929]
2688    /usr/bin/wpa_supplicant /dev/ttyS1
2688    /usr/bin/wpa_supplicant /dev/input/event0
2688    /usr/bin/wpa_supplicant /dev/rfkill
2688    /usr/bin/wpa_supplicant socket:[80933]
2688    /usr/bin/wpa_supplicant socket:[80934]
2688    /usr/bin/wpa_supplicant socket:[80943]
2688    /usr/bin/wpa_supplicant socket:[80881]
2736    /usr/bin/pppd   pipe:[81146]
2736    /usr/bin/pppd   pipe:[81147]
2736    /usr/bin/pppd   pipe:[81148]
2736    /usr/bin/pppd   /dev/null
2736    /usr/bin/pppd   socket:[81202]
2736    /usr/bin/pppd   /run/pppd2.tdb
2736    /usr/bin/pppd   pipe:[81206]
2736    /usr/bin/pppd   socket:[80273]
2736    /usr/bin/pppd   socket:[80274]
2736    /usr/bin/pppd   pipe:[81206]
2736    /usr/bin/pppd   /dev/ttyS1
2736    /usr/bin/pppd   /dev/input/event0
2736    /usr/bin/pppd   /dev/ttyS4
2736    /usr/bin/pppd   /dev/ppp
2736    /usr/bin/pppd   /dev/ppp
2736    /usr/bin/pppd   socket:[80881]
2931    /bin/busybox    /dev/pts/2
2931    /bin/busybox    /dev/pts/2
2931    /bin/busybox    /dev/pts/2
2931    /bin/busybox    /dev/tty
3578    /bin/busybox    /dev/null
3578    /bin/busybox    /tmp/moblienet
3578    /bin/busybox    /tmp/moblienet

```


```bash
root@Rockchip:/oem/test$ strace -fp 2671 -e trace=epoll_ctl,epoll_wait,writev
strace: Process 2671 attached with 2 threads
[pid  2671] epoll_wait(9,  <unfinished ...>
[pid  2664] epoll_wait(4, [], 128, 1444) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1958) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1998) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1958) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1959) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1958) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1998) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1997) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1959) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1998) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1960) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1998) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1997) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1960) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1998) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1962) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1997) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1963) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1998) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1964) = 0
[pid  2664] epoll_ctl(4, EPOLL_CTL_MOD, 3, {EPOLLIN|EPOLLERR|EPOLLET, {u32=966452, u64=966452}}) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1998) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1997) = 1
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, ^Cstrace: Process 2671 detached
strace: Process 2664 detached
 <detached ...>


```


```bash
oot@Rockchip:/oem/test$ strace -fp 2671 -e trace=network,write,read,send,recv,e
poll_wait,writev
strace: Process 2671 attached with 2 threads
[pid  2671] epoll_wait(9,  <unfinished ...>
[pid  2664] epoll_wait(4, [], 128, 212) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1999) = 1
[pid  2664] recv(6, "\33\0\0\0", 4, 0)  = 4
[pid  2664] recv(6, "A\5\0\0\24\0ble_client_heartbeat\5", 27, 0) = 27
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] read(15, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33"..., 68) = 68
[pid  2664] read(15, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0"..., 68) = 68
[pid  2664] write(1, "\33[36m[25-09-16 21:35:01.501][bt_"..., 124) = 124
[pid  2664] sendmsg(6, {msg_name=NULL, msg_namelen=0, msg_iov=[{iov_base="\10\0\0\0", iov_len=4}, {iov_base="A\5\0\0\0\0\2\0", iov_len=8}], msg_iovlen=2, msg_controllen=0, msg_flags=0}, MSG_
NOSIGNAL) = 12
[pid  2664] recv(6, 0xec1d4, 4, 0)      = -1 EAGAIN (Resource temporarily unavailable)
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1954) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1999) = 1
[pid  2664] recv(6, "\33\0\0\0", 4, 0)  = 4
[pid  2664] recv(6, "B\5\0\0\24\0ble_client_heartbeat\5", 27, 0) = 27
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] read(15, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33"..., 68) = 68
[pid  2664] read(15, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0"..., 68) = 68
[pid  2664] write(1, "\33[36m[25-09-16 21:35:03.505][bt_"..., 124) = 124
[pid  2664] sendmsg(6, {msg_name=NULL, msg_namelen=0, msg_iov=[{iov_base="\10\0\0\0", iov_len=4}, {iov_base="B\5\0\0\0\0\2\0", iov_len=8}], msg_iovlen=2, msg_controllen=0, msg_flags=0}, MSG_
NOSIGNAL) = 12
[pid  2664] recv(6, 0xec1d4, 4, 0)      = -1 EAGAIN (Resource temporarily unavailable)
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1948) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1998) = 1
[pid  2664] recv(6, "\33\0\0\0", 4, 0)  = 4
[pid  2664] recv(6, "C\5\0\0\24\0ble_client_heartbeat\5", 27, 0) = 27
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] read(15, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33"..., 68) = 68
[pid  2664] read(15, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0"..., 68) = 68
[pid  2664] write(1, "\33[36m[25-09-16 21:35:05.505][bt_"..., 124) = 124
[pid  2664] sendmsg(6, {msg_name=NULL, msg_namelen=0, msg_iov=[{iov_base="\10\0\0\0", iov_len=4}, {iov_base="C\5\0\0\0\0\2\0", iov_len=8}], msg_iovlen=2, msg_controllen=0, msg_flags=0}, MSG_
NOSIGNAL) = 12
[pid  2664] recv(6, 0xec1d4, 4, 0)      = -1 EAGAIN (Resource temporarily unavailable)
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1950) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1999) = 1
[pid  2664] recv(6, "\33\0\0\0", 4, 0)  = 4
[pid  2664] recv(6, "D\5\0\0\24\0ble_client_heartbeat\5", 27, 0) = 27
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] read(15, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33"..., 68) = 68
[pid  2664] read(15, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0"..., 68) = 68
[pid  2664] write(1, "\33[36m[25-09-16 21:35:07.507][bt_"..., 124) = 124
[pid  2664] sendmsg(6, {msg_name=NULL, msg_namelen=0, msg_iov=[{iov_base="\10\0\0\0", iov_len=4}, {iov_base="D\5\0\0\0\0\2\0", iov_len=8}], msg_iovlen=2, msg_controllen=0, msg_flags=0}, MSG_
NOSIGNAL) = 12
[pid  2664] recv(6, 0xec1d4, 4, 0)      = -1 EAGAIN (Resource temporarily unavailable)
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, [], 128, 1951) = 0
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966452, u64=966452}}], 128, 1999) = 1
[pid  2664] epoll_wait(4, [{EPOLLIN, {u32=966952, u64=966952}}], 128, 1999) = 1
[pid  2664] recv(6, "\33\0\0\0", 4, 0)  = 4
[pid  2664] recv(6, "E\5\0\0\24\0ble_client_heartbeat\5", 27, 0) = 27
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] read(15, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33"..., 68) = 68
[pid  2664] read(15, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0"..., 68) = 68
[pid  2664] write(1, "\33[36m[25-09-16 21:35:09.506][bt_"..., 124) = 124
[pid  2664] sendmsg(6, {msg_name=NULL, msg_namelen=0, msg_iov=[{iov_base="\10\0\0\0", iov_len=4}, {iov_base="E\5\0\0\0\0\2\0", iov_len=8}], msg_iovlen=2, msg_controllen=0, msg_flags=0}, MSG_
NOSIGNAL) = 12
[pid  2664] recv(6, 0xec1d4, 4, 0)      = -1 EAGAIN (Resource temporarily unavailable)
[pid  2664] epoll_wait(4, [], 128, 0)   = 0
[pid  2664] epoll_wait(4, ^Cstrace: Process 2671 detached
strace: Process 2664 detached
 <detached ...>

```



```bash
root@Rockchip:/oem/test$ strace -p 2671 -tt -T -e trace=send,sendto,sendmsg,recv ,recvmsg,write,read,poll,epoll_wait strace: Process 2671 attached 21:44:10.862312 epoll_wait(9,

root@Rockchip:/$ ls -l /proc/2671/fd/9
lrwx------    1 root     root            64 /proc/2671/fd/9 -> anon_inode:[eventpoll]

root@Rockchip:/$ cat /proc/2671/fdinfo/9
pos:    0
flags:  02000002
mnt_id: 11
ino:    80
tfd:       13 events:       19 data:            e60c8  pos:0 ino:2228b sdev:7
tfd:       12 events:       19 data:            ec110  pos:0 ino:50 sdev:a
tfd:       14 events:     2019 data:            f0088  pos:0 ino:223f3 sdev:7


# fd：被 epoll 监听的真实文件描述符（可能是 socket）
# events：监听事件掩码
# 1 = EPOLLIN（可读）
# 4 = EPOLLOUT（可写）
# 5 = EPOLLIN | EPOLLOUT


root@Rockchip:/$ ls -l /proc/2671/fd/13
lrwx------    1 root     root            64 /proc/2671/fd/13 -> socket:[139915]
root@Rockchip:/$ ls -l /proc/2671/fd/12
lrwx------    1 root     root            64 /proc/2671/fd/12 -> anon_inode:[eventfd]
root@Rockchip:/$ ls -l /proc/2671/fd/14
lrwx------    1 root     root            64 /proc/2671/fd/14 -> socket:[140275]


fd 13 → socket:[139915] → L2CAP socket（对应 /proc/2664/net/l2cap 的那条 139915）

fd 12 → anon_inode:[eventfd] → 一般是线程间通知事件，用于 epoll wakeup

fd 14 → socket:[140275] → 另一个蓝牙 socket（如果 /proc/2664/net/l2cap 中有 140275，就能对应上；如果没有，可能是 RFCOMM 或其他协议的 socket）

root@Rockchip:/$ ls -l /proc/2664/net/l2cap^C
root@Rockchip:/$ cat /proc/2664/net/l2cap
sk               RefCnt Rmem   Wmem   User   Inode  Parent
480df240 2      0      0      0      139915 0
95dae104 2      0      0      0      668    0

```

发送失败后，超时1后链接退出
```bash
root@Rockchip:/oem/test$ strace -p 2671 -tt -T -e trace=send,sendto,sendmsg,recv
,recvmsg,write,read,poll,epoll_wait
strace: Process 2671 attached
21:44:10.862312 epoll_wait(9, [{EPOLLERR|EPOLLHUP, {u32=983176, u64=983176}}], 10, -1) = 1 <349.607437>
21:50:00.472104 write(1, "\33[36m[25-09-16 21:50:00.472][my_"..., 272) = 272 <0.001898>
21:50:00.474747 write(1, "\33[m\n", 4)  = 4 <0.000182>
21:50:00.477690 write(1, "\33[36m[25-09-16 21:50:00.477][my_"..., 129) = 129 <0.001122>
21:50:00.480725 write(1, "\33[m\n", 4)  = 4 <0.000518>
21:50:00.483536 write(1, "\33[36m[25-09-16 21:50:00.483][my_"..., 98) = 98 <0.000526>
21:50:00.486438 sendmsg(6, {msg_name=NULL, msg_namelen=0, msg_iov=[{iov_base="9\0\0\0", iov_len=4}, {iov_base="_\0\0\0\t\0ble_state\5\1\2194:BA:06:91:04"..., iov_len=57}], msg_iovlen=2, msg_
controllen=0, msg_flags=0}, MSG_NOSIGNAL) = 61 <0.002541>
21:50:00.493812 write(1, "\33[32m[25-09-16 21:50:00.493][bt_"..., 164) = 164 <0.000516>
21:50:00.496911 write(1, "\33[36m[25-09-16 21:50:00.496][my_"..., 111) = 111 <0.000522>
21:50:00.499806 write(1, "\33[36m[25-09-16 21:50:00.499][my_"..., 115) = 115 <0.000347>
21:50:00.502601 write(1, "\33[36m[25-09-16 21:50:00.502][my_"..., 126) = 126 <0.000537>
21:50:00.507541 write(1, "\33[36m[25-09-16 21:50:00.507][my_"..., 105) = 105 <0.000375>
21:50:00.510326 write(1, "\33[36m[25-09-16 21:50:00.510][my_"..., 115) = 115 <0.000333>
21:50:00.512856 write(1, "\33[36m[25-09-16 21:50:00.512][my_"..., 155) = 155 <0.000625>
21:50:00.516002 write(1, "\33[36m[25-09-16 21:50:00.515][my_"..., 109) = 109 <0.000638>
21:50:00.518971 write(1, "\33[36m[25-09-16 21:50:00.518][my_"..., 109) = 109 <0.000621>
21:50:00.521863 write(1, "\33[36m[25-09-16 21:50:00.521][my_"..., 207) = 207 <0.000481>
21:50:00.525153 poll([{fd=9, events=POLLIN}], 1, 1000) = 1 ([{fd=9, revents=POLLIN}]) <0.000048>
21:50:00.528565 read(9, "\4\16\4\2\n \0", 260) = 7 <0.000087>
21:50:00.531464 write(1, "\33[36m[25-09-16 21:50:00.531][my_"..., 117) = 117 <0.000317>
21:50:00.535269 poll([{fd=9, events=POLLIN}], 1, 1000) = 1 ([{fd=9, revents=POLLIN}]) <0.000085>
21:50:00.538762 read(9, "\4\16\4\2\t \0", 260) = 7 <0.000308>
21:50:00.541797 poll([{fd=9, events=POLLIN}], 1, 1000) = 1 ([{fd=9, revents=POLLIN}]) <0.000301>
21:50:00.550491 read(9, "\4\16\4\2\n \0", 260) = 7 <0.000075>
21:50:00.553318 write(1, "\33[36m[25-09-16 21:50:00.553][my_"..., 120) = 120 <0.000342>
21:50:00.560372 write(1, "\33[36m[25-09-16 21:50:00.557][my_"..., 115) = 115 <0.000481>
21:50:00.563606 write(1, "\33[36m[25-09-16 21:50:00.563][my_"..., 92) = 92 <0.000496>
21:50:00.566453 write(1, "\33[36m[25-09-16 21:50:00.566][my_"..., 111) = 111 <0.000510>
21:50:00.569300 write(1, "\33[36m[25-09-16 21:50:00.569][my_"..., 135) = 135 <0.000577>
21:50:00.572998 write(1, "\33[36m[25-09-16 21:50:00.572][my_"..., 109) = 109 <0.000136>
21:50:00.580413 write(1, "\33[36m[25-09-16 21:50:00.580][my_"..., 100) = 100 <0.000593>
21:50:00.583173 write(1, "\33[36m[25-09-16 21:50:00.583][my_"..., 104) = 104 <0.000504>
21:50:00.586209 write(1, "\33[36m[25-09-16 21:50:00.586][my_"..., 102) = 102 <0.005343>
21:50:00.595468 write(1, "\33[36m[25-09-16 21:50:00.595][my_"..., 108) = 108 <0.000532>
21:50:00.598509 write(1, "\33[36m[25-09-16 21:50:00.598][my_"..., 103) = 103 <0.000389>
21:50:00.601605 write(1, "\33[36m[25-09-16 21:50:00.601][my_"..., 100) = 100 <0.000478>
21:50:00.604497 write(1, "\33[36m[25-09-16 21:50:00.604][my_"..., 113) = 113 <0.000198>
21:50:00.607710 write(1, "\33[36m[25-09-16 21:50:00.607][my_"..., 109) = 109 <0.000534>
21:50:00.610364 write(1, "\33[36m[25-09-16 21:50:00.610][my_"..., 98) = 98 <0.000480>
21:50:00.612990 epoll_wait(9,

```


新的连接失效，配置失败， 但是链接未失效，重新收发是成功的，看错了
```bash

[25-09-16 21:56:13.956][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter
 callback, rk_state: [OPEN]
[NETWK] CB WIFI STATUS : 6
=== WPA Network List (count: 0) ===
[25-09-16 21:56:13.965][rk_wifi_c_][print_saved_i 117][thread2672][I] Total saved networks: 0
[25-09-16 21:56:13.965][rk_wifi_c_][Mock_RK_WifiG 135][thread2672][I] [exit] RK_WifiGetSavedInfo: [0]
[25-09-16 21:56:13.965][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break
 callback, rk_state: [OPEN]
[25-09-16 21:56:14.068][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd44
16e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-48~1~zzwifi~0~0|#]
[25-09-16 21:56:14.069][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:56:14.069][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-
1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [40]
[25-09-16 21:56:14.071][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe
-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0268) ATT
 op 0x1b

[25-09-16 21:56:14.079][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout f
or cmd: [ble_client_notify], ret code: [0], cost time ms: [3], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], m
essage: [/>|34|WIFILISTV2|OK~0|-48~1~zzwifi~0~0|#]
[25-09-16 21:56:14.080][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd44
16e-1810-47f7-8248-eb8be3dc47f9], data: [/>|34|WIFILISTV2|OK~0|-48~1~zzwifi~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|34|WIFILISTV2|OK~0|-48~1~zzwifi~0~0|#][size:0 ver:2]
[25-09-16 21:56:14.078][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 34 7c 57 49
46 49 4c 49 53  .../>|34|WIFILIS

[25-09-16 21:56:14.089][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 34
38 7e 31 7e 7a  TV2|OK~0|-48~1~z

[25-09-16 21:56:14.100][my_btgatt_][att_debug_cb  213][thread2671][D] att:   7a 77 69 66 69 7e 30 7e 30 7c 23
                zwifi~0~0|#

[25-09-16 21:56:14.270][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd44
16e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#]
[25-09-16 21:56:14.270][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:56:14.270][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-
1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-16 21:56:14.272][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout f
or cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], m
essage: [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#]
[25-09-16 21:56:14.272][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd44
16e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-52~1~Libawall~0~0|#][size:0 ver:2]
[25-09-16 21:56:14.271][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe
-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0268) ATT
 op 0x1b

[25-09-16 21:56:14.275][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 36 7c 57 49
46 49 4c 49 53  .../>|36|WIFILIS

[25-09-16 21:56:14.280][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 35
32 7e 31 7e 4c  TV2|OK~0|-52~1~L

[25-09-16 21:56:14.287][my_btgatt_][att_debug_cb  213][thread2671][D] att:   69 62 61 77 61 6c 6c 7e 30 7e 30
7c 23           ibawall~0~0|#

[25-09-16 21:56:14.466][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:14.466][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:14.468][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd44
16e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-56~1~TP-LINK_F41B~0~0|#]
[25-09-16 21:56:14.469][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:56:14.469][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-
1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [46]
[25-09-16 21:56:14.470][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout f
or cmd: [ble_client_notify], ret code: [0], cost time ms: [1], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], m
essage: [/>|40|WIFILISTV2|OK~0|-56~1~TP-LINK_F41B~0~0|#]
[25-09-16 21:56:14.470][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd44
16e-1810-47f7-8248-eb8be3dc47f9], data: [/>|40|WIFILISTV2|OK~0|-56~1~TP-LINK_F41B~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|40|WIFILISTV2|OK~0|-56~1~TP-LINK_F41B~0~0|#][size:0 ver:2]
[25-09-16 21:56:14.473][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe
-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0268) ATT
 op 0x1b

[25-09-16 21:56:14.478][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 34 30 7c 57 49
46 49 4c 49 53  .../>|40|WIFILIS

[25-09-16 21:56:14.485][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 35
36 7e 31 7e 54  TV2|OK~0|-56~1~T

[25-09-16 21:56:14.492][my_btgatt_][att_debug_cb  213][thread2671][D] att:   50 2d 4c 49 4e 4b 5f 46 34 31 42
7e 30 7e 30 7c  P-LINK_F41B~0~0|

[25-09-16 21:56:14.497][my_btgatt_][att_debug_cb  213][thread2671][D] att:   23
                #

[25-09-16 21:56:14.668][rk_bt_c_ad][Mock_RK_BleWr 286][thread2654][D] [==>] Mock_RK_BleWriteData, uuid: [dfd44
16e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-58~1~ceshi_5G~0~0|#]
[25-09-16 21:56:14.669][my_btgatt_][ble_server_no 429][thread2664][D] enter ble_server_notify
[25-09-16 21:56:14.669][my_btgatt_][ble_server_no 460][thread2664][D] send gatt notification, uuid: [dfd4416e-
1810-47f7-8248-eb8be3dc47f9], handle: [15], payload len: [42]
[25-09-16 21:56:14.669][bt_ble_rpc][notify_sync   175][thread2654][W] [BtBleRpcClient] [notify_sync] timeout f
or cmd: [ble_client_notify], ret code: [0], cost time ms: [0], uuid: [dfd4416e-1810-47f7-8248-eb8be3dc47f9], m
essage: [/>|36|WIFILISTV2|OK~0|-58~1~ceshi_5G~0~0|#]
[25-09-16 21:56:14.669][rk_bt_c_ad][Mock_RK_BleWr 289][thread2654][D] [end] Mock_RK_BleWriteData, uuid: [dfd44
16e-1810-47f7-8248-eb8be3dc47f9], data: [/>|36|WIFILISTV2|OK~0|-58~1~ceshi_5G~0~0|#], ret: [0]
[NETWK] BT Write Data(ret=0): [/>|36|WIFILISTV2|OK~0|-58~1~ceshi_5G~0~0|#][size:0 ver:2]
[25-09-16 21:56:14.672][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe
-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0268) ATT
 op 0x1b

[25-09-16 21:56:14.678][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 1b 0f 00 2f 3e 7c 33 36 7c 57 49
46 49 4c 49 53  .../>|36|WIFILIS

[25-09-16 21:56:14.683][my_btgatt_][att_debug_cb  213][thread2671][D] att:   54 56 32 7c 4f 4b 7e 30 7c 2d 35
38 7e 31 7e 63  TV2|OK~0|-58~1~c

[25-09-16 21:56:14.689][my_btgatt_][att_debug_cb  213][thread2671][D] att:   65 73 68 69 5f 35 47 7e 30 7e 30
7c 23           eshi_5G~0~0|#

[25-09-16 21:56:16.467][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:16.468][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:18.467][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:18.468][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:
3 isCharge:1 energy:100 capcity:4158mV curent:-24nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared
:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1
 isGpySync:0}
[25-09-16 21:56:20.468][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:20.469][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:22.470][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:22.471][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:24.471][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:24.472][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:26.472][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:26.473][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:
3 isCharge:1 energy:100 capcity:4158mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared
:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1
 isGpySync:0}
[25-09-16 21:56:28.033][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe
-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xf0268) ATT rec
eived: 49

[25-09-16 21:56:28.042][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe
-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:can_read_data() (chan 0xf0268) ATT PDU
 received: 0x12

[25-09-16 21:56:28.049][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down
/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_cb() Write Req - hand
le: 0x000d

enter rkbt_write_cb, data: />|46|WIFISETTINGV2|1~1~Libawall-5G~-1223344|#2~徐佳飞-smart-奥特之章重生~-1~1|#me/
xjf1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share
[rkbt_write_cb] offset: 0, len: 46
Data: 2F 3E 7C 34 36 7C 57 49 46 49 53 45 54 54 49 4E 47 56 32 7C 31 7E 31 7E 4C 69 62 61 77 61 6C 6C 2D 35 47
 7E 2D 31 32 32 33 33 34 34 7C 23
[25-09-16 21:56:28.066][bt_ble_rpc][operator()    239][thread2671][I] receive msg, uuid: [00009999-0000-1000-8
000-00805F9B34FB], msg: [/>|46|WIFISETTINGV2|1~1~Libawall-5G~-1223344|#2~徐佳飞-smart-奥特之章重生~-1~1|#me/xj
f1127/codespace/git_down/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/share     ], len: [46]
[25-09-16 21:56:28.098][rk_bt_c_ad][operator()    250][thread2668][I] [BtBleRpcClient] Receive data, uuid: 000
09999-0000-1000-8000-00805F9B34FB, data: />|46|WIFISETTINGV2|1~1~Libawall-5G~-1223344|#, len: 46
[COMM] Handle One Message( m_msgId=1 [type:1] topic:ble datas:/>|46|WIFISETTINGV2|1~1~Libawall-5G~-1223344|# )
[NETWK] Handle Wlan Controller Cmd: send:CommandController type:1(CTL_NETWORK_CONFI_WIFI) args:1|1|Libawall-5G
|-1223344|1
[25-09-16 21:56:28.104][bt_ble_rpc][bluez_call_re 395][thread2671][I] bluez_call_receive_sync_ timeout for cmd
: [ble_client_receive], ret code: [0], cost time ms: [26]
[25-09-16 21:56:28.107][my_btgatt_][gatt_debug_cb 220][thread2671][D] server: /home/xjf1127/codespace/git_down
/pe-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/gatt-server.c:write_complete_cb() Write C
omplete: err 0

[25-09-16 21:56:28.113][my_btgatt_][att_debug_cb  213][thread2671][D] att: /home/xjf1127/codespace/git_down/pe
-bluez_independ_sample/module/bluez-lib/src/bluez-5.77/src/shared/att.c:bt_att_chan_write() (chan 0xf0268) ATT
 op 0x13

[25-09-16 21:56:28.132][my_btgatt_][att_debug_cb  213][thread2671][D] att: < 13
                .

[25-09-16 21:56:28.215][rk_wifi_c_][Mock_RK_WifiC  66][thread2654][I] [===>] RK_WifiConnect
[25-09-16 21:56:28.215][rk_wifi_c_][Mock_RK_WifiC  70][thread2654][I] the matched wpa_network_config_list coun
t: [0], with ssid: [Libawall-5G], pask: [-1223344]
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "250",
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
    "time": 1758030988,
    "version": "1.5.1"
}

[25-09-16 21:56:28.394][rk_wifi_c_][Mock_RK_WifiC  90][thread2654][I] [exit] RK_WifiConnect
[25-09-16 21:56:28.473][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:28.473][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:29.134][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter
 callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-16 21:56:29.134][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break
 callback, rk_state: [IDLE]
[25-09-16 21:56:30.473][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:30.474][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:32.185][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter
 callback, rk_state: [CONNECTING]
[NETWK] CB WIFI STATUS : 1
[25-09-16 21:56:32.185][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break
 callback, rk_state: [CONNECTING]
[25-09-16 21:56:32.474][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:32.475][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:34.476][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:34.476][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:
3 isCharge:1 energy:100 capcity:4156mV curent:-43nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared
:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1
 isGpySync:0}
[25-09-16 21:56:36.218][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter
 callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-16 21:56:36.219][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break
 callback, rk_state: [IDLE]
[25-09-16 21:56:36.477][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:36.478][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:38.477][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:38.478][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:40.478][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:40.479][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:42.479][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:42.480][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "251",
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
    "time": 1758031003,
    "version": "1.5.1"
}

[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:
3 isCharge:1 energy:100 capcity:4158mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared
:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1
 isGpySync:0}
[25-09-16 21:56:44.481][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:44.481][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:46.482][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:46.483][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:48.483][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:48.483][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:50.311][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter
 callback, rk_state: [CONNECTING]
[NETWK] CB WIFI STATUS : 1
[25-09-16 21:56:50.311][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break
 callback, rk_state: [CONNECTING]
[25-09-16 21:56:50.484][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:50.484][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:
3 isCharge:1 energy:100 capcity:4158mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared
:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1
 isGpySync:0}
[25-09-16 21:56:52.484][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:52.485][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:54.339][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter
 callback, rk_state: [DISCONNECTED]
[NETWK] CB WIFI STATUS : 5
[NETWK] Update Network Status (2(NSTA_WIFI) value:0)
=== WPA Network List (count: 1) ===
Index: 0
  ID:     0
  SSID:   Libawall-5G
  BSSID:  any
  FLAGS:        [TEMP-DISABLED]
-----------------------------------
[25-09-16 21:56:54.348][rk_wifi_c_][print_saved_i 117][thread2672][I] Total saved networks: 1
[25-09-16 21:56:54.348][rk_wifi_c_][print_saved_i 127][thread2672][I]   [#0] SSID: 'Libawall-5G', BSSID: 'any'
, State: '[ENABLED]'
[25-09-16 21:56:54.348][rk_wifi_c_][Mock_RK_WifiG 135][thread2672][I] [exit] RK_WifiGetSavedInfo: [0]
[25-09-16 21:56:54.348][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break
 callback, rk_state: [DISCONNECTED]
[25-09-16 21:56:54.486][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:54.487][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:56:55.355][wifi_mng_w][operator()   1073][thread2672][I] [on loop] start_polling_status_rk, enter
 callback, rk_state: [IDLE]
[NETWK] CB WIFI STATUS : 0
[25-09-16 21:56:55.355][wifi_mng_w][operator()   1075][thread2672][I] [on loop] start_polling_status_rk, break
 callback, rk_state: [IDLE]
[25-09-16 21:56:56.486][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:56.487][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "252",
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
    "time": 1758031018,
    "version": "1.5.1"
}

[25-09-16 21:56:58.487][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:56:58.488][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:
3 isCharge:1 energy:100 capcity:4158mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared
:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1
 isGpySync:0}
[25-09-16 21:57:00.487][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:00.488][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:57:02.489][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:02.490][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:57:04.490][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:04.491][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:57:06.491][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:06.491][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:
3 isCharge:1 energy:100 capcity:4158mV curent:-13nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared
:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1
 isGpySync:0}
[25-09-16 21:57:08.492][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:08.492][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:57:10.492][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:10.493][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:57:12.494][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:12.494][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[MQTT] Publist Compeletet ret[0] (/sys/yda/YDAS250701000005/event/property/post) Msg:{
    "id": "253",
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
    "time": 1758031033,
    "version": "1.5.1"
}

[25-09-16 21:57:14.494][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:14.495][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[SYS] Update Status: {isLogin:1(idy:4) operation:0 is4G_WifiLink:[1 sig:0, 0 sig:0] isDeviceLock:0 mqttStatus:
3 isCharge:1 energy:100 capcity:4158mV curent:-12nA isLessDisk:0 remainSize:23549(less:200)MB isLow:0 infrared
:0 isStampLock:0 useNetwork:1 fingeOperation:-4 isStampLock:0 ugResult:0 upperTest:0 jiaTurnOff:0 cloudVideo:1
 isGpySync:0}
[25-09-16 21:57:16.495][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:16.496][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:57:18.496][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:18.497][bt_ble_rpc][operator()    212][thread2668][I] [BtBleRpcClient] [req_rsp] BLE_CLIENT_HE
ARTBEAT, rsp=0
[25-09-16 21:57:20.497][bt_ble_rpc][operator()     92][thread2664][D] [BtBleRpcServer] receive BLE_CLIENT_HEAR
TBEAT
[25-09-16 21:57:20.498][bt_ble_rpc][operator()    


系统调用卡在
:56:28.035886 read(15, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0"...,
 68) = 68 <0.000225>
21:56:28.037955 write(1, "\33[36m[25-09-16 21:56:28.033][my_"..., 253) = 253 <0.000598>
21:56:28.040042 write(1, "\33[m\n", 4)  = 4 <0.000338>
21:56:28.042924 write(1, "\33[36m[25-09-16 21:56:28.042][my_"..., 259) = 259 <0.000560>
21:56:28.045159 write(1, "\33[m\n", 4)  = 4 <0.000279>
21:56:28.049765 write(1, "\33[36m[25-09-16 21:56:28.049][my_"..., 247) = 247 <0.000514>
21:56:28.052629 write(1, "\33[m\n", 4)  = 4 <0.000523>
21:56:28.056209 write(1, "enter rkbt_write_cb, data: />|46"..., 215) = 215 <0.000548>
21:56:28.058724 write(1, "[rkbt_write_cb] offset: 0, len: "..., 35) = 35 <0.000547>
21:56:28.061956 write(1, "Data: 2F 3E 7C 34 36 7C 57 49 46"..., 145) = 145 <0.000546>
21:56:28.066821 write(1, "\33[32m[25-09-16 21:56:28.066][bt_"..., 343) = 343 <0.000385>
21:56:28.078717 sendmsg(6, {msg_name=NULL, msg_namelen=0, msg_iov=[{iov_base="q\0\0\0", iov_len=4}, {iov_bas
e="i\0\0\0\22\0ble_client_receive\5\1$00009"..., iov_len=113}], msg_iovlen=2, msg_controllen=0, msg_flags=0}
, MSG_NOSIGNAL) = 117 <0.000232>
21:56:28.104719 write(1, "\33[32m[25-09-16 21:56:28.104][bt_"..., 176) = 176 <0.000331>
21:56:28.107360 write(1, "\33[36m[25-09-16 21:56:28.107][my_"..., 251) = 251 <0.000359>
21:56:28.110793 write(1, "\33[m\n", 4)  = 4 <0.000060>
21:56:28.112954 epoll_wait(9, [{EPOLLOUT, {u32=983752, u64=983752}}], 10, -1) = 1 <0.000037>
21:56:28.116385 write(1, "\33[36m[25-09-16 21:56:28.113][my_"..., 252) = 252 <0.000614>
21:56:28.125657 write(1, "\33[m\n", 4)  = 4 <0.000420>
21:56:28.134478 write(1, "\33[36m[25-09-16 21:56:28.132][my_"..., 166) = 166 <0.000466>
21:56:28.137041 write(1, "\33[m\n", 4)  = 4 <0.000322>
21:56:28.144940 epoll_wait(9, [{EPOLLOUT, {u32=983752, u64=983752}}], 10, -1) = 1 <0.000037>
21:56:28.145730 epoll_wait(9,



```
