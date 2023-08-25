---
id: l59a62v9u29hcyrrkhb5peq
title: Spdlog
desc: ''
updated: 1692768938090
created: 1692712451015
---

### spdlog
-------------------------
#### reference
****
- [blog-spdlog struct](https://www.cnblogs.com/shuqin/p/12214439.html)
- [github](https://github.com/gabime/spdlog)

#### work flow
****
-  


#### features
****
-  



#### concept 
**logger**
**sinks**


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