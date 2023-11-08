---
id: zr82dxk945c58occ6i52qi6
title: Event_system
desc: ''
updated: 1697815483547
created: 1697506550946
---

#### refe
- [offical: event system](https://doc.qt.io/qt-6/eventsandfilters.html)
- [offical: another look at events](https://doc.qt.io/archives/qq/qq11-events.html)
- [offical: QEvent](https://doc.qt.io/qt-5/qevent.html#Type-enum)
- [tao ge: 理解事件循环](https://jaredtao.github.io/2019/07/06/%E7%8E%A9%E8%BD%ACQt(5)-%E7%90%86%E8%A7%A3%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF/)
- [blog: 深入理解qt事件循环](https://www.cnblogs.com/mobeisiran/p/16992570.html)
- [blog: event dispatcher create](https://www.cnblogs.com/appsucc/p/12004967.html)
- [blog: qt 从0到1机制](https://xyz1001.xyz/articles/11422.html)
- [zhihu: 源码分析事件循环 event dispatcher](https://zhuanlan.zhihu.com/p/567780774)
- [blog: QT事件传递与事件过滤器](https://developer.aliyun.com/article/308755)
- [blog: qt event](http://blog.chinaunix.net/uid-25749806-id-335300.html)
- [blog: qt事件系统](http://blog.chinaunix.net/uid-25749806-id-335300.html)
- [blog: Qt widget事件传递顺序以及监听特定控件是否接收某个事件_qt时间响应的顺序-程序员宅基地](https://www.cxyzjd.com/article/liunanya/102650938)

#### abstract concept
event occurs
event deliver
event source
event type
event handler
> passive call, dependent the occurrence and delivered of event
spontaneous event
posted event
sent event




#### understand
- for cross platform
- passive call
- provide the specification(post: define->reigster->execute, send: define->execute) to bind event trigger and evnet handler
- convenience of coding, provide a large num of event and default handler and process loop/thread previously, just to define the handler or event which you need actually and to combine them.
- core idea of event system is to provide a way to `mark various known event` and `control corresponding response behavior`


#### handle flow and control the event propagated
**key: 在 post 以及 send 时，已经获取到了执行 handler 的obj，以及 handler，event 。每一个 object 都存储了父对象指针，通过这种对象结构，形成树状依赖关系，来进行内存管理和数据的传递回溯**
1. who execute the handler and ergodic widget structure?
2. the machanism of event propageted? when will the event be propageted? related fuhnction?
call `accept()` to stop propagate further <==> reimplement and not calling default handler 
call `ignore()` to continue propagate further <==> call default handler
passing up depends on default handler of qt
事件传递的过程实现就是一个 事件发送(send)或循环方(post->queue) 遍历和选择对象执行其event处理函数的过程


#### event dispatch with parent-child component
**mouse press event**
spontaneous event how to pass target object??
