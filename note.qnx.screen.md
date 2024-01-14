---
id: 3gdjxeljai26y5p4m780t46
title: Screen
desc: ''
updated: 1704866321617
created: 1703726178852
---


#### screen and EGL and opengl
都属于图形框架, 图形 API
screen 是 qnx 上的原生图形框架，本身是属于系统的图形抽象层，作为 qnx 上最顶层的图形资源管理和显示的 API
> 提供全面且底层的， 承接最顶层的图形数据管理/graphic management，以及图形显示显示/display, 图形渲染/rendering

EGL 由 Khronos 组织推出 核心价值 就是作为 中间层/移植层 向上承接 系统原生图形API 向下对接 Khronos 图形渲染API
>核心功能在于 为opengl的rendering提供 数据管理/图形资源管理/graphic resource managerment, 可以认为是提搞 opengl的移植性而抽象的layer

opengl/opengles/openvg 同样由 Khronos 组织推出 核心价值 作为一个 跨平台的通用的 GPU 硬件渲染框架 核心功能在于 将输入的图形数据计算为待display的图像数据 即 rendering 在于对图形数据的计算和转化
> 核心 关注于 数据处理

#### 在多进程场景下，如何进行图像融合，以及多线程情况下
通过screen 创建windows时进行图层zorder优先级设置，越大的图层显示在前面，同等zorder编号，越靠后启动的图层优先级高,包括同个线程中的两个渲染线程，若id相同后起的覆盖

#### problem to be solved
- [ ] screen 最前面图层透明区域 无法接收到event

#### windows buff display pipeline 以及event间的层级关系
1. 窗口: 矩形的图形数据buff的持有者, 不同类型的窗口拥有不同的特性 程序窗口，子窗口，嵌入式窗口等
    > 
2. display: 该抽象对象指向 呈现/显示 图像的物理设备，即显示屏
3. groups: 用于组织 管理n个窗口对象的一种抽象属性
4. 缓冲区共享: 让几个窗口 持有共同的 buff

#### refe
[offical: qnx700](https://www.qnx.com/developers/docs/7.0.0/#com.qnx.doc.screen/topic/manual/cscreen_about.html)
[offical: qnx660](https://www.qnx.com/developers/docs/6.6.0.update/index_frames.html?q=/developers/docs/6.6.0.update/com.qnx.doc.screen/topic/manual/cscreen_compmanager.html)
[ppt: qnx software systems](https://www.slideshare.net/robertemmanuelmayssat/qnx-software-systems)
[csdn: qnx-screen](https://blog.csdn.net/Suixing_yuan/article/details/115145756)
[blog: android AHardwareBuffer](https://juejin.cn/post/7309133107228803083)
[blog: qnx-screen hardware rendering](https://juejin.cn/post/7297057358726430732)
[blog: qnx-screen software rendering](https://juejin.cn/post/7296016127553142825)
[blog: QNX-Screen官方文档理解](https://blog.csdn.net/weixin_40822211/article/details/127682306)
[csdn: QNX screen使用介绍 第五章](https://blog.csdn.net/Suixing_yuan/article/details/115440148)
[blog: screen 共享和克隆](https://www.zxcms.com/content/iwmoc14331l6bo.html)
