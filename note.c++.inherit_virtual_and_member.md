---
id: x3ue6km5bcnxl3lgzz45h96
title: Inherit_virtual_and_member
desc: ''
updated: 1696922682461
created: 1696921298138
---

#### use case of virtual
- directly call virtual outside to the class
- call virtual method inside to class member function, then derived class just modify the virtual and still call the member of base class outside
```c++
#include <stdio.h>
#include <iostream>
class Base {
public:
    virtual void foo() {
        std::cout << "Base::foo()" << std::endl;
    }
    void bar() {
        std::cout << "Base::bar() calling foo()" << std::endl;
        foo(); // 这里调用的是虚函数 foo()
    }
};
class Derived : public Base {
public:
    void foo() override {
        std::cout << "Derived::foo()" << std::endl;
    }
};
int main() {
    Derived d;
    Base& b = d; // 向上转型，将 Derived 对象引用绑定到 Base 类型的引用上

    b.bar(); // 调用 Base 类的 bar() 函数
    printf("enter second!\n");
    
    Base* bb = new Derived{};
    bb->bar();
    return 0;
}
```
- call virtual method inside to class virtual member function
```c++
#include <stdio.h>
#include <iostream>
class Base {
public:
    virtual void foo() {
        std::cout << "Base::foo()" << std::endl;
    }

    virtual void bar() {
        std::cout << "Base::bar() calling foo()" << std::endl;
        foo(); // 这里调用的是虚函数 foo()
    }
};

class Derived : public Base {
public:
    void foo() override {
        std::cout << "Derived::foo()" << std::endl;
    }
    void bar() {
        std::cout << "Derived::bar() calling foo()" << std::endl;
        foo(); // 这里调用的是虚函数 foo()
        Base::foo();
    }
};

int main() {
    Derived d;
    Base& b = d; // 向上转型，将 Derived 对象引用绑定到 Base 类型的引用上

    b.bar(); // 调用 Base 类的 bar() 函数
    printf("enter second!\n");
    
    Base* bb = new Derived{};
    bb->bar();
    return 0;
}
```


#### example
**sink of spdlog**