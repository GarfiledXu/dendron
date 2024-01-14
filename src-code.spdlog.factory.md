---
id: lpl4v3adzi3wmgs5eqx3m4b
title: Factory
desc: ''
updated: 1703495113603
created: 1703487391881
---

#### sample
```c++
//common define
//define static template member method
//why is need to be defined as member method instead of global method?
struct synchronous_factory
{
    template<typename Sink, typename... SinkArgs>
    static std::shared_ptr<spdlog::logger> create(std::string logger_name, SinkArgs &&...args)
    {
        auto sink = std::make_shared<Sink>(std::forward<SinkArgs>(args)...);
        auto new_logger = std::make_shared<spdlog::logger>(std::move(logger_name), std::move(sink));
        details::registry::instance().initialize_logger(new_logger);
        return new_logger;
    }
};

//-inl.h define
//merge special Factory class and special Sink to a global static method
//whole flow path, could strip current step?
template<typename Factory>
SPDLOG_INLINE std::shared_ptr<logger> stdout_color_mt(const std::string &logger_name, color_mode mode)
{
    return Factory::template create<sinks::stdout_color_sink_mt>(logger_name, mode);
}

template<typename Factory>
SPDLOG_INLINE std::shared_ptr<logger> stdout_color_st(const std::string &logger_name, color_mode mode)
{
    return Factory::template create<sinks::stdout_color_sink_st>(logger_name, mode);
}

//.h declare
template<typename Factory = spdlog::synchronous_factory>
std::shared_ptr<logger> stdout_color_st(const std::string &logger_name, color_mode mode = color_mode::automatic);

template<typename Factory = spdlog::synchronous_factory>
std::shared_ptr<logger> stdout_color_mt(const std::string &logger_name, color_mode mode = color_mode::automatic);

//using 

```

#### key grammar
question1: 如何让一个函数调用一个模板对象的某个方法
>  1. 函数改为模板函数, 内部直接强制默认某个对象为目标模板类型，并且默认该类型拥有该方法，直接强制调用
>  2. 设置一个模板类型参数用于传入目标模板对象的类型, 设置一个函数形参
>  3. 


#### other alternative way

    
#### design core point

    

- type:<Factory> + param: (logger_name, color_mode) => global template method: call Factory create method
   - Factory:  struct/class: define static member template method create 
     - create method: instance sink and logger, return logger

    