---
id: l59a62v9u29hcyrrkhb5peq
title: Spdlog
desc: ''
updated: 1695566089026
created: 1692712451015
---


### spdlog
-------------------------
#### reference
****
- [blog-spdlog struct](https://www.cnblogs.com/shuqin/p/12214439.html)
- [github](https://github.com/gabime/spdlog)
- [spdlog 学习笔记](https://blog.csdn.net/haojie_superstar/article/details/89383433)
- [beautful format](https://dev.to/ptkdev/the-best-alternative-to-the-console-log-statement-470k)

#### work flow
****
-  

#### call entry
- modify source code to specify default pattern and settings
- using singleton for global call?
- using global object?
#### inherit of spdlog
##### sink
- sink(log)->>base_sink(ov log called sink_it_)->>rotating_sink(ov sink_it)
- sink->>color_sink(ov log)

#### concept and component and call order
> refer note of source code: // thread safe logger (except for set_error_handler())
// has name, log level, vector of std::shared sink pointers and formatter
// upon each log write the logger:
// 1. checks if its log level is enough to log the message and if yes:
// 2. call the underlying sinks to do the job.
// 3. each sink use its own private copy of a formatter to format the message
// and send to its destination.
//
// the use of private formatter per sink provides the opportunity to cache some
// formatted data, and support for different format per sink.

**register**
a top manager all logger obj

**logger**
could understand as front end
provide outside method as user input entry and priority setting
each logger will bind/register a global uniform tag

**sinks**
could understand as back end
msg pipeline, respect transfer the msg data to the target such as console, file, or remote host

**formatter**
an important component of back end, to convert msg from log() of sinks to a buff data which be proccessed on end point such as fwrite or console stdout
provide the format function **called by each sink** to format msg
adapt user input argument of msg convert to an format string
- custom flag
> exactly, user select the flag which predefine on the framework of spdlog 
> default behavior of unknow flag ?

#### custom level name
- level name and level short name
1. reference notes of **tweakme.h**
2. outside provide user  **tweakme.h file** to add customize macro to control log feature
3. **tweakme.h as bottom header include by other header**
4. other header provide customize switch macro
5. finally, add or reduce switch macro and modify correspond macro
> if tweakme.h just provideo some customize notes instead of for user modify
> or it's right way to add customize macro before inclucde header of spdlog
> macro just text replace, so if using header only way to reference spdlog, upon way is useful
> now need to confirm if it's useful upon way when using spdlog lib which is predeine as a lib?[to resolve]
1. modify array directly, it's a run time way, upon way can't use when user spdlog lib
```c++
//common.h 
#include "tweakme.h"
#define SPDLOG_LEVEL_NAME_TRACE spdlog::string_view_t("trace", 5)
#define SPDLOG_LEVEL_NAME_DEBUG spdlog::string_view_t("debug", 5)
#define SPDLOG_LEVEL_NAME_INFO spdlog::string_view_t("info", 4)
#define SPDLOG_LEVEL_NAME_WARNING spdlog::string_view_t("warning", 7)
#define SPDLOG_LEVEL_NAME_ERROR spdlog::string_view_t("error", 5)
#define SPDLOG_LEVEL_NAME_CRITICAL spdlog::string_view_t("critical", 8)
#define SPDLOG_LEVEL_NAME_OFF spdlog::string_view_t("off", 3)

#if !defined(SPDLOG_LEVEL_NAMES)
#    define SPDLOG_LEVEL_NAMES                                                                                                             \
        {                                                                                                                                  \
            SPDLOG_LEVEL_NAME_TRACE, SPDLOG_LEVEL_NAME_DEBUG, SPDLOG_LEVEL_NAME_INFO, SPDLOG_LEVEL_NAME_WARNING, SPDLOG_LEVEL_NAME_ERROR,  \
                SPDLOG_LEVEL_NAME_CRITICAL, SPDLOG_LEVEL_NAME_OFF                                                                          \
        }
#endif

#if !defined(SPDLOG_SHORT_LEVEL_NAMES)

#    define SPDLOG_SHORT_LEVEL_NAMES                                                                                                       \
        {                                                                                                                                  \
            "T", "D", "I", "W", "E", "C", "O"                                                                                              \
        }
#endif
//common-inl.h
static string_view_t level_string_views[] SPDLOG_LEVEL_NAMES;
static const char *short_level_names[] SPDLOG_SHORT_LEVEL_NAMES;
```
```c++
//tweakme.h
//user custom
#define SPDLOG_LEVEL_NAMES { "MY TRACE", "MY DEBUG", "MY INFO", "MY WARNING", "MY ERROR", "MY CRITICAL", "OFF" }
#define SPDLOG_SHORT_LEVEL_NAMES { "T", "D", "I", "W", "E", "C", "O" }
```
#### customize message content format
```c++
//align
%<width><flag> right
%-<width><flag> left
%=<width><flag> center
//default
[日期][等级][threadid][processid][logger][file and line][function]
//how to setting dynamic
```
#### customize msg color
#### customize msg colored location
#### setting level
#### API range
**static method**
**basic class**
**derive class**
**struct**
**stand-alone module**
**global variable**

#### usage example

#### usage target
- custom log tag and log name str and color for each log level
- manager all stdout save to file whilch can be roll and override
- transform log msg to remote server by tcp and udp
- log file dump

#### test code

#### the register flow of logger on spdlog
- register operation provide uniform entry to refer logger by name to uniform manager logger

#### std::function && nullptr && default_handler_ && struct_handler_set
****
- what is NULL? the different between C and C++?
  - which is a macro correspond to a explict type value
  - NULL in C, is a (void*) point type value
    > #define NULL ((void*)0)
  - NULL in C++, is a const value
    > #define 0
- type conversion rule of C and C++, contain display and implicit(auto type conversion/implicit type conversion)?
    - [zhihu-c++终极避坑指南](https://zhuanlan.zhihu.com/p/561716560)
    - **the different interpret for c/c++ standard between diff compilers**
    - typically, the difference in interpretation between compilers will be what
      - different levels of realization
      - some feature not implementation
      - default type size

- what is nullptr? why need nullptr? how to indicate the type of nullptr?
    - which is a keyword, be derived by compiler to type specific value is done during compilation.
- why std::function can accept nullptr to initialize and assigned
- how to define an custom type to like std::function has the attribute with nullptr
- the stand specification of callback define
  ?


#### abstract class derived how to keep pure virtual function?
*****
- why need to pass same virtual function by declare middle abstract class?
1. typically, just like java inherit, there some reason tha we need add some uniform operation to the virtual function, sprite the uniform operation and implementaion of virtual function
2. in spdlog, there is uniform inherit behavior to add mutex behavior for each virtual function,
3. sink is responsible for polymorphic behavior, base_sink is responsible for as an abstract class keep pass polymorphic behavior and provide mutex control!
  
- rule: if want an abstract class to derived other abstract, there is must to do is that iplementation the pure virtual function on derived class
- way: using protected modifier to declare the pure virtual correspond to the pure virtual function of base class, then new virtual function called by the implementaion of old virtual function, **all in all, using new pure virtual function as the implementation of old pure virutal funciton provide the same interface to next derived class.**
``` c++
//base_sink.h
namespace spdlog {
namespace sinks {
template<typename Mutex>
class SPDLOG_API base_sink : public sink
{
public:
    base_sink();
    explicit base_sink(std::unique_ptr<spdlog::formatter> formatter);
    ~base_sink() override = default;

    base_sink(const base_sink &) = delete;
    base_sink(base_sink &&) = delete;

    base_sink &operator=(const base_sink &) = delete;
    base_sink &operator=(base_sink &&) = delete;

    void log(const details::log_msg &msg) final;
    void flush() final;
    void set_pattern(const std::string &pattern) final;
    void set_formatter(std::unique_ptr<spdlog::formatter> sink_formatter) final;

protected:
    // sink formatter
    std::unique_ptr<spdlog::formatter> formatter_;
    Mutex mutex_;

    virtual void sink_it_(const details::log_msg &msg) = 0;
    virtual void flush_() = 0;
    virtual void set_pattern_(const std::string &pattern);
    virtual void set_formatter_(std::unique_ptr<spdlog::formatter> sink_formatter);
};
} // namespace sinks
} // namespace spdlog
//base_sink-inl.h
template<typename Mutex>
void SPDLOG_INLINE spdlog::sinks::base_sink<Mutex>::log(const details::log_msg &msg)
{
    std::lock_guard<Mutex> lock(mutex_);
    sink_it_(msg);
}

template<typename Mutex>
void SPDLOG_INLINE spdlog::sinks::base_sink<Mutex>::flush()
{
    std::lock_guard<Mutex> lock(mutex_);
    flush_();
}

template<typename Mutex>
void SPDLOG_INLINE spdlog::sinks::base_sink<Mutex>::set_pattern(const std::string &pattern)
{
    std::lock_guard<Mutex> lock(mutex_);
    set_pattern_(pattern);
}
```
- file struct of .h and -inl.h
essentially, it't using inline key word, based on modification of conditinal macros
```c++
//macro
#ifdef SPDLOG_COMPILED_LIB
#    undef SPDLOG_HEADER_ONLY
#    if defined(SPDLOG_SHARED_LIB)
#        if defined(_WIN32)
#            ifdef spdlog_EXPORTS
#                define SPDLOG_API __declspec(dllexport)
#            else // !spdlog_EXPORTS
#                define SPDLOG_API __declspec(dllimport)
#            endif
#        else // !defined(_WIN32)
#            define SPDLOG_API __attribute__((visibility("default")))
#        endif
#    else // !defined(SPDLOG_SHARED_LIB)
#        define SPDLOG_API
#    endif
#    define SPDLOG_INLINE
#else // !defined(SPDLOG_COMPILED_LIB)
#    define SPDLOG_API
#    define SPDLOG_HEADER_ONLY
#    define SPDLOG_INLINE inline
#endif // #ifdef SPDLOG_COMPILED_LIB
//inl.h
SPDLOG_INLINE void spdlog::sinks::sink::set_level(level::level_enum log_level)
{
    level_.store(log_level, std::memory_order_relaxed);
}
```
[to resolve]
- the underlying principles of inline behavior ?
  - c++ layer ?
  - ASM layer ?
- inline function and inline member function
  - the purpose of inline keyword ?
  - what will compiler to when using inline keyword ?
  - why can include repeat inline function and there is no compile conflict ?
  - is there repeat define when declare inline function but compiler not choice to inline ?
  - how to solve problem of repeated definition, is the reason to copy same implementation of function for each compiltion unit?
  - macro and inline function
  > actual effect similar
  > macro processed by preprocesser and just replace code
  > inline processed by compiler and will be modified code
- refe
  - [blog-zhihu-符号定义](https://zhuanlan.zhihu.com/p/619916346)
  - [blog-头文件中定义函数引发的multiple definition](https://jiadebin.github.io/2017/04/03/%E5%A4%B4%E6%96%87%E4%BB%B6%E4%B8%AD%E5%AE%9A%E4%B9%89%E5%87%BD%E6%95%B0%E5%BC%95%E5%8F%91%E7%9A%84multiple-definition/)
  - [blog-jianhsu-C++ multiple definition 总结](https://www.jianshu.com/p/c028fee0f202)
  - [microsoft-inline function](https://learn.microsoft.com/zh-cn/cpp/cpp/inline-functions-cpp?view=msvc-170)
  
  ##### recursive mutex using scene
  - mutex refe by multi inner function, this method will be called parallel or nested
