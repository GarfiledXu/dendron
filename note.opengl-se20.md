---
id: noy931vdhmg56m651362fm1
title: Opengl Se20
desc: ''
updated: 1693326101320
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
- [blog-sample](https://github.com/SaschaWillems/openglcpp/tree/master)
- [program guide sample code](https://github.com/danginsburg/opengles-book-samples/tree/master)

#### baisc work flow
- egl init
- shader load
  1. create shader object, return **object id**
  2. load shader by source code or other way with use **object id**
  3. compile shader with input **object id**
  4. check compile status, get the error information to feedback
- program load
  1. create program object, return **object id**
  2. attach two kind of shader with **object id**
  3. **bind location to attribute declared in vertex shader**
    > there has variation ways to bind attribute location 
    > 1. bind location by `glBindAttributeLocatioin` prior to link program, to tell program linker location of attribute variable
    > 2. using `glGetAttribLocation()` by the qeury way to get one address(location) of a attribute
    > 3. to specify the location to attribute decalred in vertex shader
  4. link program object with **object id**
  5. check link status, get the error information to feedback(delete program object)
- set viewport
- clean buff(typically, the on-screen)(multipule type of buff, such as color, depth, stencil)
- load position data and connect to attribute variable declared in vertex shader **by attribute location**

#### the variation of GPU graphic memory for each step
- guess: glCompileShader operation, the first step that compliles the shader bin file by gpu driver program in cpu end, and then send binary file of shader to stored by GPU end.
- glUseProgram: set enable state of gl program used in after step, no data transfer involved


#### the data interact of CPU to GPU in opengles

#### attribute with vertex data and location
- **is it neccessary to bind attribute variable that from shader with the vertext data that from cpu program by this way through binding location id? instead of using attribute name directly**
  > strip the effect of attribute name
  > consider the modifies of shader program source code
  > the bind id is generated before use program
- attribute operations like transfer vertex data **throgh the location of attribute**, as such, that informs to get location of attribute is the important thing.
- as a specific variable type of shader, attribute connect to operations by location
- size and stride, component and attribute and group attribute
  > size explains the length to tell method of gl api to extract one unit target attribute data begin with the data pointer which point the start location of one unit data
  > stride explains the length to tell method of gl api to extract next target attribute data need to offset  begin with the current data pointer

#### global shader variable name
- gl_Position
- 


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