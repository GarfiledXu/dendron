---
id: s7tdxpkvbwflsd8ax0a4u18
title: Io
desc: ''
updated: 1737174639718
created: 1737167282773
---

### 常用clib库接口 缓冲刷新的默认行为
- `fprintf` 首先可以确定的是，如果是普通的FILE流，fprintf执行的是全缓冲策略，遇到`\n`不会刷新，这与printf不同，printf是行缓冲
  但如果fprintf使用的是关联终端的标准流呢，是否是行缓冲，就类似printf一样,如果FILE参数是`stdout`,那么就等同于printf
  **so, printf修饰的含义并不是行缓冲，而是格式化输出，这与缓冲模式没有然后关系，仅仅是printf关联标准输出，所以与终端交互默认行缓冲**

- `getc` 和 `putc` 的缓冲模式呢？
  - 疑问: 即等待缓冲区就绪时， 若缓冲区为空，getc如何处理? 同理为满，putc如何处理; 什么情况会有EOF返回;
  - 试验后确认: putc 会存在缓冲，在fflush之后，或者pclose之后，才会触发write

- 如何确保流交互时的数据完整性呢?
  - 首先 read 和 write 都是由 fwrite 和 fread 判断buff后的行为触发
  - fwrite: 当buff 满时，触发write
    fread: 当buff 空时， 触发一次read
  - 所以需要考虑，最后一次的fwrite数据 没有被发送，而fread是调用后，buff非空读取buff，buff为空，进行read,不用担心读取的问题
  - fclose 或者 fflush时可以保证触发write


读写与缓冲区的交互过程，读: 缓冲区为空，触发read进货到缓冲区; 写: 缓冲区满，触发write发货；
分析策略, 使用什么缓冲模式，取决于对应的文件流的默认属性以及设置属性
首先与终端交互的一般默认行缓冲，比如printf，以及使用stdout的fprintf
sock 经过fdopen以后，以及popen获得的文件流，又是什么缓冲模式的呢
**通过sock打开的文件流，实验用fprintf进行写入，是全缓冲**
**通过popen打开的文件流**

### 流的种类
文件流
管道流
网络流
### 实验设计
1. 验证 putc(c, sock_fpo) 是否在fclose前将数据写入缓冲区? 
   1. 调用putc n次后
   2. 在fclose之前
   3. 通过gdb命令，查看指针缓冲区成员内存情况
   ```bash
   检查缓冲区内存
    (gdb) p sock_fpo->_IO_buf_base
    $1 = 0x55555556f810 "compile_info.txt\nrls_client\nrls_server\n"
    (gdb) x/s sock_fpo->_IO_buf_base
    0x55555556f810: "compile_info.txt\nrls_client\nrls_server\n"
   ```

### server端的，socket接口返回的fd，以及accept返回的fd，是否要进行关闭? socket描述符的处理?
### 如何确认 描述符资源是否泄漏? 能否用gdb进行调试确认?