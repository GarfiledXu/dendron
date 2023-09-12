---
id: d74lkdxhno90ce4y273oalv
title: GC-reuse-obj
desc: ''
updated: 1694484868016
created: 1694484574207
---

#### for avoid GC by new object frequently
--------------
1. predefine new object in upper scope
    > 缺点，代码繁琐，需要在代码逻辑外加上预实例化对象，并且经常涉及到传参
2. use objectpool
   > 理想情况，直接通过类型的静态方法引用，而不是对象引用，来直接获取到