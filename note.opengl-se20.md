---
id: noy931vdhmg56m651362fm1
title: Opengl Se20
desc: ''
updated: 1691838483591
created: 1691630285207
---

### opengl es2.0
> 与其说是图形API，不如说是GPU绘图控制API啊
#### 设计理念和目标
- 跨平台，去除硬件相关性
- 提供图形渲染功能
- 并行高效性
- API设计要足够灵活，供开发人员使用
------------------

### reference
- [blog-nice-opengl es2.0](http://geekfaner.com/shineengine/blog2_OpenGLESv2_1.html)
- [opengl es2.0 full specification-pdf](https://registry.khronos.org/OpenGL/specs/es/2.0/es_full_spec_2.0.pdf)
- [opengl es2.0 shaders language specification-pdf](https://registry.khronos.org/OpenGL/specs/es/2.0/GLSL_ES_Specification_1.00.pdf)
- [opengl es2.0 refpages-online](https://registry.khronos.org/OpenGL-Refpages/es2.0/)
- [blog-opengl for android](http://lanixzcj.github.io/opengl-es2.0-for-android/)
- [blog-zhihu-yuanshuo opengl es2.0](https://zhuanlan.zhihu.com/p/560012304)
- [blog-juejin opengl es2.0 tutorial](https://juejin.cn/post/7206882855200145465)
- [opengl registry](https://registry.khronos.org/OpenGL/index_es.php)
- [offical developers-opengl es2.0 tutorial](https://tool.oschina.net/uploads/apidocs/android/resources/tutorials/opengl/opengl-es20.html)
- [opengl es2.0 program guide](https://www.opengles-book.com/es2/errata.html)


### EGL `embedded graphic interface`
-------
#### reference
- [offical-overivew](https://www.khronos.org/egl)




#### gen API
- 仅仅生成buff obj name
- 为什么需要将生成对象索引作为独立步骤，而不是索引与初始化一并处理?
  > 本质上这是对象的`声明`与`初始化`分离，提供编程灵活性，允许对象的生成和对象的资源管理分阶段批处理
- 由 `API` 易知 gl 客户端没有 buff 对象名称定义的权限，仅有从 gl 服务端获取 buff 对象索引的权限
- gen生成的索引 name ,在哪些地方使用？

**流程**:
由gl客户端通过 `gen` 接口发出buff对象生成请求，gl服务端根据管理buff返回`可用空对象索引(引用)`

#### bind API
- buff object 做了什么？
- buff object 与 buff object type 以及与 gl context 以及 gl thead 的关系?
- 相关操作的内存分配详情？

#### vertext attribute
- vertext attribute name
- vertext shader
- pre-define vextext attribute name correspond to generic vertext attribute
- programable pipline -- generic vertext attribute
- 