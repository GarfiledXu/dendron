---
id: jw7o7tx92o6sqh19mjjhknp
title: Template_member_function
desc: ''
updated: 1699347034314
created: 1699279184196
---

#### sence
```c++
/******************************************************************************

Welcome to GDB Online.
GDB online is an online compiler and debugger tool for C, C++, Python, Java, PHP, Ruby, Perl,
C#, OCaml, VB, Swift, Pascal, Fortran, Haskell, Objective-C, Assembly, HTML, CSS, JS, SQLite, Prolog.
Code, Compile, Run and Debug online from anywhere in world.

*******************************************************************************/
#include <iostream>
#include <mutex>
using namespace std;
struct cfg_init_to_server
{
    struct i30_network_
    {
        std::string listen_ip;
        int listen_port;
        std::string upload_ip;
        int upload_port;
        int upload_connect_timeout_ms;
        int upload_connect_retry_max;
    };

    struct objective_network_
    {
        std::string listen_ip;
        int listen_port;
    };

    struct log_
    {
        /* data */
        //log output mode controled by commond line argument input
        int level;
        std::string root_path;
        std::string testbed_stdout_name;
        //not required
        std::string server_stdout_name;
    };

    struct objective_runtime_
    {
        /* data */
        int objective_run_testbed_process_max;
        int objective_download_thread_max;
        int objective_upload_thread_max;
        int objective_cache_video_max;
    };

    std::string modify_date;
    std::string default_parse_version;

    log_ log;
    objective_network_ objective_network;
    i30_network_ i30_network;
    objective_runtime_ objective_runtime;
};
#define DECLARE_GET_SET(TYPE) \
private: \
    TYPE TYPE##_ = TYPE(); \
    mutable std::mutex mutex_##TYPE##_; \
public: \
    void set_##TYPE(const TYPE& value) { \
        std::lock_guard<std::mutex> lg(mutex_##TYPE##_); \
        TYPE##_ = value; \
    }; \
    TYPE get_##TYPE() const { \
        std::lock_guard<std::mutex> lg(mutex_##TYPE##_); \
        return TYPE##_; \
    } ;\
    const TYPE& get_##TYPE##ref() const{ \
        std::lock_guard<std::mutex> lg(mutex_##TYPE##_); \
        return TYPE##_; \
    };
class CfgFocus {
   public:
   
    static CfgFocus& get_ins();
    DECLARE_GET_SET(cfg_init_to_server);

    //adapt protocol to struct, hide protocol
    template <typename T>
    T load_cfg_by_path(const std::string& in_path);

    template <typename T>
    T load_cfg_by_iostream(std::ifstream& instream);

    template <typename T>
    T load_cfg_by_content(const std::string& in_content);

private:
    CfgFocus() = default;
    virtual ~CfgFocus() = default;
};

inline CfgFocus& CfgFocus::get_ins() {
    static CfgFocus cfg_focus{};
    return cfg_focus;
}

template <>
inline cfg_init_to_server CfgFocus::load_cfg_by_path(const std::string& in_path) {
    // auto toml_from_txt = toml::parse(in_path);
    // return toml::from<cfg_init_to_server>::from_toml(toml_from_txt);
    std::cout<<"template specialization " << in_path<<std::endl;
    return cfg_init_to_server{};
}
int main()
{
    cout<<"Hello World\n";
    CfgFocus::get_ins().load_cfg_by_path<cfg_init_to_server>("Hello");
    
    CfgFocus::get_ins().load_cfg_by_path<std::string>("Hello");
    
    return 0;
}
```
在此处使用模板函数的好处
1. 可以只声明不定义，若未定义调用则编译报错 目标函数未定义
2. 使用特化，可以在开发过程中持续添加
3. 保持函数名，返回值以及其他部分类型不同
4. 将特化函数嵌入到一些重载函数中


### refe
[github: 特化与重载](https://github.com/r00tk1ts/cpp-templates-2nd/blob/master/%E7%AC%AC16%E7%AB%A0%20%E7%89%B9%E5%8C%96%E4%B8%8E%E9%87%8D%E8%BD%BD.md)

### link
[[note.c++.template_specialization]]