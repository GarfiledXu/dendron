---
id: z1by6f3dsgn5r4zx2oynyk3
title: Demo
desc: ''
updated: 1736909785359
created: 1736828431918
---

### 线程间同步
1. 互斥锁
2. 递归锁
3. 读写锁
4. 自旋锁
5. 条件变量
6. 信号量

### 进程间通信 + 同步
1. pipe
2. fifo
3. 消息队列
4. 共享内存
5. 套接字

6. 文件锁
7. 信号
8. 信号量

### 智能指针的使用以及案例
1. 智能指针的创建
2. 智能指针的获取
3. 循环引用 + 解决循环引用

### 网络编程 进行数据流编程和数据报编程的区分
0. 与互联网的基础服务进行通信
1. socket 消息传输
   1. 基于tcp实现的time server(参考 understanding unix/linux programmming)
2. socket 终端交互
3. socket 传二进制文件
4. socket 进行序列化参数传递
5. socket 实现http协议
6. socket 实现websocket协议
7. socket 实现mqtt协议
8. 通过io复用优化上述功能
9.  通过文件内存映射，来优化大文件传输
10. 使用开源的c++网络库