---
id: n09b0a0e8b2hi7mp3h5wlbj
title: Template_recursion_and_indefinite_parameter
desc: ''
updated: 1699335358502
created: 1699322723752
---

#### sence
```c++
#include <iostream>

using namespace std;
#include <iostream>

// 定义一个嵌套的结构体
struct Inner {
    int inner_value;
    std::string inner_ss;
    bool inner_bool;
};
struct inner_inner{
    
};
struct Outer {
    int outer_value;
    Inner inner;
};

// 递归终止条件：当没有成员时，不再递归调用
template<typename T>
void print_struct_members(const T& t) {
    std::cout << "start" << std::endl;
    std::cout << "typeinfo: " << typeid(T).name() << std::endl;
    if(typeid(T) == typeid(std::string)){
        std::cout << "match the string typeid" << std::endl;
    }
    if(typeid(T) == typeid(int)){
        std::cout << "match the int typeid" << std::endl;
    }
    if(typeid(T) == typeid(bool)){
        std::cout << "match the bool typeid" << std::endl;
    }
    if(typeid(T) == typeid(int32_t)){
        std::cout << "match the int32_t typeid" << std::endl;
    }
    if(typeid(T) == typeid(int16_t)){
        std::cout << "match the int16_t typeid" << std::endl;
    }
    if(typeid(T) == typeid(int64_t)){
        std::cout << "match the int64_t typeid" << std::endl;
    }
    if(typeid(T) == typeid(int8_t)){
        std::cout << "match the int8_t typeid" << std::endl;
    }
    std::cout << t << std::endl;
    std::cout << "end" << std::endl;
}

// 递归调用，打印第一个成员，然后继续打印剩下的成员
template<typename T, typename... Args>
void print_struct_members(const T& t, const Args&... args) {
    // std::cout << t << std::endl;
    print_struct_members(t);
    print_struct_members(args...); // 递归调用
}

int main() {
    Outer obj{42, {10, "ss"}};
    print_struct_members(obj.outer_value, obj.inner.inner_value, obj.inner.inner_ss, obj.inner.inner_bool);

    return 0;
}
```
可变参模板参数 + 模板递归 可以 实现参数递归处理
```c++
//factory template
template<typename…  Args>
T* Instance(Args&&… args)
{
    return new T(std::forward<Args>(args)…);
}
A* pa = Instance<A>(1);
B* pb = Instance<B>(1,2);
```
可变参模板 可以 实现最外层工厂类实现的统一，去匹配不同参数的构造, 消除外层的重复代码


#### refe
[泛化之美--C++11可变模版参数的妙用](https://www.cnblogs.com/qicosmos/p/4325949.html)
[github: 第15章 模板实参推导](https://github.com/r00tk1ts/cpp-templates-2nd/blob/master/%E7%AC%AC15%E7%AB%A0%20%E6%A8%A1%E6%9D%BF%E5%AE%9E%E5%8F%82%E6%8E%A8%E5%AF%BC.md)