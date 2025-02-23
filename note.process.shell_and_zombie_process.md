---
id: s1qhuvzrsuye6euzb4h80n0
title: Shell_and_zombie_process
desc: ''
updated: 1727078753141
created: 1727072836541
---

#### 场景复现
父进程a, fork子进程b, 子进程b通过exec来执行/bin/sh,来执行目标命令，命令中包含了目标程序，目标程序进入main函数则处sleep中;
当父进程根据子进程pid进行，kill(in_pid, SIGKILL)时，子进程b被杀死退出，但是执行命令的子进程b的子进程c并没有立刻退出，还在sleep中
此时杀死父进程以后，进zeze的父进程变为了init进程, 即进程c变孤儿进程

**僵尸进程 是进程已经结束，死了，但没有人收尸没有棺材，所以成僵尸**
**通常父进程由于代码逻辑阻塞，导致子进程即使结束了也并没有被回收**
**孤儿进程 是父进程结束，子进程则变成孤儿被init托管时，init会在短暂时间窗口后进行处理，回收**

类似blog: https://blog.csdn.net/JiMoKuangXiangQu/article/details/128996150

```bash
root@j7-evm:/opt_ping/app/device_server# ps aux | grep TEST
root      348274  0.0  0.1  10880  4160 pts/0    S    14:40   0:00 ./TESTBED_PLATFORM testbed_conn 1 --ws_client_id=1508136689993138176 --ws_server_url=ws://localhost:4399
root      348288  0.0  0.0   3328   960 pts/0    S+   14:40   0:00 grep --color=auto TEST
root@j7-evm:/opt_ping/app/device_server# ps -ef -o pid,ppid,comm
    PID    PPID COMMAND
  47273   47269 sh
   2003    1994 sh
 348292    2003  \_ ps
 348274       1 TESTBED_PLATFOR
  27650       1 login
  27654   27650  \_ sh
    280       1 login
    945     280  \_ sh
    279       1 agetty

```
如果直接发送SIGINT，并没有任何反应
所以具体问题是: 当对进程b发出kill信号时，进程b结束，当进程c变为了孤儿进程，并没有受到信号影响
猜测1: /bin/sh 的子进程c的信号被忽略，导致最外层的爷爷进程发出的kill没有进入到孙子进程，仅仅是父进程b收到了信号
猜测2: 直接对父进程进行sigkill, 导致父进程来不及执行处理子进程的程序
猜测2: kill信号对sleep行为不能起作用

需求方案:
能否杀死一个进程和其所有的子进程？
1.通过进程组进行控制: 在fork子进程以后，执行exec之前，将子进程加入新的进程组，在进行kill时，对子进程所在进程组进行kill，very perfect

##### /bin/sh 的进程和信号处理行为
信号能否传递，父进程接收到信号，是否会传递到子进程?
信号并不能传递，但是信号的处理设置会继承，但是进程可以编写信号处理来对信号进行转发, 实现信号的传递

#### 进程组和信号组
```bash
int setpgid(pid_t pid, pid_t pgid); //将某一个进程加入一个进程组
int setpgrp(void); //直接将当前进程加入新的进程组
```