---
id: pqwmetfqqcn71zjr2jr429g
title: Singleton
desc: ''
updated: 1704528200898
created: 1692199421329
---

### singleton
----------------
保持唯一性的全局变量
#### reason
- 从经验中获取的现成抽象
- 将常见解决方案细节规则化
- 抽象出概念，进行指代

#### why is always initialize pointer, rather than construct obj
- 实现单例模式，使用静态对象，本质上就是将目标对象生命周期交由语言逻辑
- 而使用指针进行分配，可以惰性加载，即用时new，退时delete，控制更可靠

#### singleton inner or singleton template
- 创建全局唯一的类型及实例
- 创建全局唯一的实例

#### so/dll singleton static how to cross compile unit

#### constract to global variable
**对比全局变量，单例的好处是，将类的静态成员函数作为全局变量的代理(而静态成员函数不需要在类外定义) 相比类的静态变量必须在类外定义在代码编写上存在一定的遍历，尤其是在定义全局变量时不需要专门导出声明，进行管理**
**举一反三： 能否通过定义静态函数并返回静态变量的方式来定义类内静态变量，这样就不需要进行在类外进行定义**

[[Singleton Cross So|question.c++.singleton-cross-so]]
