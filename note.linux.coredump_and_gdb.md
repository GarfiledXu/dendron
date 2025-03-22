---
id: jxz896hzyz1gm7m6bh7d77e
title: Coredump_and_gdb
desc: ''
updated: 1742291644959
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





## 混乱时，确定当前断点位置

```bash
    rm -f /home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/../output/lib/charset.tmp ; \
  fi ; \
fi
make[11]: Nothing to be done for 'install-data-am'.
make[11]: Leaving directory '/home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/gdb/build-gnulib/import'
make[10]: Leaving directory '/home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/gdb/build-gnulib/import'
make[9]: Leaving directory '/home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/gdb/build-gnulib/import'
make[8]: Leaving directory '/home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/gdb/build-gnulib/import'
make[7]: Leaving directory '/home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/gdb/build-gnulib'
make[6]: Leaving directory '/home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/gdb/build-gnulib'
make[5]: Leaving directory '/home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/gdb/build-gnulib'
make[4]: Leaving directory '/home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/gdb'
make[3]: Leaving directory '/home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/gdb'
make[2]: Leaving directory '/home/xjf1127/codespace/toolchain/gdb/cross_compile_gdb_7/build/gdb'
make[1]: Nothing to be done for 'install-target'.

```
成功


#### 交叉编译后可执行程序如何查看其符号属性
1. 是否为debug版本?
2. 源码路径是否包含进去?
3. qtcreator 编译出来的debug版本存在问题
4. 生成debug调试信息的种类和方式，并且如何查看调试信息，比如源码路径等


```bash
(gdb) info files
Symbols from "/home/xjf1127/codespace/git_down/build-yda_aplus-rk1126_arm_linux-Profile/ydn-rk.debug".
Local exec file:
        `/home/xjf1127/codespace/git_down/build-yda_aplus-rk1126_arm_linux-Debug1/ydn-rk', 
        file type elf32-littlearm.
        Entry point: 0x1f6fc
        0x00010154 - 0x0001016d is .interp
        0x00010170 - 0x00010190 is .note.ABI-tag
        0x00010190 - 0x000114c0 is .hash
        0x000114c0 - 0x00012988 is .gnu.hash
        0x00012988 - 0x00015598 is .dynsym
        0x00015598 - 0x0001abfe is .dynstr
        0x0001abfe - 0x0001b180 is .gnu.version
        0x0001b180 - 0x0001b290 is .gnu.version_r
        0x0001b290 - 0x0001b3e8 is .rel.dyn
        0x0001b3e8 - 0x0001c808 is .rel.plt
        0x0001c808 - 0x0001c814 is .init
        0x0001c814 - 0x0001e658 is .plt
        0x0001e658 - 0x000a08ec is .text
        0x000a08ec - 0x000a08f4 is .fini
        0x000a08f4 - 0x001a4c24 is .rodata
        0x001a4c24 - 0x001ac593 is .ARM.extab
        0x001ac594 - 0x001adea4 is .ARM.exidx
        0x001adea4 - 0x001adea8 is .eh_frame
        0x001be0f8 - 0x001be130 is .init_array
        0x001be130 - 0x001be134 is .fini_array
        0x001be134 - 0x001bee50 is .data.rel.ro
        0x001bee50 - 0x001bf000 is .dynamic
        0x001bf000 - 0x001c0058 is .got
        0x001c0058 - 0x001c0700 is .data
        0x001c0700 - 0x001ca584 is .bss
```

`info program` 可以查看程序是否有在运行


`gdb 删除断点后才能继续continue`
**由于使用release版本，会导致远程调试时出现各种异常**

gdbclient运行时报错: 主要集中在load lib失败，无法读取到符号
```bash
=thread-group-added,id="i1"
GNU gdb (GNU Toolchain for the A-profile Architecture 8.3-2019.03 (arm-rel-8.36)) 8.2.1.20190227-git
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "--host=x86_64-pc-linux-gnu --target=arm-linux-gnueabihf".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://bugs.linaro.org/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word".
=cmd-param-changed,param="pagination",value="off"
0xa6fcea00 in _start () from /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/libc/lib/ld-linux-armhf.so.3
Warning: Exceptions are not supported in this scenario.
Loaded '/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/libc/lib/ld-linux-armhf.so.3'. Symbols loaded.
[New Thread 431.3334]

Thread 1 "ydn-rk" hit Breakpoint 1, main (argc=1, argv=0xaefffd54) at ../new_aplus/main.cpp:217
217	    GetTimetoIC();
Loaded '/lib/libatomic.so.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/libcurl.so.4'. Cannot find or open the symbol file.
Loaded '/lib/libDeviceIo.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/librga.so.2'. Cannot find or open the symbol file.
Loaded '/usr/lib/libssl.so.1.0.0'. Cannot find or open the symbol file.
Loaded '/usr/lib/libcrypto.so.1.0.0'. Cannot find or open the symbol file.
Loaded '/lib/libpaho-mqtt3a.so.1'. Cannot find or open the symbol file.
Loaded '/lib/libpaho-mqtt3as.so.1'. Cannot find or open the symbol file.
Loaded '/lib/libpaho-mqtt3c.so.1'. Cannot find or open the symbol file.
Loaded '/lib/libpaho-mqtt3cs.so.1'. Cannot find or open the symbol file.
Loaded '/oem/usr/lib/libeasymedia.so.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/librkaiq.so'. Cannot find or open the symbol file.
Loaded '/oem/usr/lib/librockx.so'. Cannot find or open the symbol file.
Loaded '/oem/usr/lib/librknn_api.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libQt5Widgets.so.5'. Cannot find or open the symbol file.
Loaded '/usr/lib/libQt5Gui.so.5'. Cannot find or open the symbol file.
Loaded '/usr/lib/libQt5Network.so.5'. Cannot find or open the symbol file.
Loaded '/usr/lib/libQt5Core.so.5'. Cannot find or open the symbol file.
Loaded '/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/libc/lib/libpthread.so.0'. Symbols loaded.
Loaded '/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/libc/lib/librt.so.1'. Symbols loaded.
Loaded '/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/libc/lib/libdl.so.2'. Symbols loaded.
Loaded '/lib/libstdc++.so.6'. Cannot find or open the symbol file.
Loaded '/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/libc/lib/libm.so.6'. Symbols loaded.
Loaded '/lib/libgcc_s.so.1'. Cannot find or open the symbol file.
Loaded '/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/libc/lib/libc.so.6'. Symbols loaded.
Loaded '/usr/lib/libssl.so.1.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/libcrypto.so.1.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/libz.so.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/libglib-2.0.so.0'. Cannot find or open the symbol file.
Loaded '/usr/lib/libdbus-1.so.3'. Cannot find or open the symbol file.
Loaded '/usr/lib/libbluetooth.so.3'. Cannot find or open the symbol file.
Loaded '/usr/lib/libwpa_client.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libdrm.so.2'. Cannot find or open the symbol file.
Loaded '/oem/usr/lib/librockchip_mpp.so.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/libliveMedia.so.61'. Cannot find or open the symbol file.
Loaded '/usr/lib/libgroupsock.so.8'. Cannot find or open the symbol file.
Loaded '/usr/lib/libBasicUsageEnvironment.so.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/libUsageEnvironment.so.3'. Cannot find or open the symbol file.
Loaded '/usr/lib/libasound.so.2'. Cannot find or open the symbol file.
Loaded '/usr/lib/libRKAP_3A.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libRKAP_ANR.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libRKAP_Common.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libv4l2.so.0'. Cannot find or open the symbol file.
Loaded '/usr/lib/librknn_runtime.so'. Cannot find or open the symbol file.
Loaded '/oem/usr/lib/librockface.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libsqlite3.so.0'. Cannot find or open the symbol file.
Loaded '/usr/lib/libmd_share.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libod_share.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libpng16.so.16'. Cannot find or open the symbol file.
Loaded '/usr/lib/libpcre2-16.so.0'. Cannot find or open the symbol file.
Loaded '/usr/lib/libgthread-2.0.so.0'. Cannot find or open the symbol file.
Loaded '/usr/lib/libpcre.so.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/libv4lconvert.so.0'. Cannot find or open the symbol file.
Loaded '/usr/lib/libOpenVX.so.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/libVSC.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libGAL.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libArchModelSw.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libNNArchPerf.so'. Cannot find or open the symbol file.
Warning: Source file '/home/xjf1127/codespace/git_down/new_aplus/main.cpp' is newer than module file '/home/xjf1127/codespace/git_down/build-yda_aplus-rk1126_arm_linux-Debug/ydn-rk'.
Execute debugger commands using "-exec <command>", for example "-exec info registers" will list registers in use (when GDB is the debugger)

Thread 1 "ydn-rk" hit Breakpoint 5, gpio_init () at ../new_aplus/main.cpp:228
228	    gpio_init();

Thread 1 "ydn-rk" hit Breakpoint 7, main (argc=<optimized out>, argv=0xaefffd54) at ../new_aplus/main.cpp:230
230	    QApplication a(argc, argv);

Thread 1 "ydn-rk" hit Breakpoint 8, main (argc=<optimized out>, argv=0xaefffd54) at ../new_aplus/main.cpp:232
232	    QString dbgStr = "dbg=0";
Loaded '/usr/lib/qt/plugins/platforms/libqlinuxfb.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libfontconfig.so.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/libfreetype.so.6'. Cannot find or open the symbol file.
Loaded '/lib/libudev.so.1'. Cannot find or open the symbol file.
Loaded '/usr/lib/libexpat.so.1'. Cannot find or open the symbol file.
Loaded '/lib/libuuid.so.1'. Cannot find or open the symbol file.
ERROR: Unexpected GDB output from command "-exec-finish ". "finish" not meaningful in the outermost frame.

Thread 1 "ydn-rk" hit Breakpoint 2, create_log () at ../new_aplus/main.cpp:243
243	            create_log();

Thread 1 "ydn-rk" hit Breakpoint 4, setQuitSignals () at ../new_aplus/main.cpp:248
248	    setQuitSignals();

Thread 1 "ydn-rk" hit Breakpoint 9, main (argc=<optimized out>, argv=<optimized out>) at ../new_aplus/main.cpp:251
251	    if (QFile::exists("/lib/udev/rules.d/99-usb.rules")) {

Thread 1 "ydn-rk" hit Breakpoint 10, main (argc=<optimized out>, argv=<optimized out>) at ../new_aplus/main.cpp:259
259	    File_maker udev_maker;

Thread 1 "ydn-rk" hit Breakpoint 11, main (argc=<optimized out>, argv=<optimized out>) at ../new_aplus/main.cpp:260
260	    udev_maker.file_make_execcute();/*追加热插拔配置文件及变更ppp拨号节点映射*/

Thread 1 "ydn-rk" hit Breakpoint 12, main (argc=<optimized out>, argv=<optimized out>) at ../new_aplus/main.cpp:276
276	    MainWindow w_ui;
[New Thread 431.1704]
[New Thread 431.1746]
[New Thread 431.1726]
[New Thread 431.1749]
[New Thread 431.1795]
[New Thread 431.1689]
[New Thread 431.1690]
[New Thread 431.1705]
[New Thread 431.1706]
[New Thread 431.1747]
[New Thread 431.1748]
[New Thread 431.1757]

Thread 1 "ydn-rk" hit Breakpoint 13, main (argc=<optimized out>, argv=<optimized out>) at ../new_aplus/main.cpp:278
278	    w_ui.show();
Loaded '/usr/lib/qt/plugins/imageformats/libqgif.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/qt/plugins/imageformats/libqico.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/qt/plugins/imageformats/libqjpeg.so'. Cannot find or open the symbol file.
Loaded '/usr/lib/libjpeg.so.62'. Cannot find or open the symbol file.
Loaded '/usr/lib/qt/plugins/bearer/libqgenericbearer.so'. Cannot find or open the symbol file.
ERROR: Exception while processing MIEngine operation. An item with the same key has already been added. Key: i1. If the problem continues restart debugging.
[New Thread 1973.1973]
ERROR: Exception while processing MIEngine operation. An item with the same key has already been added. Key: i1. If the problem continues restart debugging.
[New Thread 1974.1974]
/tmp/dgboter/bbs/rhev-vm8--rhe6x86_64/buildbot/rhe6x86_64--arm-linux-gnueabihf/build/src/binutils-gdb--gdb/gdb/target.c:2599: internal-error: Can't determine the current address space of thread Thread 431.431

A problem internal to GDB has been detected,
further debugging may prove unreliable.
Quit this debugging session? (y or n) [answered Y; input not from terminal]
/tmp/dgboter/bbs/rhev-vm8--rhe6x86_64/buildbot/rhe6x86_64--arm-linux-gnueabihf/build/src/binutils-gdb--gdb/gdb/target.c:2599: internal-error: Can't determine the current address space of thread Thread 431.431

A problem internal to GDB has been detected,
further debugging may prove unreliable.
Create a core file of GDB? (y or n) [answered Y; input not from terminal]
ERROR: GDB exited unexpectedly with exit code 134 (0x86). Debugging will now abort.
The program '/home/xjf1127/codespace/git_down/build-yda_aplus-rk1126_arm_linux-Debug/ydn-rk' has exited with code -1 (0xffffffff).

```
**可以发现很多符号是需要从lib中加载的**
refe: https://visualgdb.com/gdbreference/commands/set_sysroot
`show sysroot` 查看当前sysroot环境
```bash
(gdb) show sysroot
The current system root is "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf/libc".
### 可以发现目前只有一个交叉编译工具链的libc

```
补充
```bash
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot

/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/target

/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf

/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2

set sysroot remote:/
set solib-search-path /path/to/your/libs
set debug-file-directory /path/to/debug/files
set native-async on

## 源文件路径
directory /path/to/your/source
set sysroot：用于指定目标文件系统的根目录，帮助 GDB 查找库。
set solib-search-path：可以用于指定动态库的搜索路径。

# 添加多个搜索路径（本机存放源码的路径）
dir /path/to/source1 /path/to/source2

# 如果源码在远程和本地位置不同，可进行路径替换
set substitute-path /remote/source/path /local/source/path

# 手动加载共享库的符号
sharedlibrary /path/to/libmylib.so

# 指定搜索共享库的路径（如果库在不同目录）
set solib-search-path /path/to/libs

# 允许GDB自动加载调试符号（如果目标支持）
set auto-load safe-path /path/to/libs


 如果远程设备上的库位于 /lib/ 和 /usr/lib/，但本机存放在 /mnt/target/lib/ 和 /mnt/target/usr/lib/，可使用：


如果远程设备上的库位于 /lib/ 和 /usr/lib/，但本机存放在 /mnt/target/lib/ 和 /mnt/target/usr/lib/，可使用：
set solib-search-path /mnt/target/lib:/mnt/target/usr/lib


在 GDB 中，您可以使用 directory 命令来指定源代码的搜索路径。这可以让 GDB 知道在何处查找源文件，即使这些文件的路径在远程设备和本地机器上不同。
directory /path/to/local/source

如果您希望在 GDB 中更灵活地处理源文件路径，可以使用 set substitute-path 命令。此命令允许您将一个路径替换为另一个路径，以便 GDB 可以找到源文件。
set substitute-path /original/source/path /new/source/path

例如，如果您的源文件在电脑1上位于 /home/user/project/src，而在电脑2上位于 /home/user/src，可以运行以下命令：
set substitute-path /home/user/project/src /home/user/src

 示例
假设您在电脑1上编译的源文件路径是 /home/user/project/src，而在电脑2上源文件路径是 /home/user/src。您可以在 GDB 中执行以下命令：
(gdb) directory /home/user/src
(gdb) set substitute-path /home/user/project/src /home/user/src
(gdb) target remote <remote_ip>:<port>

检查已加载的库：
info sharedlibrary
```
actual
```bash
set sysroot /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot

set sysroot /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/target

set sysroot /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/arm-linux-gnueabihf


set sysroot remote: /

set solib-search-path /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib

info sharedlibrary
info files
show sysroot

# show directory

# show substitue-path

```

在pc端gdb调试，ctrlz 退到后台后
```bash

## 使用jobs命令，查看后台作业

## fg %<number> 编号切换到前台 (foreground)

## bg %<number> 编号切换到后台 (background)

shell 会话的作业（jobs）是指在一个 shell 实例中启动的进程或任务。作业可以在前台或后台运行，具体解释如下：

前台作业
定义：前台作业是指在当前 shell 会话中直接运行的进程，用户可以与之交互。例如，您在终端中直接运行的命令。
特征：
可以接收输入和输出。
用户可以直接看到其运行状态。

后台作业
定义：后台作业是在 shell 中启动，但不与当前终端直接交互的进程。通常在命令后加上 & 符号启动。
特征：
进程在后台运行，用户不能直接输入。
用户可以使用 jobs 命令查看后台作业的状态。

# 运行一个前台作业
sleep 10

# 运行一个后台作业
sleep 10 &

obs：列出当前 shell 会话中的所有作业及其状态。
fg：将后台作业恢复到前台。
bg：将挂起的作业移到后台继续运行。
kill：终止作业。

SIGTSTP：挂起作业（通常由 Ctrl + Z 发送）。
SIGCONT：恢复作业（通常由 fg 或 bg 发送）。


```


vscode launch.json配置
```bash

```