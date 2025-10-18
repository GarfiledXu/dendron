---
id: l2v46p0j6egy5nfriyyj37d
title: Release_exe
desc: ''
updated: 1757664184327
created: 1757663764619
---

## 问题背景

调查rpc_core测试程序，运行时崩溃问题，发现只有release版本运行效率最高时才能复现，debug版本无法复现


```bash
cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo ..
效果：

和 Release 一样用 -O2 优化。

同时加上 -g 保留调试符号。

程序运行时就是 release 性能，但 gdb 可以调试。
```


```bash
你可以在 CMakeLists.txt 里加一段规则，让 CMake 自动生成 .debug 文件：

# 假设你的可执行程序名是 my_app
add_executable(my_app main.cpp)

# 仅在 Release 或 RelWithDebInfo 模式下做符号分离
if (CMAKE_BUILD_TYPE MATCHES "Release|RelWithDebInfo")
    add_custom_command(TARGET my_app POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} --only-keep-debug $<TARGET_FILE:my_app> $<TARGET_FILE:my_app>.debug
        COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded $<TARGET_FILE:my_app>
        COMMAND ${CMAKE_OBJCOPY} --add-gnu-debuglink=$<TARGET_FILE:my_app>.debug $<TARGET_FILE:my_app>
        COMMENT "Stripping symbols and saving debug info to $<TARGET_FILE:my_app>.debug"
    )
endif()


这样构建流程就是：

先编译 → 生成带符号的 release 二进制

提取符号表 → 保存到 my_app.debug

strip 程序 → 生成精简版 release 可执行文件

打上 debug link → gdb 可以自动加载 .debug 符号

3. 使用方式

部署到设备：只带 my_app（小，strip 过）

调试问题：在本地把 my_app.debug 放到同目录，gdb ./my_app 就能看到函数和源码行号。
```
