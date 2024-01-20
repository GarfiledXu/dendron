---
id: uqup6fo0jzvxk2qpttgog5e
title: Opengles
desc: ''
updated: 1705285049916
created: 1699450116764
---
### basic concept
#### coordinates system of opengl
**pixel coordinate**
坐标来源?
定义:
像素定义:
应用场景:
相关坐标转化:
[wiki: pixel](https://zh.wikipedia.org/zh-hans/%E5%83%8F%E7%B4%A0)
[zhihu: 像素是什么](https://zhuanlan.zhihu.com/p/578202748)

**window coordinates**

**normalize device coordinates**
坐标系确定了，那么坐标来源?基于其它坐标系的坐标转化(gl中通过窗口系统坐标系的坐标转化(viewport))?
转化为标准化设备坐标的意义?相比其它坐标系参与什么运算时能够带来优势?
何为标准化? 无论实际显示空间的长宽比，以及大小，都被映射为相同的坐标范围(一个 横向向右为x正方向，纵向向右为正y正方向，值范围为-1到1)

变换矩阵和坐标系的关系？

**clip and eye coordinates**

**world coordinates**

**object coordinates**


#### question
1. opengl 的实现原理
2. 理解opengl的状态机和上下文概念
    1. 为什么是状态机? 
    2. 为什么强调上下文?
    > 采用C-S架构，将控制图形渲染的行为抽象/建模为状态机，核心就是封装资源，在满足目标性能的情况下让用户端进行最精简化的操作/交互
    而 每一个渲染执行流，或者是渲染目标 则将对应/对标/映射 着一份 gl 内部管理的资源单位，这个资源单位即 context
3. opengl 的上下文 与线程 进场关系
4. opengl 不同上下文间的资源共享
5. opengl 的软硬件架构
6. opengl 上下文中的资源管理(shader, pragram等)
7. 如何将上一次的渲染结果作为下一次的渲染输入
8. 渲染结果是如何上屏的？
    > 通常来说屏幕的显示会有 gl 直接关联显示的缓冲区，那么 gl 渲染计算出来对应 buff 之后，要做的就是进行交换，即上屏，若不进行上屏，而是其他处理则是 离屏渲染

#### refe
[youtube: stanford 19. OpenGL ES](https://www.youtube.com/watch?v=_WcMe4Yj0NM&t=232s)
[超强总结！GPU 渲染管线和硬件架构](https://segmentfault.com/a/1190000042930791)
[]()