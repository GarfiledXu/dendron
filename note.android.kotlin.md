---
id: rmew7bghf0t6j6ymu1o7k4q
title: Kotlin
desc: ''
updated: 1716907377912
created: 1716688502481
---

#### refe
[android: kotlin top](https://developer.android.com/kotlin?hl=zh-cn)
[android: 面向编程人员的 Kotlin 训练营](https://developer.android.com/courses/kotlin-bootcamp/overview?hl=zh-cn)
[android:Jetpack Compose 基础知识](https://developer.android.com/codelabs/jetpack-compose-basics?hl=zh-cn#0)
[android: kotlin 基础知识](https://developer.android.com/codelabs/android-development-kotlin-1.2?hl=zh-cn&continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-development-with-kotlin-1%3Fhl%3Dzh-cn%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fandroid-development-kotlin-1.2#0)
[将 Kotlin 添加到现有应用](https://developer.android.com/kotlin/add-kotlin?hl=zh-cn)
[kotlin 官方 ch](https://book.kotlincn.net/text/android-overview.html)
[kotlin 官方 en](https://kotlinlang.org/docs/android-overview.html)


#### 语法主动需求列表
- [ ] 基础类型名称
  - [ ] 数值 10进制 16进制 8进制表示
  - [ ] 布尔类型以及布尔值
  - [ ] 字符
  - [ ] 字符串
    - [ ] 格式化打印
  - [ ] 基础类型数组和自定义类型数组
  - [ ] 类型判断
  - [ ] 类型转化
- [ ] 定义一个变量
- [ ] 给变量赋值
- [ ] 定义一个函数
  - [ ] 如何申明返回值
  - [ ] 如何申明参数类型
- [ ] 定义一个类
  - [ ] 如何申明成员变量
  - [ ] 定义内部类
  - [ ] 接口定义
  - [ ] 类的继承与接口的实现
  - [ ] 属性控制修饰符
  - [ ] 泛型
  - [ ] 类型别名
- [ ] 给类赋值  
- [ ] 操作符重载
- [ ] 基础的流程控制
  - [ ] 条件表达式
  - [ ] 循环
  - [ ] 大妈跳转
  - [ ] 数组遍历语法
- [ ] 闭包
  - [ ] 匿名内部类
  - [ ] lambda表达式
- [ ] 异常
  - [ ] 声明
  - [ ] 抛出
  - [ ] 捕获
- [ ] null 对象处理
- [ ] 注解的定义与使用
- [ ] 枚举的定义与使用
- [ ] 线程
  - [ ] 线程的定义与使用
  - [ ] 协程的定义与使用
- [ ] 功能
  - [ ] 获取系统时间
  - [ ] 文件io
- [ ] native jni
- [ ] specific concept
- [ ] 包的导入导出

#### 语法糖
- [null safety](https://kotlinlang.org/docs/null-safety.html#the-operator)
- kotlin 中的 狗皮膏药 ? 修饰，真看得老子晕头转向
  - 首先所有类型默认非空，类型紧贴的?, 给类型增加 可空属性，将被编译器用于编译期检查，类似在编译期检查变量访问修饰符，来报错违法的null值操作，比如将 一个非空类型对象赋值给 val 变量，将引发编译报错
  - java 是默认可空，除非使用注解进行非空检查，缺点在于这通常在运行时生效，而 kotlin 意在默认非空，要可空则需添加 ? 修饰
  
- 替代三元运算符, 使用 if 表达式, kotlin的if表达式可以默认返回分支中最后一个表达式的值
  ```kotlin
 
  ```
- 更加灵活和平面的 when 表达式, 
 when 后使用 {} 来攘括所有相关内容
  当when表达式尾随括号进行传参时，->作为一个匹配符号，对->前的表达式与传参值进行匹配
  当when表达式没有括号传参时，->作为一个布尔条件判断符号，判断->前的表达式是否为true
  通过 , 号将相同处理的待判断的表达式放置在一起
  表达式可以包含哪些操作:
    1. 使用 `in` 关键字 判断范围，使用 `..` 关键字 表示范围, 检查值是否在范围中: 
       1. in 1..10 -> print("x is in the range")
       2. !in 10..20 -> print("x is outside the range")
    2. 使用 `is` 关键字 判断类型

  ```kotlin
  //switch 用法
  when (<待匹配参数>) {
    <匹配参数1> -> <匹配成功，执行表达式>
    <匹配参数2> -> <匹配成功, 执行表达式>
    else -> {
      <最后一括号所有其它情况，执行表达式>
    }
  }
  //if 条件表达式用法
  
  
  ```

- 可执行对象 类型表示
  以下例子中 定义了 一个可执行对象 sum，定义结构为 `val <对象名>: <可执行对象类型> = <lambda表达式>`
  即声明类型+赋值对象
  1. val sum: (Int, Int) -> Int 
  2. {a: Int, b: Int -> 连续的函数体}
  ```kotlin
  val sum: (Int, Int) -> Int = { a: Int, b: Int ->
      val result = a + b
      result
  }

  ```
- 循环 loop, 比较传统的 for 和 while 循环
  - for 通常使用 `in` 关键字进行遍历
  ```kotlin
  ```
- 替代空值检查, 使用 elvis 表达式 , 可以理解为 空则表达式
  ```kotlin
  
  
  ```
- java null 不安全 与 异常
  java中默认对象可空，也就意味着会发生 null 对象引用，也就意味着必然会发生 null 引用异常，而异常的发生就代表着 非期望行为，即 null 不安全，当然直接允许空指针/null对象异常的发生，也是一种调试方式，至少可以明确的发现null对象位置
- 对于 null对象 的处理，一种是需要在检查到非null时进行重新赋值，一种是只在非null时处理，null时忽略，这在kotlin的语法中都有对应的语法加强 最后一种就是必须判断到，直接抛出，以异常方式将错误的发生传递出来，即null断言  ?. !!. ?:都是运算符 operator 
- kotlin 的 可空类型 和 非空类型, boxing 和 unboxing
  **kotlin 中在语法/关键字层面的可空类型修饰，目的是提供给编译器进行空值检查**
  **什么是 null 安全: 即kotlin中在进行对象引用操作时，有机制确保 异常的不发生，以及异常的判断**
  ```kotlin
  ```
- kotlin 的类型转换
  **安全类型转换中的安全是指什么?**
  ```kotlin
  ```
- kotlin 的类型判断
  **判断是否为空直接用**

- kotlin 的定义式风格
  **与c++ 以及java 差别较大的一点，类型的声明通常是后置的，而不是以类型起手，也就意味着定义变量，函数，与类，都是直接关键字起手**

#### 函数
- 单表达式函数
  - 当函数体内只有一个表达式时，直接在函数声明之后 用 `= <单表达式>`的方式来替代 `{ 函数体 + <return语句>}`的形式
  ```kotlin
  //原形式
  fun get_condition_str(in_count: Int): string {
    if(in_count > total_count){
      return "success"
    }
    else{
      return "fail"
    }
  }
  //语法糖简化
  fun get_condition_str(int_count: Int):string = if(in_count > total_count){
    return "success"
  }
  else{
    return "fail"
  }
  ```
- 为什么叫表达式，而不是关键字?
  - `return` `break` `continue` 在官方文档中都称为结构跳转表达式，因为他们可以作为表达式使用，可以作为返回值
  - 比如在 ?: 中使用，当左边为空时，右边填 return `in_var?:null`
#### kotlin 中的类设计
#### kotlin 中抽象的 @ 符号
- 注解
- 自定义符号名称后尾随@，`abc@` ，可以作为label

#### the design of kotlin
#### time count
- [x] 罗列初步计划 20min
- [ ] 环境搭建
- [ ] 代码转化对照
- [ ] pc原生环境搭建
- [ ] android 环境搭建
- [ ] 习惯规范
- [ ] java 语法对比总结
- [ ] 编码规范以及命名规范