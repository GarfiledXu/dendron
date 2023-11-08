---
id: 7va3muon7q66smpplisot4t
title: Compile_analysis
desc: ''
updated: 1699414070987
created: 1695610893045
---
#### refe
- [analysis cost time of compile](https://tech.meituan.com/2020/12/10/apache-kylin-practice-in-meituan.html)
- [c++ 符号表](https://zhuanlan.zhihu.com/p/600009670)
- [cppreference.initialization](https://en.cppreference.com/w/cpp/language/initialization)
- [csdn-查看内存布局的方法](https://blog.csdn.net/weixin_43919932/article/details/120268494)
- [blog-程序员自我修养_编译链接](https://taifua.com/psst-compile-link.html)
- [blog-从四个问题透析Linux下C++编译&链接](https://bbs.huaweicloud.com/blogs/197791)
- [blog-c++ 内存模型](https://paul.pub/cpp-memory-model/)
#### open middle file in cmake
- add compile option: `-save-temps`, to generate *.i *.ii and *.s file on build dir
```c++
add_definitions(-save-temps)
```


#### sence 2
compile can't pass that undefined refer on spdlog when link stage, but same lib module and impl code, just replace the project to reference
```bash
/home/xjf2613/code-space/qnx/pe-board_sdk_service/src/module/m_samba/lib/libsmbclient.so: warning: The 'mktemp' function is dangerous. Use 'mkstemp' instead.
CMakeFiles/main_service_qnx.dir/main.cpp.o: In function `spdlog::sinks::gf_rotating_file_sink<std::__1::mutex>::sink_it_(spdlog::details::log_msg const&)':
/home/xjf2613/code-space/qnx/pe-board_sdk_service/src/module/m_spdlog/impl/gf_rotating_file_sink-inl.h:166: undefined reference to `spdlog::details::file_helper::write(fmt::v10::basic_memory_buffer<char, 250ul, std::__1::allocator<char> > const&)'
collect2: error: ld returned 1 exit status
o_main_service/CMakeFiles/main_service_qnx.dir/build.make:369: recipe for target '/home/xjf2613/code-space/qnx/pe-board_sdk_service/out/main_service_qnx/main_service_qnx' failed
make[2]: *** [/home/xjf2613/code-space/qnx/pe-board_sdk_service/out/main_service_qnx/main_service_qnx] Error 1
CMakeFiles/Makefile2:367: recipe for target 'o_main_service/CMakeFiles/main_service_qnx.dir/all' failed
make[1]: *** [o_main_service/CMakeFiles/main_service_qnx.dir/all] Error 2
Makefile:90: recipe for target 'all' failed
```
##### find resean
on cmakelist using the diff compile macro which used inside the spdlog
```cmake
#right modify
set(CMAKE_CXX_STANDARD 11)
ADD_DEFINITIONS(-std=gnu++11)
add_definitions(-w)
# add_definitions(-DSPDLOG_COMPILED_LIB)
```