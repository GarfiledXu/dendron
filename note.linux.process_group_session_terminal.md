---
id: cozpmze3bewz5lo0u7xmuvi
title: Process_group_session_terminal
desc: ''
updated: 1757995166983
created: 1757986487038
---

## 进程 进程组 会话 终端 作业 作业控制

1. 每一种属性的作用是什么?
   1. 进程
   2. 进程组：对进程批量控制，提供组级别的信号发送，例如shell管理一条完整的pipeline，以便一次性进行SIGINT
2. 每种抽象的来源背景? 是内核实现，还是其他管理程序设计的概念?
   1. 进程 内核数据结构
   2. 进程组 内核数据结构
   3. 会话
3. 是否是必要属性
   1. 必要则默认属性值是什么，哪些行为决定默认属性值，哪些行为改变属性值
   2. 非必要则哪些行为会附加属性，哪些行为会剥离属性
4. 相关工具命令行以及系统调用
5. 一些常见终端和命令行为的对应的各种数据结构关系
   1. ssh 登录执行命令
   2. adb shell 登录执行命令
   3. 系统启动时，sh脚本内执行的命令
6. 进程fork子进程时，通过属性控制实现的进程行为控制
7. 试验

## refe

[blog1](https://www.cnblogs.com/liqiuhao/p/8087539.html)

## gpt answer

```bash
进程 / 进程组 / 会话 / 终端 / 作业 / 作业控制 — 详尽笔记

下面把这些概念逐项讲清楚：作用、来源（内核或用户态/标准）、是否必要、默认值与变更方式、相关系统调用/命令、以及常见场景（ssh/adb/启动脚本/子进程继承等）。文中同时给出行为示例和常见操作命令/系统调用，便于查阅与实验。

一、核心概念一览（先速览）

进程（process）：内核的基本执行单位，必然存在。内核数据结构（Linux 中 task_struct）。

进程组（process group, PGID）：内核维护的标识，把一组相关进程（通常是同一作业/管道）组织在一起，便于信号一起发送（内核级）。

会话（session, SID）：内核维护的更高层次聚合，包含一个或多个进程组，通常由登录会话或服务创建（内核级）；每个会话可有一个“会话领导(process session leader)”。

控制终端 / 控制 tty（controlling terminal）：终端驱动层概念（由内核维护），会话可以有一个控制终端；前台进程组与后台进程组由终端驱动强制读写 / 信号行为。

作业（job）：**壳（shell）**的抽象，表示用户层面“一组相关进程（通常是一个 pipeline）”，并由 shell 管理（用户态、非内核强制）。

作业控制（job control）：shell 提供的功能（fg/bg/jobs 等），通过内核提供的会话、进程组与终端接口实现（用户态 + 内核协作）。

二、每一种属性的作用（详细）
1. 进程（Process）

作用：运行代码的实体（地址空间 + CPU 状态 + 文件描述符等）。

来源：内核（task_struct）实现。

是否必要：必须存在（每个执行的程序至少是一个进程）。

改变行为：通过 fork() 产生子进程，execve() 加载新程序映像。

常见系统调用：fork(), vfork(), clone(), execve(), exit()。

命令/工具：ps, top, pstree, pgrep。

2. 进程组（Process Group，PGID）

作用：

提供“组级别”的信号发送（例如终端产生的中断信号发送到前台进程组）。

便于 shell 管理一个 pipeline（把 pipeline 内所有进程放到同一个 PGID，以便一次性 SIGINT 等）。

来源：内核实现，但由进程（或 shell）通过系统调用管理。

是否必要：不是强制“必须有一个非默认值”的属性，但每个进程都有一个 PGID（默认 PGID = PID，除非被 setpgid()/setpgrp() 改变）。

默认值与改变：

新 fork 出来的子进程默认继承父进程的 PGID（因此一开始 PGID = 父 PGID）。

可以通过 setpgid(pid, pgid) 或 setpgrp() 改变（setpgid(0,0) 将进程变为以自己 PID 为 PGID）。

常见系统调用：getpgid(), getpgrp(), setpgid(), setpgrp()（POSIX）。kill(-pgid, sig) 可以向整个进程组发送信号。

命令/工具：ps -o pid,pgid,sid,tty,cmd，pgrep -g PGID，kill -SIGTERM -PGID（注意 kill 的负号语义）。

3. 会话（Session，SID）

作用：

把若干进程组归到同一个登陆会话（例如 ssh 登录的所有进程属于同一会话）。

与“控制终端”关联：会话可有一个控制终端（控制终端是会话和终端驱动之间的绑定）。

来源：内核实现（但行为受 POSIX 规范）。

是否必要：每个进程都属于某个会话；会话本身是内核级实体（会话 ID = 会话领导的 PID）。

默认/改变：

新进程继承父进程的 SID。

调用 setsid()（必须是进程组的领导）会创建一个新的会话并成为会话领导（同时成为一个新进程组的领导，并与原控制终端分离）。

系统调用：getsid(), setsid()。

常见场景：登录 shell 创建会话；守护进程典型做法：fork() + setsid() + fork()（双重 fork）来脱离控制终端并避免重新获得控制终端。

4. 控制终端（Controlling Terminal）/ 前台/后台进程组

作用：

终端驱动维护哪个会话拥有该 tty（控制终端），并记录哪个进程组是前台进程组（可读取终端输入）。

终端在需要时向前台进程组发送信号（例如 Ctrl-C -> SIGINT，Ctrl-Z -> SIGTSTP）。

如果后台进程尝试读终端 -> kernel sends SIGTTIN；写终端 -> SIGTTOU（除非忽略）。

来源：终端驱动（内核） + POSIX 约定。

是否必要：交互式会话需要控制终端；守护进程和非交互进程通常没有控制终端。

系统调用/接口：tcgetpgrp() / tcsetpgrp()（获取/设置终端的前台 PGID），ioctl(TIOCGPGRP/TIOCSPGRP)。

命令/工具：tty, stty, fg, bg, jobs（shell 提供）。

5. 作业（Job） & 作业控制（Job Control）

作用：

作业：shell 层面的概念，代表用户发起的“单位工作”（通常是一条命令或 pipeline）。

作业控制：shell 提供前台/后台切换、暂停/恢复 (fg/bg/jobs)，依赖内核的进程组和控制终端机制。

来源：用户态（shell）实现，由内核的 PGID/SID/tty 機制支撑。

关键点：

shell 在启动 job 时创建新的进程组并把该进程组设置为控制终端的前台（tcsetpgrp），这样终端信号发送会影响到这个 job。

当用户按 Ctrl-Z，内核向前台进程组发送 SIGTSTP，进程收到后停下（如果 shell 支持 job control，会把该进程组放后台）。

命令/工具：jobs, fg, bg, disown（bash）。

三、必要属性、默认值及改动规则（更系统化）
1. 进程

必要：是（每个执行单元都是进程）。

默认值：由 fork()/execve() 推导出（无“默认”属性之外的概念）。

2. 进程组（PGID）

必要：每个进程都有 PGID（内核属性）。

默认：

新 fork 的子进程继承父 PGID。

新进程如果直接由 shell 启动（单个命令），shell 通常为其新建 PG（即把它放到以自己的 PGID 或新 PGID）。

改变：

setpgid(pid, pgid)：把指定进程加入/创建进程组（父子之间通常允许）。

setpgrp()：通常等价 setpgid(0,0)。

shell 在启动 pipeline 时会把 pipeline 的各进程都 setpgid() 为同一 PGID（通常以第一个子进程 PID 为 PGID）。

3. 会话（SID）

必要：每个进程属于某个会话（内核属性）。

默认：继承父会话。

改变：

setsid()：创建新会话（调用进程成为会话领导者和进程组领导），同时脱离当前控制终端（如果有）。

注意：调用 setsid() 的进程必须不是其进程组领导，否则调用会失败。

4. 控制终端

必要：不是必须（守护进程通常没有）。

默认：交互式登录 shell 会在登录时获得控制终端（通常由 getty/login/ssh 分配）。

改变：

setsid() 会使进程脱离控制终端（新会话没有控制终端）。

会话领导打开一个终端设备（例如 /dev/ttyX 或 pty）并使用 ioctl(TIOCSCTTY) 来把它设为控制终端（通常由终端分配程序/登录程序完成）。

四、相关系统调用 & shell 命令/工具（汇总）
常用系统调用

fork(), vfork(), clone()

execve()

exit()

getpid(), getppid()

getpgrp(), setpgid(pid, pgid), setpgrp()（进程组）

getsid(pid), setsid()（会话）

tcgetpgrp(fd), tcsetpgrp(fd, pgid)（终端前台 PG）

ioctl(fd, TIOCGPGRP/TIOCSPGRP)（终端 ioctl）

kill(pid| -pgid, sig)（发送信号到单个进程或进程组）

waitpid() / waitid()（等待子进程）

open() + ioctl(TIOCSCTTY)（使某 tty 成为控制终端，通常登录进程/pty master 做）

常用命令/工具

ps -o pid,ppid,pgid,sid,tty,cmd

pstree

pgrep -g PGID

kill -<sig> <pid>（或 kill -<sig> -<pgid> 给进程组）

shell 内置：jobs, fg, bg, disown, stty, tty

setsid <cmd>（以新会话启动）

nohup <cmd>（忽略 SIGHUP，常配 & 后台运行）

screen, tmux（提供虚拟 tty 并管理 session）

五、几个常见命令行为对应的数据结构情况（逐例说明）
场景 A：用户在物理终端/tty 上交互登录（login -> shell）

getty / login 接受登录，分配一个 tty (/dev/ttyX 或 pty)。

login 启动 shell，shell 成为该会话的会话领导（SID = shell PID），shell 通常获得该 tty 作为控制终端。

shell 启动命令（例如 sleep 100 &）：

shell 为 job 创建新的进程组（setpgid），并在需要时把该 PG 设置为前台或后台（tcsetpgrp）。

前台 job 的 PG 接收键盘产生的信号键（SIGINT、SIGTSTP）。

shell 退出或心跳关闭终端时，shell 会（通常）给其 jobs 发送 SIGHUP，以便子进程知道 tty 断开。

场景 B：ssh 登录执行交互 shell（ssh user host）

**交互式 ssh（默认）：**服务端为会话分配一个 pty（伪终端），sshd 在服务端创建一个会话、fork 出 shell，shell 获得该 pty 作为控制终端。行为类似本地 tty 登录。

非交互 ssh host command（无 -t）：

通常不会分配 pty（除非使用 -t 强制分配）。因此远程命令没有控制终端（没有 tty），也没有前台/后台的终端信号交互。

因为没有控制终端，终端相关的信号/行为不会发生（例如后台 SIGTTIN 等）。

场景 C：adb shell（Android）

adb shell 默认分配一个 pty（交互式）在设备端，从而 shell 获得控制 tty。

若通过 adb 执行单条命令而非交互，会视实现而定是否分配 pty（通常会），但行为与 ssh 类似：是否分配 pty 决定是否有控制终端。

场景 D：系统启动时，init/systemd 启动服务（和 shell 启动脚本）

启动脚本通常在没有交互式 tty 的环境下运行（没有控制终端）。

systemd 启动的服务通常 没有控制终端（除非配置 TTYPath= 等），每个服务进程继承 systemd 的会话或独立（systemd 管理单独 cgroup）。

因为没有控制终端，SIGHUP/tty 信号不会自动由终端产生；shell 的 job-control 也不适用。

六、fork/exec 与这些属性之间的继承与变更（详解）

fork()：

子进程继承父进程的：PGID、SID、控制终端句柄（文件描述符）、signal dispositions（某些被重置的例外情况）等。

子进程在初始时与父进程共享大多数属性，但 getpid() 是不同的。

execve()：

替换进程映像，但不会改变 PID / PGID / SID / 控制终端（除非新程序主动调用 setsid()/setpgid() 等）。

信号处理：若被设置为捕获/忽略等，某些信号处理可能被重置为默认（实现细节：SIGCHLD、SIGURG 等与 exec 的细微点，参考 POSIX）。

通过进程组和会话控制实现的功能示例：

Shell 启动 pipeline（cmd1 | cmd2 | cmd3）：

Shell fork 出多份子进程，shell 使用 setpgid() 把这些子进程都放入同一 PG（通常 PGID = 第一个子进程 PID），然后用 tcsetpgrp() 把该 PG 设为控制终端的前台。这样 ^C 会发送 SIGINT 给整个 pipeline。

守护进程脱离控制终端：

标准做法：fork() -> parent exit -> child setsid()（建立新会话、成为会话领导、与 tty 分离）-> 有时再 fork() 一次以避免将来重新获得控制终端（确保不是会话领导）。

七、信号传播/tty 断开等行为（关键细节）

终端产生的键盘信号（如 Ctrl-C / Ctrl-Z）由终端驱动发送到前台进程组（不是单个进程）。

后台进程试图读 tty -> kernel 给该进程发送 SIGTTIN（默认暂停进程）；写 tty -> SIGTTOU。

终端 hangup（挂断 / 关闭）：

终端关闭或客户端（例如 ssh）断开连接时，终端驱动会产生 hangup。shell 常见的实现：当 shell 退出时，shell 向其作业的进程组发送 SIGHUP，促使它们退出或让守护进程处理（守护进程可用 nohup 或忽略 SIGHUP）。

细节：不同情形下内核/terminal 事件会触发向 foreground/background 的不同处理，shell 通常在自身退出时人为发送 SIGHUP 给其作业（而不是内核直接对 session 的所有进程统一发送）。

八、一些命令行为对应的“数据结构快照”（例）

下面给出使用 ps 查看时常见的信息解释：

$ ps -o pid,ppid,pgid,sid,tty,cmd


PID：进程 ID

PPID：父进程 ID

PGID：进程组 ID（同一 pipeline 或 job 的进程相同）

SID：会话 ID（会话领导的 PID）

TTY：控制终端（若无则显示 ?）

CMD：命令行

实例：

交互 shell：shell 的 SID == PID（会话领导），控制终端是 /dev/pts/X。

后台守护：TTY == ?，可能 SID != PID 或已 setsid() 后仍是会话领导，但没有控制终端。

九、关于 “作业” 概念的起源与定义（更深入）

作业（job） 的概念主要来自 交互式 shell（例如 sh, ksh, bash），为用户提供对“正在运行的一组进程”的管理接口。

其由来：在早期 Unix 中，用户需要在交互式会话中管理多个命令（前台/后台、暂停/恢复），shell 为此维护 job table（记录每个 job 的 PGID、命令文本、状态）。

Job 是用户态的抽象：内核并不知道“job”是什么；内核只管理进程、进程组和会话。Shell 将这些内核概念组合成 job，并提供 jobs/fg/bg 等用户命令实现作业控制。

job-control 的存在依赖于内核提供的 PGID/SID/tcsetpgrp/tcgetpgrp 等机制。

十、进程组 / 会话 / 终端 / 作业 在不同场景下的具体行为（总结式）
登录终端（交互）

会话：shell 为会话领导（SID == shell PID）

控制终端：有 /dev/pts/N

job control：启用（shell 支持），前台 PG 接收键盘信号

ssh（交互式）

如果 sshd 为 session 分配 pty：与登录终端类似（有控制终端）

如果不分配 pty（远程命令）：无控制终端（no job control）

systemd / 启动脚本

启动时多数服务/脚本没有控制终端（TTY == ?），不会受终端产生的信号影响

systemd 管理服务生命周期，通过 SIGTERM/SIGKILL/notify 等方式控制服务

daemonize（守护进程）

常见步骤：

fork() -> 父进程 exit（使子进程在后台）

setsid() -> 创建新会话，脱离控制终端

（可选）再次 fork() -> 避免重新获得控制终端（第二次 fork 后子进程不是会话领导）

chdir("/"), umask(0), 关闭/重定向文件描述符等

十一、常见问题 Q&A（实践角度）

Q1：为什么 nohup 能让程序在退出 shell 后继续运行？
A：nohup 会忽略 SIGHUP（signal(SIGUP, SIG_IGN)），shell 在退出时给作业发送 SIGHUP，因此忽略后程序不会收到该信号而被终止。通常配合 & 使用把进程放后台。

Q2：为什么后台进程写终端会被暂停（SIGTTOU）？
A：终端驱动防止后台进程随意写前台终端（避免输出干扰用户交互）。可以在 shell 中通过 stty -tostop 改变行为或在进程中忽略 SIGTTOU。

Q3：当 ssh 断开时，为什么会话内的进程被杀死？
A：如果 ssh 分配了 pty，断开连接会导致 tty hangup。shell 在收到 hangup/退出时通常会向其作业发送 SIGHUP。如果子进程不忽略 SIGHUP，会被终止。可用 nohup 或 disown 来防止被杀死；或使用 screen/tmux 来通过虚拟 pty 保持会话。

十二、实验/调试小贴士（命令合集）

查看 pid/pgid/sid/tty：
ps -o pid,ppid,pgid,sid,tty,cmd -p <pid>

向进程组发送信号：
kill -TERM -<pgid> （注意 - 放在 pgid 前）

启动新会话：
setsid <cmd>

以新会话后台运行：
nohup <cmd> &

shell job 操作：
jobs, fg %1, bg %1, disown %1

新终端前台 PG：
tcsetpgrp(fd, pgid)（程序内部调用，通常由 shell 完成）

观察 tty：
tty

十三、总结（要点回顾）

进程/进程组/会话/控制终端均为内核或内核驱动（tty）提供的机制，shell（用户态）基于这些机制实现“作业/作业控制”。

进程一定有；进程组和会话也是内核级属性，且每个进程都有 PGID/SID（只是默认可能等于 PID/继承父值）。

作业是 shell 的概念，用来管理一组进程（通常同一 PGID）。

终端（tty）是信号产生与前台/后台限制的关键点；是否有控制终端决定很多行为（SIGHUP、SIGTTIN、SIGTTOU、前台信号分发等）。

理解这些概念的关键是两层：内核层（数据结构 + 系统调用） 与 用户层（shell 的 job table 和策略） 的协作。

如果你愿意，我可以：

给出一份常用 syscalls 示例代码（C/C++）展示如何 setsid()/setpgid()/tcsetpgrp()/fork() 做 daemon 或 job 管理；

给出 实验步骤（一系列命令）让你在终端上验证每一步（例如创建 pipeline，查看 PGID、用 kill -SIGINT -PGID 验证信号行为；演示 nohup/disown/screen 的差别）。

想要哪一种先做？
```

## 查看进程状态

```bash
root@Rockchip:/oem/test$ cat /proc/2815/status
Name:   bt_mock_client
Umask:  0022
State:  S (sleeping)
Tgid:   2811
Ngid:   0
Pid:    2815
PPid:   3111
TracerPid:      0
Uid:    0       0       0       0
Gid:    0       0       0       0
FDSize: 32
Groups:
VmPeak:     4476 kB
VmSize:     4476 kB
VmLck:         0 kB
VmPin:         0 kB
VmHWM:      2008 kB
VmRSS:      2008 kB
RssAnon:             136 kB
RssFile:            1872 kB
RssShmem:              0 kB
VmData:     2216 kB
VmStk:       132 kB
VmExe:       384 kB
VmLib:      1524 kB
VmPTE:         8 kB
VmSwap:        0 kB
CoreDumping:    0
THP_enabled:    0
Threads:        2
SigQ:   0/1440
SigPnd: 0000000000000000
ShdPnd: 0000000000000000
SigBlk: 0000000000000000
SigIgn: 0000000000001000
SigCgt: 0000000180000002
CapInh: 0000000000000000
CapPrm: 000001ffffffffff
CapEff: 000001ffffffffff
CapBnd: 000001ffffffffff
CapAmb: 0000000000000000
NoNewPrivs:     0
Seccomp:        0
Seccomp_filters:        0
Speculation_Store_Bypass:       unknown
Cpus_allowed:   1
Cpus_allowed_list:      0
voluntary_ctxt_switches:        55
nonvoluntary_ctxt_switches:     31

逐行说明

Name:   bt_mock_client            # 进程名称（可执行文件名）
Umask:  0022                      # 文件创建掩码（umask）
State:  S (sleeping)              # 进程状态：S = 可中断睡眠，R=运行，D=不可中断睡眠，Z=僵尸，T=停止
Tgid:   2811                      # 线程组 ID（主线程 PID）
Ngid:   0                          # NUMA 节点 ID（多数系统不使用）
Pid:    2815                      # 当前线程或进程的 PID
PPid:   3111                      # 父进程 PID
TracerPid:      0                  # 如果被 ptrace 追踪，这里显示追踪者 PID，否则 0
Uid:    0       0       0       0  # 用户 ID（真实 / 有效 / saved / filesystem）
Gid:    0       0       0       0  # 组 ID（真实 / 有效 / saved / filesystem）
FDSize: 32                         # 文件描述符表大小上限
Groups:                            # 进程所属的 supplementary group
VmPeak:     4476 kB                # 历史最大虚拟内存
VmSize:     4476 kB                # 当前虚拟内存总量
VmLck:         0 kB                # 被锁定在内存中的字节数（mlock）
VmPin:         0 kB                # 被 pin 的内存（多数系统为 0）
VmHWM:      2008 kB                # 高水位驻留集大小（物理内存最大占用）
VmRSS:      2008 kB                # 驻留集大小（当前物理内存占用）
RssAnon:             136 kB        # 匿名映射占用的物理内存（堆等）
RssFile:            1872 kB        # 文件映射占用的物理内存
RssShmem:              0 kB        # 共享内存占用的物理内存
VmData:     2216 kB                # 数据段大小（堆）
VmStk:       132 kB                # 栈大小
VmExe:       384 kB                # 可执行文件映射大小
VmLib:      1524 kB                # 共享库映射大小
VmPTE:         8 kB                # 页表占用大小
VmSwap:        0 kB                # 交换空间占用
CoreDumping:    0                  # 是否允许生成 core dump（0=否, 1=是）
THP_enabled:    0                  # 是否启用 Transparent Huge Pages
Threads:        2                  # 线程数量
SigQ:   0/1440                     # 信号队列（已排队/最大队列）
SigPnd: 0000000000000000           # 进程挂起的信号掩码
ShdPnd: 0000000000000000           # 共享挂起信号掩码
SigBlk: 0000000000000000           # 被阻塞的信号掩码
SigIgn: 0000000000001000           # 被忽略的信号掩码
SigCgt: 0000000180000002           # 被捕获的信号掩码
CapInh: 0000000000000000           # 继承能力（capabilities）
CapPrm: 000001ffffffffff           # 允许能力
CapEff: 000001ffffffffff           # 当前有效能力
CapBnd: 000001ffffffffff           # 能力边界
CapAmb: 0000000000000000           # Ambient capabilities（环境能力）
NoNewPrivs:     0                  # no_new_privs 安全标志
Seccomp:        0                  # 是否启用了 seccomp 过滤
Seccomp_filters:        0          # seccomp 过滤器数量
Speculation_Store_Bypass:       unknown  # CPU 推测执行缓解状态
Cpus_allowed:   1                  # 允许运行的 CPU 掩码
Cpus_allowed_list:      0          # CPU 亲和性列表
voluntary_ctxt_switches:        55  # 自愿上下文切换次数
nonvoluntary_ctxt_switches:     31  # 非自愿上下文切换次数（被抢占）

```

```bash
1. /proc 文件系统

来源：Linux 内核提供
用途：获取运行时进程和线程信息

/proc/[pid]/status

人类可读格式，包含进程基本信息、状态、内存占用、信号掩码、能力等

/proc/[pid]/stat

单行、空格分隔，包含 PID、父进程、进程状态、CPU 时间、优先级、线程组 ID 等

/proc/[pid]/cmdline

进程启动命令和参数（\0 分隔）

/proc/[pid]/environ

进程环境变量

/proc/[pid]/fd/

当前进程打开的文件描述符列表

/proc/[pid]/task/[tid]/status

线程详细信息，类似进程 status

/proc/[pid]/maps

内存映射信息，显示代码段、堆、共享库

/proc/[pid]/io

I/O 统计

/proc/[pid]/sched

调度信息，包含调度策略、运行时间

/proc/[pid]/cgroup

所属 cgroup 信息

2. 系统调用接口

来源：POSIX / Linux 内核
用途：动态查询和控制进程/线程

getpid(), getppid() — 当前进程 PID / 父进程 PID

gettid() — 当前线程 TID（Linux 特有）

getpgid(pid) / setpgid(pid, pgid) — 进程组管理

getsid(pid) / setsid() — 会话管理

fork() / vfork() / clone() — 创建新进程/线程

waitpid(pid, ...) / waitid() — 等待子进程结束

kill(pid, sig) — 发送信号给进程

pthread_self() / pthread_getschedparam() — 线程相关信息

sched_getscheduler() / sched_getparam() — 获取调度策略和优先级

prctl() — 高级进程控制（名称、能力、dump 限制等）

setrlimit() / getrlimit() — 限制进程资源

3. 工具命令行

来源：用户空间工具（基于系统调用或 /proc）
用途：快速查询、调试、监控

ps — 显示进程状态、PID、PPID、TTY、状态、CPU/内存占用

top / htop — 实时进程资源监控

pstree — 显示进程树结构

pgrep / pidof — 查询 PID

lsof -p [pid] — 列出进程打开的文件

strace -p [pid] — 系统调用跟踪

pmap [pid] — 内存映射

cat /proc/... — 手动查看内核导出的进程数据

4. 内核数据结构

来源：内核 C 结构体
用途：核心存储、调度和信号处理

task_struct

Linux 内核的核心进程数据结构

包含 PID、父进程指针、线程组、状态、信号信息、调度信息、内存管理 info 等

thread_info

线程信息，通常嵌入 task_struct

signal_struct

线程组共享的信号信息

mm_struct

内存管理信息（虚拟地址空间、映射表）

files_struct

打开的文件描述符表

cred / capability

用户/组权限与能力

5. 用途分类

状态与生命周期管理

PID / TID、PPID、PGID、SID、线程数、状态（R/S/D/Z/T）

接口：getpid(), fork(), waitpid(), /proc/[pid]/status

信号与权限管理

信号掩码、阻塞、忽略、捕获

能力（capabilities）

接口：kill(), sigprocmask(), prctl()

资源与性能监控

内存占用、CPU 时间、IO

接口：/proc/[pid]/stat, /proc/[pid]/io, getrusage()

线程调度和同步

调度策略、优先级、上下文切换

接口：sched_getparam(), pthread_* 系列

进程组和作业控制

进程组、会话、终端、作业

接口：setpgid(), setsid(), tcgetpgrp(), shell 内建 job control
```

进程fd分析

```bash
root@Rockchip:/oem/test$ ls -l /proc/3860/fd/
lrwx------    1 root     root            64 6 -> socket:[2681085]
lrwx------    1 root     root            64 5 -> socket:[2681082]
lrwx------    1 root     root            64 4 -> anon_inode:[eventpoll]
lrwx------    1 root     root            64 3 -> anon_inode:[eventfd]
lrwx------    1 root     root            64 2 -> /dev/pts/0
lrwx------    1 root     root            64 1 -> /dev/pts/0
lrwx------    1 root     root            64 0 -> /dev/pts/0

```
