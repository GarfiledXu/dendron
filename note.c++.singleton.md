---
id: pqwmetfqqcn71zjr2jr429g
title: Singleton
desc: ''
updated: 1692237876912
created: 1692199421329
---

### singleton
----------------
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

[[Singleton Cross So|question.c++.singleton-cross-so]]
