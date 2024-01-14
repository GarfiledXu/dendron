---
id: iknjzc7c717vxnn7e57k6vl
title: Any
desc: ''
updated: 1703578073395
created: 1700639189064
---

##### concept
多态
类型擦除
接口灵活性

##### question
how does the std::function implemente the polymorphism?

##### why need type strip
1. 对一类但并不同属于同一类型的数据结构，进行统一存储，即把在类型系统上有差异的数据结构，存放到一个容器中，比如设计了一个模板类，想要把同一个模板类实例化出的不同对象放在一个容器中，进行统一处理
> 繁琐方式：通过定义基类，手动实现不同的继承类，最后在 pop 数据时，进行类型恢复
> 设计：设计一个通用类型，容纳内建类型以及自定义类型，并且可以进行类型判断

##### 涉及特性
1. 模板 类型萃取 std::decay | std::enable_if std::type_index
2. 模板类型推导
3. std::decay 相关的原始类型和非原始类型
> 一个类型诞生后在使用过程中通常会延申出对应修饰类型，比如引用，左右值类型等
> 这些修饰类型都会被视为不同类型
> 故要提取出最基本的数据结构声明类型，就需要存在一个方式，对传入类型进行"清洗"
> 借助 std::decay 就能够实现去除类型中的引用符和cv符，获取到原始类型
4. typename
> typename 
> 相关编译器规则: 对于用于模板定义的依赖于模板参数的名称，只有在实例化的参数中存在这个类型名，或者这个名称前使用了typename关键字来修饰，编译器才会将该名称当成是类型。除了以上这两种情况，绝不会被当成是类型。=> 模板中的符号名称，在编译器角度存在着几种解释可能: 是个类型 | 是个对象符号 | 


##### scene
在定义虚函数接口时，无法确定完整的接口形态

### refe
[C++11中的std::function](https://www.cnblogs.com/paul-617/p/15675896.html)
[C++值多态：传统多态与类型擦除之间](https://www.cnblogs.com/jerry-fuyi/p/value_polymorphism.html)
[实现any的关键技术](https://www.cnblogs.com/qicosmos/p/3420095.html)

[知无涯之C++ typename的起源与用法::good](https://feihu.me/blog/2014/the-origin-and-usage-of-typename/)
[Where and why do I have to put the "template" and "typename" keywords?](https://stackoverflow.com/questions/610245/where-and-why-do-i-have-to-put-the-template-and-typename-keywords/613132#613132)