---
id: ngtis90kfnpu44vh8mjr5s6
title: Initializer_list_and_template
desc: ''
updated: 1696930782838
created: 1696930293470
---

#### refe
- [C++ 可变参数函数、initializer_list、省略号形参](https://blog.csdn.net/weixin_38739598/article/details/112596360)

```c++
//why?
using sinks_init_list = std::initializer_list<sink_ptr>;
logger(std::string name, sinks_init_list sinks)
    : logger(std::move(name), sinks.begin(), sinks.end())
{}

auto ss = std::shared_ptr<spdlog::logger>(new spdlog::logger("logger0", {console_sink, file_rotate_sink}));//compile pass
auto new_logger = std::make_shared<spdlog::logger>("logger0", console_sink, file_rotate_sink);//compile fail

auto new_logger1 = std::make_shared<spdlog::logger>("logger1", { file_rotate_sink1 }); //compile fail, 

auto new_logger = std::make_shared<spdlog::logger>("logger0", spdlog::sinks_init_list({ console_sink, file_rotate_sink }));//compile pass
```