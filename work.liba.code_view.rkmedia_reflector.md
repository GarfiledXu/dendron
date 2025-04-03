---
id: rj7kzyuejirjsipi5xq0azi
title: Rkmedia_reflector
desc: ''
updated: 1743064198590
created: 1742973993141
---

## rkmedia 中工厂+反射机制分析

```c++

#ifndef EASYMEDIA_REFLECTOR_H_
#define EASYMEDIA_REFLECTOR_H_

#include <map>
#include <memory>
#include <string>

#include "utils.h"

// all come the external interface
#define REFLECTOR(PRODUCT) PRODUCT##Reflector

// use internally
#define FACTORY(PRODUCT) PRODUCT##Factory

// macro of declare reflector
#define DECLARE_REFLECTOR(PRODUCT)                                             \
  class PRODUCT;                                                               \
  class PRODUCT##Factory;                                                      \
  class _API PRODUCT##Reflector {                                              \
  public:                                                                      \
    static const char *FindFirstMatchIdentifier(const char *rules);            \
    static bool IsMatch(const char *identifier, const char *rules);            \
    template <class T>                                                         \
    static std::shared_ptr<T> Create(const char *request,                      \
                                     const char *param = nullptr) {            \
      if (!IsDerived<T, PRODUCT>::Result) {                                    \
        printf("The template class type is not derived of required type\n");   \
        return nullptr;                                                        \
      }                                                                        \
                                                                               \
      const char *identifier = PRODUCT##Factory::Parse(request);               \
      if (!identifier)                                                         \
        return nullptr;                                                        \
                                                                               \
      auto it = factories.find(identifier);                                    \
      if (it != factories.end()) {                                             \
        const PRODUCT##Factory *f = it->second;                                \
        if (!T::Compatible(f)) {                                               \
          printf("%s is not compatible with the template\n", request);         \
          return nullptr;                                                      \
        }                                                                      \
        return std::static_pointer_cast<T>(                                    \
            const_cast<PRODUCT##Factory *>(f)->NewProduct(param));             \
      }                                                                        \
      printf("%s is not Integrated\n", request);                               \
      return nullptr;                                                          \
    }                                                                          \
    static void RegisterFactory(std::string identifier,                        \
                                const PRODUCT##Factory *factory);              \
    static void DumpFactories();                                               \
                                                                               \
  private:                                                                     \
    PRODUCT##Reflector() = default;                                            \
    ~PRODUCT##Reflector() = default;                                           \
    PRODUCT##Reflector(const PRODUCT##Reflector &) = delete;                   \
    PRODUCT##Reflector &operator=(const PRODUCT##Reflector &) = delete;        \
                                                                               \
    static std::map<std::string, const PRODUCT##Factory *> factories;          \
  };

// macro of define reflector
#define DEFINE_REFLECTOR(PRODUCT)                                              \
  std::map<std::string, const PRODUCT##Factory *>                              \
      PRODUCT##Reflector::factories;                                           \
  const char *PRODUCT##Reflector::FindFirstMatchIdentifier(                    \
      const char *rules) {                                                     \
    for (auto &it : factories) {                                               \
      const PRODUCT##Factory *f = it.second;                                   \
      if (f->AcceptRules(rules))                                               \
        return it.first.c_str();                                               \
    }                                                                          \
    return nullptr;                                                            \
  }                                                                            \
  bool PRODUCT##Reflector::IsMatch(const char *identifier,                     \
                                   const char *rules) {                        \
    auto it = factories.find(identifier);                                      \
    if (it == factories.end()) {                                               \
      printf("%s is not Integrated\n", identifier);                            \
      return false;                                                            \
    }                                                                          \
    return it->second->AcceptRules(rules);                                     \
  }                                                                            \
  void PRODUCT##Reflector::RegisterFactory(std::string identifier,             \
                                           const PRODUCT##Factory *factory) {  \
    auto it = factories.find(identifier);                                      \
    if (it == factories.end()) {                                               \
      factories[identifier] = factory;                                         \
      printf("register factory : %s\n", identifier.c_str());                   \
    } else                                                                     \
      printf("repeated identifier : %s\n", identifier.c_str());                \
  }                                                                            \
  void PRODUCT##Reflector::DumpFactories() {                                   \
    printf("\n%s:\n", #PRODUCT);                                               \
    for (auto &it : factories) {                                               \
      printf(" %s", it.first.c_str());                                         \
    }                                                                          \
    printf("\n\n");                                                            \
  }

// macro of declare base factory
#define DECLARE_FACTORY(PRODUCT)                                               \
  class PRODUCT;                                                               \
  class PRODUCT##Factory {                                                     \
  public:                                                                      \
    virtual const char *Identifier() const = 0;                                \
    static _API const char *Parse(const char *request);                        \
    virtual std::shared_ptr<PRODUCT> NewProduct(const char *param) = 0;        \
    bool AcceptRules(const char *rules) const {                                \
      std::map<std::string, std::string> map;                                  \
      if (!parse_media_param_map(rules, map))                                  \
        return false;                                                          \
      return AcceptRules(map);                                                 \
    }                                                                          \
    virtual bool                                                               \
    AcceptRules(const std::map<std::string, std::string> &map) const = 0;      \
                                                                               \
  protected:                                                                   \
    PRODUCT##Factory() = default;                                              \
    virtual ~PRODUCT##Factory() = default;                                     \
                                                                               \
  private:                                                                     \
    PRODUCT##Factory(const PRODUCT##Factory &) = delete;                       \
    PRODUCT##Factory &operator=(const PRODUCT##Factory &) = delete;            \
  };

#define DEFINE_FACTORY_COMMON_PARSE(PRODUCT)                                   \
  const char *PRODUCT##Factory::Parse(const char *request) { return request; }

#define FACTORY_IDENTIFIER_DEFINITION(IDENTIFIER)                              \
  const char *Identifier() const override { return IDENTIFIER; }

#define FACTORY_INSTANCE_DEFINITION(FACTORY)                                   \
  static const FACTORY &Instance() {                                           \
    static const FACTORY object;                                               \
    return object;                                                             \
  }

#define FACTORY_REGISTER(FACTORY, REFLECTOR, FINAL_EXPOSE_PRODUCT)             \
  class Register_##FACTORY {                                                   \
  public:                                                                      \
    Register_##FACTORY() {                                                     \
      const FACTORY &obj = FACTORY::Instance();                                \
      REFLECTOR::RegisterFactory(obj.Identifier(), &obj);                      \
      FINAL_EXPOSE_PRODUCT::RegisterFactory(&obj);                             \
    }                                                                          \
  };                                                                           \
  Register_##FACTORY reg_##FACTORY;

#define DEFINE_ERR_GETSET()                                                    \
protected:                                                                     \
  class ErrGetSet {                                                            \
  public:                                                                      \
    ErrGetSet() : err_val(0) {}                                                \
    void Set(int val) { err_val = val; }                                       \
    int Get() { return err_val; }                                              \
                                                                               \
  private:                                                                     \
    int err_val;                                                               \
  };                                                                           \
  ErrGetSet IErr;                                                              \
                                                                               \
public:                                                                        \
  void SetError(int val) { IErr.Set(val); }                                    \
  int GetError() { return IErr.Get(); }

#define DECLARE_PART_FINAL_EXPOSE_PRODUCT(PRODUCT)                             \
public:                                                                        \
  static bool Compatible(const PRODUCT##Factory *factory);                     \
  static void RegisterFactory(const PRODUCT##Factory *factory);                \
                                                                               \
private:                                                                       \
  static std::list<const PRODUCT##Factory *> compatiable_factories;

#define DEFINE_PART_FINAL_EXPOSE_PRODUCT(FINAL_EXPOSE_PRODUCT, PRODUCT)        \
  std::list<const PRODUCT##Factory *>                                          \
      FINAL_EXPOSE_PRODUCT::compatiable_factories;                             \
  bool FINAL_EXPOSE_PRODUCT::Compatible(const PRODUCT##Factory *factory) {     \
    auto it = std::find(compatiable_factories.begin(),                         \
                        compatiable_factories.end(), factory);                 \
    if (it != compatiable_factories.end())                                     \
      return true;                                                             \
    return false;                                                              \
  }                                                                            \
  void FINAL_EXPOSE_PRODUCT::RegisterFactory(                                  \
      const PRODUCT##Factory *factory) {                                       \
    auto it = std::find(compatiable_factories.begin(),                         \
                        compatiable_factories.end(), factory);                 \
    if (it == compatiable_factories.end())                                     \
      compatiable_factories.push_back(factory);                                \
  }

// macro of define child factory
// EXTRA_CODE: extra class declaration
#define DEFINE_CHILD_FACTORY(REAL_PRODUCT, IDENTIFIER, FINAL_EXPOSE_PRODUCT,   \
                             PRODUCT, EXTRA_CODE)                              \
  class REAL_PRODUCT##Factory : public PRODUCT##Factory {                      \
  public:                                                                      \
    FACTORY_IDENTIFIER_DEFINITION(IDENTIFIER)                                  \
    std::shared_ptr<PRODUCT> NewProduct(const char *param) override;           \
    FACTORY_INSTANCE_DEFINITION(REAL_PRODUCT##Factory)                         \
                                                                               \
  private:                                                                     \
    REAL_PRODUCT##Factory() = default;                                         \
    ~REAL_PRODUCT##Factory() = default;                                        \
    EXTRA_CODE                                                                 \
  };                                                                           \
                                                                               \
  FACTORY_REGISTER(REAL_PRODUCT##Factory, PRODUCT##Reflector,                  \
                   FINAL_EXPOSE_PRODUCT)

#endif // EASYMEDIA_REFLECTOR_H_
```

### why is it called reflector?

### 反射机制和工厂模式的关系?

### 动态性的体现

### 对比gtest的用例注册机制

### libthird_media.so 符号分析

```bash
#调试命令
readelf
objdump
nm
ldd

## ?? 如何知道当前so需要依赖哪些外部全局符号，实体
#使用readelf时，符号名称会包含[...] 即被截断，需使用其他命令进行符号查看

## 假设so包含全局符号对象，在编译时链接so和运行时动态加载so，有什么区别?
#在pc linux上并不能通过ldd查看交叉编译生成的so连接关系
List Dynamic Dependencies，依赖于ld.so运行时解析
```


```bash
初步的原理可知，是基于全局对象的构造来进行reflector和factory的注册，那么这里的构造顺序如何保证呢，由于全局对象是存在管理关系的

本质上是利用了c++全局符号对象的初始化构造机制，来完成运行时的注入 (字符串+对象)，而不是依赖于编译时的符号调用
所以问题变为了 .so 内的全局符号对象，在不同的so加载链接方式下，执行的时机?(c++ 动态库内的全局变量的构造时机)


!!! 命名空间是否会影响 全局变量的初始化，全局作用域下的会在加载时构造，而命名空间下的全局变量呢？
```

### ref

- [C++ 通过模版工厂实现 简单反射机制](https://blog.csdn.net/Z_Stand/article/details/117796155)

[[note.c++.global_variable_init_on_dynamic.md]]
