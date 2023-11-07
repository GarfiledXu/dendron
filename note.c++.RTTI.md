---
id: 9gas2lqxjfd1369ipdymedl
title: RTTI
desc: ''
updated: 1697443931730
created: 1697436358007
---

#### c++ RTTI
- [blog-RTTI](https://nirvana1997.github.io/RTTI%E7%9A%84%E5%8E%9F%E7%90%86/)
- [jianshu-RTTI](https://www.jianshu.com/p/3b4a80adffa7)
- [zhihu-the meta system of qt](https://zhuanlan.zhihu.com/p/61303678)
- [blog-探究RTTI内存布局](https://www.cnblogs.com/zhyg6516/archive/2011/03/07/1971898.html)
- [csdn-探究RTTI内存布局](https://blog.csdn.net/qq_29523119/article/details/109108914)

#### full name
run time type info

#### typeid() operator
返回一个包含表达式类型信息的对象引用
##### member function: name()
返回类型名称
##### member function: before(\<constract typeinfo>)
返回是否当前类型在参数类型前，何为前？越基的类型越前

#### what is run time typeinfo?
在运行时，通过函数/运算符获取对象

#### dynamic_cast
用于转化继承链上的指针类型转换，操作基础就是RTTI，该操作符通过RTTI来确定类型转换是否安全，具体操作为
判断到类型不合适时，操作符返回空指针或者异常，来反馈错误
#### use case
一个基类数组中存储了各种被赋值了不同继承类的基类指针，如何在使用时通过基类指针来获取其真实的继承类型，进一步处理
```c++
Shape *shape1 = new Square();
Shape *shape2 = new Triangle();
Shape *shape3 = new Circle();
```
#### limitation
- 只有类型名称，信息有限
- 类型信息需要占用内存资源，编译期功能一旦开启，对于所有对象都将附加额外信息，灵活性不佳

