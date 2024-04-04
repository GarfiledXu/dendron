---
id: tjyoavoy0b8eos6caaepv21
title: Shader_glsl
desc: ''
updated: 1710902384504
created: 1691650830878
---

 #### reference
 [github-cn manual](https://github.com/wshxbqq/GLSL-Card)
 [GLSL版本的区别和对比](https://www.cnblogs.com/OctoptusLian/p/9909170.html) 
 [shader系列之什么是着色器？](https://developer.aliyun.com/article/950943)
 [zhihu: （搬砖）什么是着色器Shader](https://zhuanlan.zhihu.com/p/94461981)
 [zhonguangchun: 超越图形界限 AMD并行计算技术全面解析](https://vga.zol.com.cn/192/1927855_all.html#p1929467)
[gost: 着色器简介](https://docs.godotengine.org/zh-cn/4.x/tutorials/shaders/introduction_to_shaders.html)
[zhihu:《Real-Time Rendering》第四版学习笔记——Chapter 3 The Graphics Processing Unit](https://zhuanlan.zhihu.com/p/462489434)
[shader调试验证: shadertoy](https://www.shadertoy.com/view/MslfWj)


#### target
texture unit target and vertex buff object target
每一个上下文中，shader 访问的 texture unit 的类别和对象数量，vertex buff object 的类别以及target 都是固定的，
而在编程

 #### question
 1. GPU 整个渲染管线的运行过程，并行结构是怎么样的？同种着色器如何并行，不同种着色器如何配合?着色器的数量分布?



 ### basic grammar
 #### ref
 [category: opengl shading language](https://www.khronos.org/opengl/wiki/Category:OpenGL_Shading_Language)
 [offical: data type](https://www.khronos.org/opengl/wiki/Data_Type_(GLSL))
 [Built-in_Variable_(GLSL)](https://www.khronos.org/opengl/wiki/Built-in_Variable_(GLSL))
 [Interface Block (GLSL)](https://www.khronos.org/opengl/wiki/Interface_Block_(GLSL))
 [Data Type (GLSL)](https://www.khronos.org/opengl/wiki/Data_Type_(GLSL))
 #### variable data type
 1. **scalars 标量**
    > 只有单一的数值，没有内部结构以及分量，是一个独立的数据项

    最基本类型: `void` `bool` `int` `float`, 相比c语言，没有 `long`, 没有 `double`类型 

 2. **vectors 向量**
    > Each of the scalar types, 意味着向量的元素单位是标量类型


![https://img2020.cnblogs.com/blog/1627257/202104/1627257-20210413194021997-232449982.jpg](https://img2020.cnblogs.com/blog/1627257/202104/1627257-20210413194021997-232449982.jpg)
    