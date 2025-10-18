---
id: 4er0i4lmauiantrcgwk3ixh
title: Ble_problem
desc: ''
updated: 1758029731695
created: 1757490475053
---


## bluez question io debug

1. # 跟踪整个进程及所有线程
strace -f -p 576 -e trace=epoll_ctl,epoll_wait,write,read

strace -p 586 -e trace=epoll_ctl,epoll_wait,write,read

strace -p <tid> -e trace=epoll_ctl,epoll_wait,write,read

2. strace -p 1504 -e trace=epoll_ctl,epoll_wait
strace -p 1504 -e trace=epoll_ctl,epoll_wait, wirtev, read


```bash
root@Rockchip:/oem/pidfile$ ls /proc/2605/task/
596   2687  2677  2649  2629  2626  2623  2620  2617  2605
2966  2679  2663  2644  2628  2625  2622  2619  2616
2704  2678  2650  2630  2627  2624  2621  2618  2615

root@Rockchip:/oem$ cat pidfile/ydn-rk.pid
root@Rockchip:/oem$ cat pidfile/ydn-rk.pid
3644
root@Rockchip:/oem$ ls /proc/3644/task/
4006  3717  3707  3683  3667  3664  3661  3658  3655  3644
3742  3716  3701  3669  3666  3663  3660  3657  3654
3725  3715  3700  3668  3665  3662  3659  3656  3651

strace -p 3725 -e trace=epoll_ctl,epoll_wait,write,read

root@Rockchip:/oem$ free -h
             total       used       free     shared    buffers     cached
Mem:        184640     181040       3600        416       6044     138008
-/+ buffers/cache:      36988     147652
Swap:            0          0          0

```


```bash
开始链接
[{EPOLLIN, {u32=3380880, u64=3380880}}], 10, -1) = 1
read(44, "\22\r\0/>|46|WIFISETTINGV2|1~1~Libawall-5G~11223344|#", 185) = 49
read(41, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33\0\0\0\21\0\0\0\3\0\0\0\f\200\0\0\0\310\\\1\200\310\372'p\311\325\16\200\312\333Z\360\36\2726\r", 68) = 68
read(41, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0\0\0\0U\223-\231\0\0\0\32\0\0\0\0XhF\232\0\0\0\33\0\0\0\0\0\0\nCST-8\n", 68) = 68
epoll_ctl(10, EPOLL_CTL_ADD, 41, {EPOLLIN|EPOLLONESHOT, {u32=5061176, u64=5061176}}) = 0
epoll_ctl(10, EPOLL_CTL_DEL, 41, NULL)  = 0
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLOUT|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\23", iov_len=1}], 1) = 1
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|30|WIFISETTINGV2|OK~0|1~connect|#", iov_len=39}], 1) = 39
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLIN, {u32=3380880, u64=3380880}}], 10, -1) = 1
read(44, "\22\r\0/>|20|WIFILISTV2|~|#", 185) = 23
epoll_ctl(10, EPOLL_CTL_ADD, 41, {EPOLLIN|EPOLLONESHOT, {u32=3772792, u64=3772792}}) = 0
epoll_ctl(10, EPOLL_CTL_DEL, 41, NULL)  = 0
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLOUT|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\23", iov_len=1}], 1) = 1
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#", iov_len=42}], 1) = 42
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|-46~1~guest~0~0|#", iov_len=42}], 1) = 42
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|39|WIFILISTV2|OK~0|-46~1~Libawall-5G~1~1|#", iov_len=48}], 1) = 48
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|34|WIFILISTV2|OK~0|-48~1~device~0~0|#", iov_len=43}], 1) = 43
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|34|WIFILISTV2|OK~0|-50~1~zzwifi~0~0|#", iov_len=43}], 1) = 43
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|40|WIFILISTV2|OK~0|-54~1~ChinaNet-TNT~0~0|#", iov_len=49}], 1) = 49
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|-55~1~ceshi~0~0|#", iov_len=42}], 1) = 42
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|54|WIFILISTV2|OK~0|-58~1~DIRECT-54-HP M232 LaserJet~0~0|#", iov_len=63}], 1) = 63
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|36|WIFILISTV2|OK~0|-61~1~Libawall~0~0|#", iov_len=45}], 1) = 45
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|40|WIFILISTV2|OK~0|-67~1~TP-LINK_F41B~0~0|#", iov_len=49}], 1) = 49
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|-70~1~SLLWC~0~0|#", iov_len=42}], 1) = 42
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10,

链链接成功并且返回wifilsit<===

开始忽略
[{EPOLLIN, {u32=3380880, u64=3380880}}], 10, -1) = 1
read(44, "\22\r\0/>|36|WIFISETTINGV2|2~Libawall-5G~|#", 185) = 39
read(41, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\33\0\0\0\21\0\0\0\3\0\0\0\f\200\0\0\0\310\\\1\200\310\372'p\311\325\16\200\312\333Z\360\36\2726\r", 68) = 68
read(41, "\267\33\226\0\0\0\27\0\0\0\0I\\\7\227\0\0\0\30\0\0\0\0O\357\223\30\0\0\0\31\0\0\0\0U\223-\231\0\0\0\32\0\0\0\0XhF\232\0\0\0\33\0\0\0\0\0\0\nCST-8\n", 68) = 68
epoll_ctl(10, EPOLL_CTL_ADD, 41, {EPOLLIN|EPOLLONESHOT, {u32=3986256, u64=3986256}}) = 0
epoll_ctl(10, EPOLL_CTL_DEL, 41, NULL)  = 0
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLOUT|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\23", iov_len=1}], 1) = 1
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|33|WIFISETTINGV2|OK~0|5~disconnect|#", iov_len=42}], 1) = 42
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLIN, {u32=3380880, u64=3380880}}], 10, -1) = 1
read(44, "\22\r\0/>|20|WIFILISTV2|~|#", 185) = 23
epoll_ctl(10, EPOLL_CTL_ADD, 41, {EPOLLIN|EPOLLONESHOT, {u32=3752520, u64=3752520}}) = 0
epoll_ctl(10, EPOLL_CTL_DEL, 41, NULL)  = 0
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLOUT|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\23", iov_len=1}], 1) = 1
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|0~1~unknown~0~0|#", iov_len=42}], 1) = 42
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0

epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|-46~1~guest~0~0|#", iov_len=42}], 1) = 42
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0

epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|39|WIFILISTV2|OK~0|-46~1~Libawall-5G~0~0|#", iov_len=48}], 1) = 48
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0

epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|34|WIFILISTV2|OK~0|-48~1~device~0~0|#", iov_len=43}], 1) = 43
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|34|WIFILISTV2|OK~0|-50~1~zzwifi~0~0|#", iov_len=43}], 1) = 43
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|40|WIFILISTV2|OK~0|-54~1~ChinaNet-TNT~0~0|#", iov_len=49}], 1) = 49
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|-55~1~ceshi~0~0|#", iov_len=42}], 1) = 42
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|54|WIFILISTV2|OK~0|-58~1~DIRECT-54-HP M232 LaserJet~0~0|#", iov_len=63}], 1) = 63
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|36|WIFILISTV2|OK~0|-61~1~Libawall~0~0|#", iov_len=45}], 1) = 45
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|40|WIFILISTV2|OK~0|-67~1~TP-LINK_F41B~0~0|#", iov_len=49}], 1) = 49
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
writev(44, [{iov_base="\33\17\0/>|33|WIFILISTV2|OK~0|-70~1~SLLWC~0~0|#", iov_len=42}], 1) = 42
epoll_wait(10, [{EPOLLOUT, {u32=3380880, u64=3380880}}], 10, -1) = 1
epoll_ctl(10, EPOLL_CTL_MOD, 44, {EPOLLIN|EPOLLRDHUP, {u32=3380880, u64=3380880}}) = 0
epoll_wait(10,

忽略成功并且wifilsit也成功发送<===

```
