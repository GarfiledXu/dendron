---
id: noy931vdhmg56m651362fm1
title: Opengl Se20
desc: ''
updated: 1695053802777
created: 1691630285207
---
### little practice
1. encapsulation drawing interface
   1. input screen coordinate to draw reactangle
   2. input screen coordinate to draw triagnle
   3. input screen coordinate to draw line
   4. input screen coordinate o draw point
   5. input viewport to draw frame of each yuv 
   6. input viewport to draw frame of jpg, png
2. transform

4. cost time of variant way and the memory use of each step 

### opengl es2.0
> 存在于GPU驱动中，用于图形绘制的API，准确说是图形加工计算，从模型数据到显示数据
#### 设计理念和目标
- 管道设计：工厂式 可伸缩性 和 并行性
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


