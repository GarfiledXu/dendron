---
id: xvie8mb76yech4qerse0f9k
title: Signal_slo
desc: ''
updated: 1697815621948
created: 1697617424538
---

#### refe 
- [zhihu: Qt源码学习系列之信号与槽（二）](https://zhuanlan.zhihu.com/p/570163788)
- [csdn: qt 元对象 和moc原理](https://blog.csdn.net/LIJIWEI0611/article/details/115056153)
- [blog-Qt Meta Object System-元对象系统](https://www.cnblogs.com/tgycoder/p/5384676.html)
- [blog: Qt从0到1之机制篇-信号槽](https://xyz1001.xyz/articles/19581.html)
- [blog: QT信号槽的实现](https://www.devbean.net/2012/12/how-qt-signals-and-slots-work/)
- [blog: Qt信号槽原理(调用)](https://www.jianshu.com/p/b981591c532c)

#### third implementation
- [github-signals](https://github.com/pbhogan/Signals/tree/master)

#### source code browser
- [codebrowser: QObject](https://codebrowser.dev/qt5/qtbase/src/corelib/kernel/qeventloop.cpp.html#_ZN10QEventLoop4exitEi)
- [codebrowser: QMetaObject](https://codebrowser.dev/qt5/qtbase/src/corelib/kernel/qmetaobject.h.html)

#### moc 扩展流程
- 查找 Q_OBJECT 宏
- 生成对应 moc_xx.cpp
- 解析当前类，生成


#### understand
- metaObject 解析源码生成数据进行组合存储(以定义数组方式), 生成对应的get函数(以强解释指针的方式将数组映射到对应结构体)
- metasignalcall 将所有存在函数存表，并将connect连接的函数关系存表，重实现信号和槽函数，通过索引或字符串进行查表调用