---
id: 65jceb5duekfq1v7pf5qp5k
title: Qt
desc: ''
updated: 1739423740812
created: 1739416736935
---

#### qt 五大模块
1. QtCore: 非gui类，提供线程，事件处理机制等功能
2. QtGui: 底层图形操作，窗口管理，事件处理，绘制和颜色
3. QtWidgets: 提供高级的gui组件，按钮，文本框，对话框
4. QtNetwork
5. QtSql

#### 讲解一下Qt的信号与槽机制，以及事件循环机制
一种是同步的调用，类似于回调函数
一种是异步的调用，投递到事件循环中等待被执行
依赖于元对象系统，在编译期间，进行代码生成，添加到所在对象的数据结构中，类似于哈希表
在运行时，如果出发了信号，就会遍历此前存储的信息，判断条件满足后进行对象调用

用户输入，系统请求，ui的重绘都属于事件，触发事件后主线程ui线程进行事件处理，重绘
其他线程触发的修改ui的操作，应该通过信号和槽来触发
而部分事件比如窗口，外设事件的生成是通过操作系统来生成执行，投入到框架的事件循环中的

#### 解释一下Qt中的模型/视图架构
model: 负责数据的 存储，读取，和通知
view: 获取数据，然后进行展示
control: 负责处理用户的交互操作

#### Qt如何实现多语言机制
    使用QTranslator，来加载.qm文件

#### Qt程序常见的事件类型
    1. 键盘事件

#### 死锁如何产生的
      1. 存在多个锁，并按一定顺序一次加锁，代码上，线程1，先持有锁A，然后持有锁B，线程2，先持有锁B，然后持有锁A，二者条件互相依赖，循环，导致死锁
        解决方案: 可以使用std::lock()可以一次性家多个互斥锁,确保所有线程的多个锁加锁顺序相同
      2. 比如资源继承等原因,导致的死锁,线程阻塞

