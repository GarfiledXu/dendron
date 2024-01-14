---
id: q4llrxq3f9wykewjn7rnqi9
title: '1'
desc: ''
updated: 1702633124580
created: 1702608561469
---

####  Data Structure
**what is context structure?**
> it's a general naming convetion that a data structure includes the "context" field in ffmpeg.

**question extend**
1. this type of data structure when will be instantiated and in which work flow?
2. and what function will be called?
3. the entity of context type structure as input data or output data for function call?
4. the relationship between file structure, file naming, and module structure and context data structure?

**discover**


**context structure list**
AVFormatContext
AVCodecContext
AVIOContext



#### 设计模式
**void* opaque**
```c++
struct AVIOContext{
    unsigned char *buffer：缓存开始位置
    int buffer_size：缓存大小（默认32768）
    unsigned char *buf_ptr：当前指针读取到的位置
    unsigned char *buf_end：缓存结束的位置
    void *opaque：URLContext结构体
};
```
> ffmpeg 中 URLContext以及URLProtocol结构体只声明于源码文件中而不存在于对外暴露的头文件，
通过void* 隐藏实现细节，定义以及转换和操作都由库配套实现
1. 实现兼容性
2. 减少外在风险

**multi url support**
```c++

```
> ffmpeg中c语言的类多态实现，通过定义URLProtocol结构体, 成员主要为url操作的函数指针，通过预定义并实例化不同的对象，来生成支持不同协议的URLprotcol进行协议切换