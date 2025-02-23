---
id: jxz896hzyz1gm7m6bh7d77e
title: Coredump_and_gdb
desc: ''
updated: 1737039257740
created: 1726727965197
---

## refe
- [github: linux 应用调试方法](https://gist.github.com/carloscn/4037f1ffd881e8eac29e8511e6ca1431)

## 查看二进制文件的内存布局
```bash
$  size [bin-file]
   data     bss     dec     hex
   512      256    768     300
```

#### 直接启动gdb调试
```bash
gdb --args ./new_service service service_switch --open_objective=true --open_i30=true --open_device_ping=true

run

bt
```

### 程序默认启动就会进入到系统调用，而导致阻塞无法使用gdb命令
1. 直接使用start命令启动程序，并定向到第一行，然后再设置gdb命令
2. 装载后再使用

### 控制程序执行的命令
- 启动程序
  - `run/r` 开始执行，直到断点和
  - `start` 开始，并停留在第一行
- 一步到位
  - `continue/c` 继续运行，知道下一个断点或者观察点
- 单步执行
  - `next/n` 下一行代码，但不进入函数内部
  - `step/s` 下一行代码，如果是函数，会进入内部
  - `finish` 把当前函数执行完返回
- 指定位置
  - `until`
  - `advance` 
  - `jump 50` 跳到指定行，跳过中间代码

### 断点处理
- `break <location>` 普通断点
- `break <location> if cond` 条件断点
- `tbreak <location>` 一次性断点
- `rbreak <regex>` 正则断点
- `ignore 1 5` 忽略断点1 前五次的触发
- `disable 2` 禁用断点2
- `enable 2` 启用断点2
- `delete 3` 删除断点3
- `watch [value-name]` 观察断点

### 查看信息
- `list` 罗列源代码
- `info` 罗列全局或者结构信息: 所有变量，线程等
  - `info threads`
  - `info program`
  - `info breakpoints`
  - `info source`
  - `info frame` 查看当前栈帧的具体信息
    - `info frame <target_level>` 查看目标栈帧的具体信息 
      ```bash
      ```
- `print` 查看具体变量以及函数信息
- `bt` backtrace 查看当前线程的call stack，以及栈帧
  - `bt full` 打印栈信息的同时，每一个栈帧的局部变量都会打印出来
  - `down` `up` 打印向下或者向上一个栈帧

### 栈帧的内存偏移计算和栈帧内存的内容查看
- `p/d <address> - <address>` 十进制方式查看偏移
- `p/x <address> - <addresss>` 16进制方式查看偏移

### 查看内存状况
- `x/[count][format][unit] address` 
count：表示要查看的数量（默认为 1）。
format：指定显示的格式：
x：十六进制（默认格式）。
d：有符号十进制。
u：无符号十进制。
o：八进制。
t：二进制。
f：浮点数。
c：字符。
s：字符串。
i：汇编指令。
unit：每次查看的内存单元大小：
b：字节（1 字节）。
h：半字（2 字节）。
w：字（4 字节）。
g：巨字（8 字节）。
address：内存地址，可以是一个变量、表达式或具体的地址。


### 查看变量
- 如何查看数组的字符串，使用p? 直接使用 `p <var>`
- 如何查看表达式的值，比如查看sizeof(argv[2])的? 可以直接使用 `p sizeof(argv[2])`
  - sizeof()
- 栈帧的查看，切换，与变量的查看: 首先 `info frame <num>` 是查看栈帧，不是切换栈帧，同理 `info threads` 也是，
  切换需要使用 `frame <num>` 或者 `down` `up`
- 如何一次性查看多个变量 `p <v1>, <v2>`

### 想要观察循环中的值变化
```bash
31      void sanitize(char* str) {
32          char* src, *dest;
33          for (src = dest = str;*src;src++)
34              if (*src == '/' || isalnum(*src))
35                  *dest++ = *src;
36          *dest = '\0';
37          return;
38      }
在这段函数中，我需要观察 n次循环以后，dest和src的值变化
```
- `ignore <break_id> <ignore time>`

### 设置变量值


### 地址映射的探索
通过`info frame` 和 `bt` `info registers sp` `info threads` 命令可以查看到调用函数地址以及栈和栈帧地址，通过`info proc mappings`可以查看程序的内存映射，以及通过`info file`命令可以查看 对比地址位置是否准确
```bash
(gdb) info files
Symbols from "/home/xjf1127/wsl_workspace/prj/socket_learn/out/linux/rls_server/rls_server".
Native process:
        Using the running image of child Thread 0x7ffff7a583c0 (LWP 80656).
        While running this, GDB does not access memory from...
Local exec file:
        `/home/xjf1127/wsl_workspace/prj/socket_learn/out/linux/rls_server/rls_server', file type elf64-x86-64.
        Entry point: 0x5555555552c0
        0x0000555555554318 - 0x0000555555554334 is .interp
        0x0000555555554338 - 0x0000555555554368 is .note.gnu.property
        0x0000555555554368 - 0x000055555555438c is .note.gnu.build-id
        0x000055555555438c - 0x00005555555543ac is .note.ABI-tag
        0x00005555555543b0 - 0x00005555555543d4 is .gnu.hash
        0x00005555555543d8 - 0x0000555555554660 is .dynsym
        0x0000555555554660 - 0x00005555555547b8 is .dynstr
        0x00005555555547b8 - 0x00005555555547ee is .gnu.version
        0x00005555555547f0 - 0x0000555555554840 is .gnu.version_r
        0x0000555555554840 - 0x0000555555554930 is .rela.dyn
        0x0000555555554930 - 0x0000555555554b10 is .rela.plt
        0x0000555555555000 - 0x000055555555501b is .init
        0x0000555555555020 - 0x0000555555555170 is .plt
        0x0000555555555170 - 0x0000555555555180 is .plt.got
        0x0000555555555180 - 0x00005555555552c0 is .plt.sec
        0x00005555555552c0 - 0x00005555555557d4 is .text
        0x00005555555557d4 - 0x00005555555557e1 is .fini
        0x0000555555556000 - 0x0000555555556063 is .rodata
        0x0000555555556064 - 0x00005555555560b0 is .eh_frame_hdr
        0x00005555555560b0 - 0x00005555555561b8 is .eh_frame
        0x0000555555557d00 - 0x0000555555557d10 is .init_array
        0x0000555555557d10 - 0x0000555555557d18 is .fini_array
        0x0000555555557d18 - 0x0000555555557f18 is .dynamic
        0x0000555555557f18 - 0x0000555555558000 is .got
        0x0000555555558000 - 0x0000555555558010 is .data
        0x0000555555558010 - 0x0000555555558018 is .bss
        0x00007ffff7fc32a8 - 0x00007ffff7fc32c8 is .note.gnu.property in /lib64/ld-linux-x86-64.so.2

(gdb) info proc mappings
process 80656
Mapped address spaces:

          Start Addr           End Addr       Size     Offset  Perms  objfile
      0x555555554000     0x555555555000     0x1000        0x0  r--p   /home/xjf1127/wsl_workspace/prj/socket_learn/out/linux/rls_server/rls_server
      0x555555555000     0x555555556000     0x1000     0x1000  r-xp   /home/xjf1127/wsl_workspace/prj/socket_learn/out/linux/rls_server/rls_server
      0x555555556000     0x555555557000     0x1000     0x2000  r--p   /home/xjf1127/wsl_workspace/prj/socket_learn/out/linux/rls_server/rls_server
      0x555555557000     0x555555558000     0x1000     0x2000  r--p   /home/xjf1127/wsl_workspace/prj/socket_learn/out/linux/rls_server/rls_server
      0x555555558000     0x555555559000     0x1000     0x3000  rw-p   /home/xjf1127/wsl_workspace/prj/socket_learn/out/linux/rls_server/rls_server
      0x555555559000     0x55555557a000    0x21000        0x0  rw-p   [heap]
      0x7ffff7a55000     0x7ffff7a59000     0x4000        0x0  rw-p   
      0x7ffff7a59000     0x7ffff7a5c000     0x3000        0x0  r--p   /usr/lib/x86_64-linux-gnu/libgcc_s.so.1
      0x7ffff7a5c000     0x7ffff7a73000    0x17000     0x3000  r-xp   /usr/lib/x86_64-linux-gnu/libgcc_s.so.1
      0x7ffff7a73000     0x7ffff7a77000     0x4000    0x1a000  r--p   /usr/lib/x86_64-linux-gnu/libgcc_s.so.1
      0x7ffff7a77000     0x7ffff7a78000     0x1000    0x1d000  r--p   /usr/lib/x86_64-linux-gnu/libgcc_s.so.1
      0x7ffff7a78000     0x7ffff7a79000     0x1000    0x1e000  rw-p   /usr/lib/x86_64-linux-gnu/libgcc_s.so.1
      0x7ffff7a79000     0x7ffff7a87000     0xe000        0x0  r--p   /usr/lib/x86_64-linux-gnu/libm.so.6
      0x7ffff7a87000     0x7ffff7b03000    0x7c000     0xe000  r-xp   /usr/lib/x86_64-linux-gnu/libm.so.6
      0x7ffff7b03000     0x7ffff7b5e000    0x5b000    0x8a000  r--p   /usr/lib/x86_64-linux-gnu/libm.so.6
      0x7ffff7b5e000     0x7ffff7b5f000     0x1000    0xe4000  r--p   /usr/lib/x86_64-linux-gnu/libm.so.6
      0x7ffff7b5f000     0x7ffff7b60000     0x1000    0xe5000  rw-p   /usr/lib/x86_64-linux-gnu/libm.so.6
      0x7ffff7b60000     0x7ffff7b88000    0x28000        0x0  r--p   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7b88000     0x7ffff7d1d000   0x195000    0x28000  r-xp   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7d1d000     0x7ffff7d75000    0x58000   0x1bd000  r--p   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7d75000     0x7ffff7d76000     0x1000   0x215000  ---p   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7d76000     0x7ffff7d7a000     0x4000   0x215000  r--p   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7d7a000     0x7ffff7d7c000     0x2000   0x219000  rw-p   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7d7c000     0x7ffff7d89000     0xd000        0x0  rw-p   
      0x7ffff7d89000     0x7ffff7e23000    0x9a000        0x0  r--p   /usr/lib/x86_64-linux-gnu/libstdc++.so.6.0.30

(gdb) p &hostname
$23 = (char (*)[256]) 0x7fffffffcfb0
(gdb) p &sock_fd
$24 = (int *) 0x7fffffffcf7c

(gdb) bt
#0  0x00007ffff7c87427 in __libc_accept (fd=3, addr=..., len=0x0) at ../sysdeps/unix/sysv/linux/accept.c:26
#1  0x0000555555555597 in main () at /home/xjf1127/wsl_workspace/prj/socket_learn/src/target/o_rls/rls_server.cpp:86

(gdb) info frame 0
Stack frame at 0x7fffffffcf70:
 rip = 0x7ffff7c87427 in __libc_accept (../sysdeps/unix/sysv/linux/accept.c:26); saved rip = 0x555555555597
 called by frame at 0x7fffffffdcd0
 source language c.
 Arglist at 0x7fffffffcf60, args: fd=3, addr=..., len=0x0
 Locals at 0x7fffffffcf60, Previous frame's sp is 0x7fffffffcf70
 Saved registers:
  rip at 0x7fffffffcf68

(gdb) inf frame 1
Stack frame at 0x7fffffffdcd0:
 rip = 0x555555555597 in main (/home/xjf1127/wsl_workspace/prj/socket_learn/src/target/o_rls/rls_server.cpp:86); 
    saved rip = 0x7ffff7b89d90
 caller of frame at 0x7fffffffcf70
 source language c++.
 Arglist at 0x7fffffffdcc0, args: 
 Locals at 0x7fffffffdcc0, Previous frame's sp is 0x7fffffffdcd0
 Saved registers:
  rbp at 0x7fffffffdcc0, rip at 0x7fffffffdcc8

```

### location express
- `<func_name>:line`
- `<file_name>:line`

### list 源码查看
当程序由于系统调用阻塞时，通过`ctrl+z`停止进程后，再通过gdb命令想查看源代码和进行断点设置时，发现此时源文件定位到系统调用所在文件中了
1. 如何切换源文件，并且进行代码查看和断点设置? 是否有先罗列自定义源代码文件的能力?
2. 如何继续运行已经stop的程序?
通过 `list <location>`可以直接切换位置，如何切换回当前栈帧对应的源文件位置, 通过 `info frame <id>`先切换栈帧，然后再用`list`命令即可
3. 如何查看当前代码运行位置 前后一定区间的源代码, 而不是指定具体行数? `list <-n>,<+n>`


### 多线程如何切换不同线程的栈帧
- `info threads` 首先使用 该命令罗列进程所有线程以及栈顶函数
- `thread <thread_id>` 切换调试线程
- `frame <frame_id>` 切换线程堆栈的栈帧

#### thread block 调试
```bash
[24-09-22 21:14:51.683][process_mn][join_pid      217][thread28][E]  system() call, cmd subprocess abnormally exited, signal number:9
[24-09-22 21:14:51.683][offline_se][start_server  215][thread28][I] out process mng join
^C^[^C
Thread 1 "testbed_server" received signal SIGINT, Interrupt.
0x0000fffff7f6da00 in pthread_cond_destroy () from /lib/libpthread.so.0
(gdb) info threads
  Id   Target Id                   Frame
* 1    LWP 285017 "testbed_server" 0x0000fffff7f6da00 in pthread_cond_destroy () from /lib/libpthread.so.0
  2    LWP 285020 "testbed_server" 0x0000fffff7f6dfb4 in pthread_cond_wait () from /lib/libpthread.so.0
(gdb) bt
#0  0x0000fffff7f6da00 in pthread_cond_destroy () from /lib/libpthread.so.0
#1  0x000000000043b8e4 in SafeQueue<TestbedConn::Material>::~SafeQueue() ()
#2  0x00000000004230d0 in TestbedProcessMng::~TestbedProcessMng() ()
#3  0x0000000000410528 in start_server() ()
#4  0x0000000000410be0 in main ()
```

启动gdb运行程序以后，阻塞在[start_server  215][thread28][I] out process mng join, 
1. ctrl c 中断程序运行
2. info threads 查看线程信息
3. bt 查看当前回溯栈
最终发现是SafeQueue<TestbedConn::Material>::~SafeQueue() () 释放资源的时候，内部线程未退出

### 如何跟踪变量



## gdb 加载程序和运行程序分开
```bash
gdb <exe_path>
run <argument1> <arguemnt2>
 ```

## 在加载程序之后但运行程序之前，gdb可以做哪些事情

## gdb的命令输入和程序的标准输入的冲突

## 程序阻塞在系统调用时，gdb如何进行调试

## gdb 调试的原理

## 利用strace查看进程当前正在做什么事情

