---
id: qcc094ndjswixyzqtov3a45
title: Default_argument_of_virtual
desc: ''
updated: 1697250865340
created: 1697242353921
---

#### case 1
```c++
#include <iostream>
#include <list>

class Base {
public:
    virtual void func() {
        std::cout << "Base: " << value << std::endl;
    }
};

class Derived : public Base {
public:
    virtual void func(int value = 2) override {
        std::cout << "Derived: " << value << std::endl;
    }
};
int main(){
    Base* ptr = new Derived;
    //specification
    //override function no default argument
    ptr->func(); 
}
```
**question:**
编译运行时发生什么？
>编译错误，无法匹配函数

#### case 2
```c++
#include <iostream>
#include <list>

class Base {
public:
    virtual void func(int value = 1) {
        std::cout << "Base: " << value << std::endl;
    }
};

class Derived : public Base {
public:

    virtual void func(int value = 2) override {
        std::cout << "Derived: " << value << std::endl;
    }
};
int main(){
    Base* ptr = new Derived;
    //specification
    //override function no default
    ptr->func(); // 输出 "Derived: 2"
}
```
**question:**
> 编译运行时，发生什么?
> 输出: Derived: 1

#### c++的默认参数的静态绑定与虚函数的动态绑定
**默认参数机制与静态绑定**
- [不要重写父类函数的默认参数](https://harttle.land/2015/09/04/effective-cpp-37.html)
1. 首要明确的: 默认参数 与 函数调用过程是分离的，不管有没有触发默认参数，函数调用时都要经过实参传参，即**默认参数机制与函数对象的自身结构无关的，影响的是与默认参数相关的函数匹配以及代码生成**
2. 默认参数的实现: **本质上是调用代码的重新生成**，即无参的函数调用代码，在匹配了默认参数的函数以后->生成了填入实参的函数调用 `p->func() => p->func(40, 60)`
3. 关键，编译期代码生成时，只能够确定当前指针对象的类型，所以仅能获取到当前指针类型对应的函数的默认参数值

**虚函数的动态绑定**
- [zhihu-虚函数机制](https://zhuanlan.zhihu.com/p/216258189)
1. 动态绑定的原理关键: 同一继承链上的类，**同一个类型的虚函数**在各自的虚函数表中的**占位索引**是一致的，也就意味着，动态绑定的**编译期基础**是:先确定虚函数类型->确定了调用时**索引**，最后生成虚函数指针变量通过指定索引调用虚函数的代码
2. 那么在运行时，虚函数指针变量切换为了实际对象的虚函数指针，调用了其对应的虚函数版本
> 总结: 编译期，对虚函数排座位，让虚函数类型与索引一致，**将虚函数代码调用处替换为指针调用**，运行时，进行虚函数指针交换赋值

**机制耦合**
1. 在直觉上，最终调用函数->对应函数定义->函数定义中的默认参数值
2. 实际上，默认参数的处理与虚函数调用不是同一阶段，是先在编译期确定了默认值重新生成了调用代码，然后在运行时进行虚函数调用，一旦覆写了基类虚函数的默认参数，这一行为将无意义，但却错误引导人们匹配默认参数值

**specifiction**
1. 定义虚函数时，不能带入默认参数，一旦基类虚函数出现默认参数，有可能后续的覆写就会继续带入并且覆盖，引发与期望冲突的致命bug