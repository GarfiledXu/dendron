---
id: nzqjkoz46d8ookq3cjwi0lh
title: Soname
desc: ''
updated: 1727167080865
created: 1727145533635
---

#### ref
[bilibili: soname](https://www.bilibili.com/read/cv27486835/)
[linux 共享库的版本控制和使用](https://lovewubo.github.io/shared_library)
[一文读懂Linux下动态链接库版本管理及查找加载方式](https://blog.ideawand.com/2020/02/15/how-does-linux-shared-library-versioning-works/)
[LINUX下动态库及版本号控制](https://www.cnblogs.com/siqi/p/4798284.html)


#### cmake中如何设置程序查找so的规则
[cmake-编译带有版本号动态库以及链接的问题](https://juejin.cn/post/6956021715671613477)


```cmake
set_target_properties(${cur_exe_name1} PROPERTIES
    # VERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}"
    VERSION "${testbed_conn_lib_version}" # 2.0.2
    SOVERSION ${PROJECT_VERSION_MAJOR} # 2
)
## 通过该设置，最终会生成 so so.2 so.2.0.2, 但是so是指向so.2,so.2指向so.2.0.2的，也就是使用so必须so.2要存在
#最后现象: 程序必须链接so.2 而不能识别so.2.0.2
# 程序能否兼容识别so.2.0.2以及so.2的格式，而不需要手动创建不带版本号的软连接指向带版本号的程序
# 如何控制cmake行为，1.让程序集成so时找so.1 major版本 2.让程序找so.2.0.2 全版本 3.让程序查找so 无版本号
# 去掉SOVERSION的设置，只流version以后，程序将默认查找全版本号名称
# 去掉version，只留soversion以后，程序将默认查找so.2版本,并且生成三个名称版本
```
只设置VERSION "${testbed_conn_lib_version}"，生成 so.2(链接) + so.2.0.2 ,程序查找全版本号
只设置soversion libtestbed_conn.so.2+libtestbed_conn.so(链接), 程序查找so.2

**根据现象可知cmake中设置了SOVERSION，表明其它程序链接该库时，将使用指定的soname**
**根据现象可知cmake中设置了VERSION，来指示生成的so名称，以及软链接**
**问题来了: 程序在运行时linker so时，查找的soname 是由程序链接时so内的信息决定的，还是在程序在编译时指定的so文件名指定的**


**可知unix上所谓的so加载，主版本mjor兼容的功能是由创建so软链接实现的，并不是由程序启动时linker实现的，即所谓的soname，只是一个命名规范而已，linker并不解析该规则，通常的做法是: 让程序在编译时写入的soname只带major号，如so.1, linker默认只寻找和加载该唯一名称的so，每一次更换动态库，将全版本so放入，并且修改软链接so.1 指向全版本so**

**对于testbed_conn模块而言，行为范式: 编译so时，只设置soversion, 指定soname: so.*, testbed 编译时设置无版本so为链接库，在运行testbed client时，server端替换 testbed自带的client，只要ABI版本匹配即可**

#### 查看so内容
```bash
oot@xjf2613-2366:/home/xjf2613/code-space/qnx/pe_new_board_service/out/out_linux_arm_tda4/testbed_conn # readelf -d libtestbed_conn.so.2.0.2 

Dynamic section at offset 0x7caaf0 contains 33 entries:
  Tag        Type                         Name/Value
 0x0000000000000001 (NEEDED)             Shared library: [libpthread.so.0]
 0x0000000000000001 (NEEDED)             Shared library: [libresolv.so.2]
 0x0000000000000001 (NEEDED)             Shared library: [libstdc++.so.6]
 0x0000000000000001 (NEEDED)             Shared library: [libm.so.6]
 0x0000000000000001 (NEEDED)             Shared library: [libgcc_s.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.6]
 0x000000000000000e (SONAME)             Library soname: [libtestbed_conn.so.2]
 0x000000000000000c (INIT)               0x43ef78
 0x000000000000000d (FINI)               0x6aff20
 0x0000000000000019 (INIT_ARRAY)         0x7d66f8
 0x000000000000001b (INIT_ARRAYSZ)       72 (bytes)
 0x000000000000001a (FINI_ARRAY)         0x7d6740
 0x000000000000001c (FINI_ARRAYSZ)       8 (bytes)
 0x0000000000000004 (HASH)               0x1c8
 0x000000006ffffef5 (GNU_HASH)           0x28fc0
 0x0000000000000005 (STRTAB)             0xee880
 0x0000000000000006 (SYMTAB)             0x59768
 0x000000000000000a (STRSZ)              2898352 (bytes)
 0x000000000000000b (SYMENT)             24 (bytes)
 0x0000000000000003 (PLTGOT)             0x7dbfe8
 0x0000000000000002 (PLTRELSZ)           459768 (bytes)
 0x0000000000000014 (PLTREL)             RELA
 0x0000000000000017 (JMPREL)             0x3ceb80
 0x000000006ffffef6 (TLSDESC_PLT)        0x489ce0
 0x000000006ffffef7 (TLSDESC_GOT)        0x7dbfe0
 0x0000000000000007 (RELA)               0x3be998
 0x0000000000000008 (RELASZ)             66024 (bytes)
 0x0000000000000009 (RELAENT)            24 (bytes)
 0x000000006ffffffe (VERNEED)            0x3be8f8
 0x000000006fffffff (VERNEEDNUM)         4
 0x000000006ffffff0 (VERSYM)             0x3b2230
 0x000000006ffffff9 (RELACOUNT)          357
 0x0000000000000000 (NULL)               0x0
```