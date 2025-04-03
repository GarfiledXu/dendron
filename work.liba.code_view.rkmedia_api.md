---
id: zsks93ftoepoebvkr66fb93
title: Rkmedia_api
desc: ''
updated: 1743047026005
created: 1743039932955
---

### RK_MPI_SYS_Init

1. 维护一个全局变量`g_sys_init`，初始化成功后，变量置于true，用于检验反复init判断
2. 重置数据通道相关结构
3. 假设编译选项`RKMEDIA_SOCKET`使能，则创建server线程：rkmedia_server_thread 每accept一个请求都会创建线程:rec_thread，进行命令解析，从全局FunMap中匹配出对应函数指针进行调用，并将最后的ret进行返回，从客户端接收所有的远程调用

### 