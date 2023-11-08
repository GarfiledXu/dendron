---
id: sc4btxb7eow7ipskhlau6we
title: Inherit_permission_modifer
desc: ''
updated: 1699407712634
created: 1696902078607
---
#### refe
[zhihu: C++的三种继承方式](https://zhuanlan.zhihu.com/p/356580826)
#### access control and level
access enable = call function no compile error == 可见性
access fail = call function compile error == 不可见
所谓权限控制, 完整描述应该是: 
类成员的访问权限控制机制，编写源代码定义类时，关键字指定成员的权限，然后在由编译器在编译期，进行权限判定，对源码中涉及到成员函数调用的位置进行权限判定。判定的流程:
分析此时访问位置，即call 符号的位置
确定此时符号此时的权限，成员符号受其他关键字以及操作的影响，如继承，友元，域解析操作等，确定一个引用成员处，成员的权限，要清楚此时成员的隶属关系，而不是单单的定义时权限
判定当前权限在当前位置是否合理，否则编译报错

#### access position
1. 
2. 



#### inherit and access permission modifer and constructor and singleton
**scene: define an base singleton, the constructor of current class is private, if inherit a class from current class, just override one virtual**
**specification:**
- the precondition of virtual mehtod that is the permission modifer is not private!!

#### access permission modifer with override virtual method action?
- 虚函数的声明是否受到权限符的限制?
  - 基类中private的函数能否声明为虚函数
- 继承类重写虚函数后能否修改权限符?



#### constructor and destructor with private and protected access permission modifer and inheritance?
- 析构和构造在调用时，编译器是否会进行权限符检查?
    - 析构与构造本质上就是成员函数，所以同样受权限控制
    - 通常情况下，析构与构造都是在类外调用，那么就要求构造函数与析构函数 public 权限
    - 类内情况: 静态成员函数、成员函数、构造、析构等函数作用域内

- 声明 构造和析构 函数 为 private 权限？
  - 构造声明为private的作用？禁止外部所有的构造实例化，并将调用

- 析构和构造，特殊权限符的控制应用?
  - 单例
  - 框架类 + 友元

- 权限符的判断是在编译期还是在运行时?
  > 权限符就是编译器在编译期对成员变量以及成员函数相关代码进行权限符解析，判断合法性，不合法则编译报错, 所以任何与权限符相关的代码都会进行解析判断
- 编译器对权限符的解析流程?
    1. 获取所有相关对象(函数，变量，类)的访问权限
    2. 判断此时解析代码所在的上下文 (在类的成员函数中/类外/友元/继承), 即分场景判断
    3. 错误则编译报错
- 一个对象的析构与构造在被执行时，是否有权限要求? 权限符能否声明为非public?(构造函数与析构函数的权限符规则?)
- 继承时 权限符的规则?


#### keyword of the class inheritance decalred
- final
- access permission modifer
- default behavior
```c++
class derived final : public base{

};
class derived : base{

};
```
1. what happen? 
> 编译器在成员调用处，判断在直接相关类中的权限(即判断引用该成员何处的权限修饰符)

    1. public 继承，所有基类成员权限 在继承类中保持原样, 在继承类中的可见性不变
    2. protected 继承，所有基类成员高于protected的权限 在继承类中降级为protected，等于低于的不动,在继承类中的可见性不变
    3. private 继承， 所有基类成员高于private的权限 在继承类中降级为private，在继承类中的可见性不变

2. 继承后的权限变更与成员可访问性
> 准确来说，这里要归纳讨论的是 在继承场景下来，在继承后 在继承类外部通过继承来访问基类成员，和 在继承类的继承者内部访问基类成员 时，基类成员的可见性

    1. 第一点明确的是，在当前类内部成员的 权限修饰符，决定了成员对 相对于当前位置的外围/外部位置的可见性，如基类对继承类的可见性，以及基类对类外部的可见性，(基类的继承类以及对象所处函数调用处，都属于外部)

    2. 所以，继承修饰符的影响效果，体现在 控制 继承类的外部对继承类的基类成员的访问性，而继承类对基类成员的访问性无关，这是由基类成员的访问权限修饰符决定的。`即成员在外部的可见性，由该成员在直接类中的修饰符决定，不是由成员在基类中的修饰符`。要明确一个成员在某个位置的访问行，首要的是确定成员引用时相关的直接类，然后确定成员在该类的修饰符。

    3. 控制思路的总结: 要控制基类成员在继承类外部的访问，开启private继承，继承类的外部没有任何访问基类成员的可能，开启portected继承则允许继承类的继承类访问继承类的基类protected及以上成员, 开启public继承，则允许当前继承类的外部调用访问基类public成员和继承类的继承类访问继承类的基类
    
3. specification
> 继承时，继承权限控制的思路
    1. 权限控制的核心无非就是，取一个满足当前需求的权限最小值， 防止成员滥用，保护代码
    2. 即分析当前继承权限对应的实际效果是否需要, 即从private -> public

```c++
```

#### =default and delete keyword and access permission modifier of constructor and desturctor
1. 能否直接delete 构造函数 和 析构函数？
> 只要是函数符号都可以加入delete关键字进行删除，在定义时关键字使用的地方不会有检查函数类型的逻辑
> 但在实例化时，编译器会判断 构造函数 和 析构函数在调用处作用域 权限是否合法，实现是否存在，否则编译报错
2. specification
> 所以 delete 对于析构函数是无意义的，因为其形式固定，必须存在，但对于构造函数存在多种重载形式，根据具体需要是否删除默认构造
3. default 的构造函数和析构函数对于成员的初始化和析构规则
> 
