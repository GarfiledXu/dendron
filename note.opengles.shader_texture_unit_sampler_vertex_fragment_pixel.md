---
id: w8kofsxr8csfaimt8fc88f4
title: Shader_texture_unit_sampler_vertex_fragment_pixel
desc: ''
updated: 1711294353830
created: 1710771314791
---
### record
2024.3.19-3.23 uyvy render success, texture knowlege resolve most, but vbo vao not clear
2024.3.24 整理好基础demo的代码再去进行扩展修改优化，效率提升明显

### assess
前期知识点的理解，战术懒惰，没有进行问题转化，效率低下
中后期代码实现遇到的问题：1. 全局函数间句柄没有真正传递，导致并没有报错且任何显现 2.缺少对关键数据的debug打印，全看效果调试 3. demo 结构，四不像，不够精简，又不够稳定，暗藏问题多，且不利于严重，从出现问题到解决问题通过不断对demo进行删减，导致后期过于混乱, 问题排查处于无序状态，浪费大量时间，next更正: 在设计理想demo结构后，还是选择 先从最简化demo结构开始进行效果验证，再进行结构递修补，才能节省时间
严重问题: 时间管理彻底失控，无节制，无规划，最终反噬身体，心神，工作
唯一庆幸，理解了大部分内容，实现demo跑通，但待整理

### plan 
- [ ] 进行视频播放

#### 2024/03/24
- [x] 加载yuv数据，使用通用的jpg转yuv模块
- [x] 重新整理 uyvy render demo 代码
- [x] 下一步 i420 渲染
- [x] 下一步 rgb 渲染
do noting

#### 2024/03/25
- [ ] 阅读所有vbo相关的API文档


### ref
[强劲 音视频栏目: 字节流动](https://www.nxrte.com/user/23)
[字节流动: 一口气搞懂所有 YUV 格式图像的 OpenGL 渲染](https://www.nxrte.com/jishu/42761.html)
[github: 字节流动](https://github.com/githubhaohao/NDK_OpenGLES_3_0)
[奇葩AnJoiner: Android编译FFmpeg 6.0 - 支持MediaCodec编解码](https://juejin.cn/post/7297838901090648118)
[奇葩AnJoiner: Android音视频开发:踩一踩“门槛”](https://juejin.cn/post/6844904045572784142)
[github: ffmpeg command](https://github.com/AnJoiner/FFmpegCommand)
[csdn: 字节流动](https://blog.csdn.net/Kennethdroid)
[字节流动: 纹理映射](https://www.jianshu.com/p/545e96798c68)
[字节流动: YUV渲染](https://www.jianshu.com/p/224892abfc33)
[字节流动: FBO离屏渲染](https://www.jianshu.com/p/2596f1e758c7)
[字节流动: YUV 和 RGB 图像转换出现偏色问题怎么解决？](https://www.nxrte.com/jishu/37806.html)

[wiki: TMU](https://en.wikipedia.org/wiki/Texture_mapping_unit)
[khronos: glBindTexture description](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glBindTexture.xhtml)
[khronos community: Terminology: texture target vs texture unit vs texture image unit, etc… help!](https://community.khronos.org/t/terminology-texture-target-vs-texture-unit-vs-texture-image-unit-etc-help/105441/2)
[learnwebgl: Texture Mapping Using Images](https://learnwebgl.brown37.net/10_surface_properties/texture_mapping_images.html#hardware-overview)

**about texture memory**
[nvidia: opengl 编程技巧:释放绑定](https://docs.nvidia.com/drive/drive_os_5.1.6.1L/nvvib_docs/index.html#page/DRIVE_OS_Linux_SDK_Development_Guide/Graphics/graphics_opengl.html) : 说明了哪些接口库会进行内存分配，哪些会进行释放
[khronos: glDeleteTextures](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glDeleteTextures.xhtml) :只讲明了该接口会删除纹理，以及运行重新复用纹理id，并没有规定 纹理内部的内存 要强制free, 存在可能内存仅仅被标记为非绑定的, 可以防止内存泄漏，但不保证内存会被free处理
[khronos: glTexImage2D](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glTexImage2D.xhtml) :主要还是描述了格式设置，但并没有说明内存管理
[khronos: glActiveTexture](https://registry.khronos.org/OpenGL-Refpages/es2.0/xhtml/glActiveTexture.xml)
[khronos: glBindTexture](https://registry.khronos.org/OpenGL-Refpages/es2.0/xhtml/glBindTexture.xml) : 说明了，当纹理已被初始化时，bind操作只做绑定切换，若未被初始化，则是对纹理对象初始化并且绑定
[khronos: Texture](https://www.khronos.org/opengl/wiki/Texture): have diagram of the content of a texture object
[gamedev: 论坛讨论 纹理释放](https://gamedev.stackexchange.com/questions/136883/opengl-texture-releasing)
[stackflow: 讨论 纹理内存的管理](https://stackoverflow.com/questions/11217121/how-to-manage-memory-with-texture-in-opengl)
[khronos: 纹理存储](https://www.khronos.org/opengl/wiki/Texture_Storage): 具体指明了几种存储方式，以及存储类别，可变存储与不可变存储等

**about bind relationship**
[khronos: 关于 glActiveTexture 以及 用c描述 相关接口](https://community.khronos.org/t/when-to-use-glactivetexture/64913) : 一个全局的unit数组 + 一个全局的unit idx，bindTexture内部先通过unit idx去获取对应unit引用，再将当前texture obj 引用赋值给unit
[stackflow: some clarification about using multiple texture](https://stackoverflow.com/questions/57621863/opengl-some-clarification-on-using-multiple-textures-or-texture-units)
[stackflow: how load multiple texture using opengl](https://stackoverflow.com/questions/51923159/how-to-load-multiple-texture-using-opengl-and-c)
[opentk: Multiple Textures and Texture Units](https://opentk.net/learn/chapter1/6-multiple-textures.html)
[stackflow: 多重纹理和多个纹理讨论](https://stackoverflow.com/questions/6969260/when-should-i-use-multiple-texture-units)
[learnopengl: texture](https://learnopengl.com/Getting-started/Textures)
[khronos: sampler object](https://www.khronos.org/opengl/wiki/Sampler_Object)
[khronos: sampler](https://www.khronos.org/opengl/wiki/Sampler_(GLSL))
[khronos: each concept](https://community.khronos.org/t/terminology-texture-target-vs-texture-unit-vs-texture-image-unit-etc-help/105441/4)
[the book of shader: texture2D](https://thebookofshaders.com/glossary/?search=texture2D)
[pdf: Tutorial 3: Texture Mapping](https://research.ncl.ac.uk/game/mastersdegree/graphicsforgames/texturemapping/Tutorial%203%20-%20Texture%20Mapping.pdf)
[字节流动: YUV渲染](https://www.jianshu.com/p/224892abfc33)**: 在这篇yuv渲染教程中，就是先默认一个unit 对多个纹理连续bind，初始化纹理以及加载数据，到末尾连续使用active+bind的方式，对unit和纹理进行绑定 !!!**
[learn-webgl: Texture Mapping Using Images](https://learnwebgl.brown37.net/10_surface_properties/texture_mapping_images.html#hardware-overview)
[cnlog: opengles2.x](https://www.cnblogs.com/tzsh1007/p/14654871.html#shader-program-creation) **具体讨论了纹理机制，有图有码 !!!**

**the order of about vbo 以及 shader program 和 shader variable的set**
- glGetAttribLocation
- glGetUniformLocation
- glBindAttribLocation
- glUniform1i
- glGenBuffer
- glBindBuffer
- glBufferData
- glSubBufferData
- glVertexAttribPointer
- glEnableVertexAttribArray
[opengles doc](https://docs.gl/es2/glVertexAttribPointer) : 首先，每一个上下文都有一个 current program的状态，这是由useProgram决定的，linkProgram 本质上是将两部分attcah到一个programid的shader程序融合，所以linProgram会处理program的二进制内容，给变量地址，让变量初始化，并且给shader变量部分属性依次进行offset，静态内容确定了就可以满足通过programid 查询变量位置，所以查询操作 linkprogram是前提，但是link了，不代表当前上下文使用的program，所以仅仅link，想直接使用变量位置来发送数据是无效的，因为此时上下文中不存在program，即目的地不存在，需要在useprogram后，上下文中存在，目的地存在，再才能够进行双方的数据交互, 为什么vertexattributepointer可以在useprogram之前
vbo中的buffer target, 与 texture target类似，进行bind，一方面可以在句柄第一次bind时，对其进行初始化(不上分配内存), 另一方面，相当于切换当前的上下文为具体类型的target的，后续在上下文中所有buff相关的设置和操作以及texture相关的操作都是针对当前target的，texture无非多了一个texture unit属性的组合, 意味切换和绑定多了一个属性，所以，绑定 更准确的形容是 将object 绑定到 target上，target视为object的operator/机床，即对object的加工操作是由target执行的，所以先绑定target和object，再设置具体的operation， 来完成对一个object所有操作的预设
vbo first bind，will initialize with the zero-size memory 
exist vbo bind, will 
  
为什么没法反复使用vbo？只有#else的代码段有效
```c++
#if 0
    // uyvy_render_init();
    GLint att_postion = glGetAttribLocation(cur_shader_info.id_shader_pragram, "a_position");
    GLint att_texcoord = glGetAttribLocation(cur_shader_info.id_shader_pragram, "a_tex_coord");
    
    glBindBuffer(GL_ARRAY_BUFFER, buff_id[0]);
    glVertexAttribPointer(att_postion, 4, GL_FLOAT, false, 4 * sizeof(GLfloat), 0);
    glEnableVertexAttribArray(att_postion);

    glBindBuffer(GL_ARRAY_BUFFER, buff_id[1]);
    glVertexAttribPointer(att_texcoord, 2, GL_FLOAT, false, 2 * sizeof(GLfloat), 0);
    glEnableVertexAttribArray(att_texcoord);
#else
    GLint att_postion = glGetAttribLocation(cur_shader_info.id_shader_pragram, "a_position");
    GLint att_texcoord = glGetAttribLocation(cur_shader_info.id_shader_pragram, "a_tex_coord");
    
    GLuint buff_id[2] = { 0 };
    glGenBuffers(2, buff_id);

    glBindBuffer(GL_ARRAY_BUFFER, buff_id[0]);
    glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
    // glVertexAttribPointer(att_postion, 4, GL_FLOAT, GL_FALSE, 4*sizeof(GLfloat), vertices);
    glVertexAttribPointer(att_postion, 4, GL_FLOAT, false, 0, 0);
    glEnableVertexAttribArray(att_postion);

    glBindBuffer(GL_ARRAY_BUFFER, buff_id[1]);
    glBufferData(GL_ARRAY_BUFFER, sizeof(texcoords), texcoords, GL_STATIC_DRAW);
    // glVertexAttribPointer(att_texcoord, 2, GL_FLOAT, GL_FALSE, 2*sizeof(GLfloat), texcoords);
    glVertexAttribPointer(att_texcoord, 2, GL_FLOAT, false, 0, 0);
    glEnableVertexAttribArray(att_texcoord);
    #endif
```

### repeat question

1. texture unit 与 sampler、texture object、 texture target、fragment shader  以及texture elements(texels 纹素)的关系
2. 问题1 在硬件层面的组织结构
3. 和纹理单元相关的操作
4. 顶点着色器 与 片段着色器的输入输出
5. 顶点着色器 到 片段着色器之间还有哪些处理流程
7. 渲染一张uyvy的图片过程中，从顶点着色器到片段着色器过程中，顶点着色器和片段着色器分别的总执行次数
8. 纹理映射是哪个硬件模块做的，纹理采样是哪个硬件模块做的？
9. 纹理坐标的计算 与 纹理采样 以及其他的纹理操作的关系？
10. 管道的每一个阶段之间是否是串行的，只是每一个阶段的内部处理是并行的？yes
11. 光栅化之前会经历剪切，剔除，以及最开始的顶点变换，在这个过程中，纹理坐标是否会跟随变换？
12. fragment shader 中发生的纹理采样操作，实际的执行操作是 shader完成的，还是texture unit完成的?
13. fragment shader 与 texture unit一样在gpu中有对应的硬件单元，那么fragment shader的并行处理是否是受限于texture unit的数量?
14. 纹理映射的单位是什么?采样的单位又是什么? 采样的输入输出是什么?输入的是像素映射的纹理坐标，输出的是纹理坐标像素对应的纹理数据，  是否都是像素单位，即不管如何映射，用什么采样模式，最终采样获得的是一个像素单位对应的纹理数据，而纹理的format则表示了一个像素单位的纹理数据结构，是否就意味着在fragment中采样出来纹理数据的格式必然等于 纹理format对应的格式(即glsl中的sampler(准确说是 texture2D函数)采样返回的数据格式是什么？由什么决定，是否就是开始指定的纹理格式，eg: 纹理格式RGBA，则texture2D返回vec4(R,G,B,A), 纹理格式BGR，则texture2D返回vec4(B,G,R,1))
15. 一个 fragment shader，可以定义多个uniform的sampler，绑定多个texture unit进行采样，对多方的采样数据进行混合输出，但使用的是同一个Texcoord, 也就意味着这里texcoord并不是片段在目标纹理上的纹理坐标?否则一个像素在不同纹理上应该有不同的纹理坐标？
    > 还是没有理解纹理映射整个过程中纹理坐标的计算! 是否纹理映射过程可以拆分为几个阶段: 纹理坐标计算，这是由输入的顶点数据和决定纹理映射方式附带的纹理坐标信息决定，即一片图元上最后光栅化后的每一个片段对应的纹理坐标都是唯一不变的，然后是根据一个像素对应一个纹理坐标的基础，在shader中选择准备好的texture unit 按一定模式进行采样，并对多方采样结果进行二次客制化处理，最终输出一个像素的色彩
    > 既然纹理坐标是与像素/片段单位对应的，那么是否说明至少是在光栅化后才完成每一个像素的纹理坐标的计算，若是如此，就意味着一个重要信息，一个像素对应的纹理坐标 与 该像素所在多边形图元 将要映射的 纹理数据是无关的，纹理数据只影响了 fragment shader的采样操作。初理解概念，易起混淆，容易将纹理坐标与纹理数据进行关联，认为纹理坐标是 最终在纹理数据上采样时的位置，这也解释了为什么在理解着色器采样时，不能理解已经存在了纹理坐标为什么还要进行采样的疑惑
    > 疑惑反思: 疑惑出现时，如何警觉，进行抽丝剥茧， 0、警觉概念存在误解，必然是有关键概念被误解，原地思考并不能带来突破，只能让困惑感保持 1、主动回头，梳理所有相关概念，一个一个进行确认，在确认具体概念时，则尝试从不同渠道进行信息搜集理解 2、在实操地找差别(这里为shader 代码)，对比正值和理解差异，发现误解关键 3、绘制流程图 4、理解的应用场景过于简单，导致不能理解设计初衷，可以放大真实的应用场景进行理解 5、很多时候疑惑，只是一种现象，要将疑惑转化为问题才能真正针对找到答案
16. 为所有后处理过的图元生成 用于在屏幕上渲染的片段/像素, 并且为片段计算屏幕坐标(screen coordinate)，以及附加属性，如为每一个片段生成一个线性插值的texture coordinate, (attributes: texture coordinate, etc.) 这些操作是在渲染管线的哪些阶段完成的?

17. 纹理数据 与 unit target 以及texture id/object 绑定映射关系的梳理： 在(一口气搞懂所有 YUV 格式图像的 OpenGL 渲染)教程中的伪代码片段里，加载两个纹理数据，是直接在glTexImage2D 之前执行 bindTexture 到对应的texid中的，似乎在第一次加载纹理之后，到第二次加载纹理之前，没有进行activeTexture 第二个对应的纹理单元，那么是否有问题？首先确定的是glTexImage2D 传参是只指定了具体的texture target 类型，并没有指定具体的texture unit，那么问题1: 加载纹理这一操作，是否需要texture unit信息，即将具体的纹理数据与具体的unit+target进行绑定? 如果不需要的话，在采样时如何确定使用哪块纹理呢?  
    > 逆向推理: 在fragment shader中的纹理采样交互，可知 由cpu客户端传递给服务端shader的是 具体的texture unit ID, shader端预先定义好接收的unit的target类型，cpu端传入unit id，来决定shader端使用的unit编号，
    纹理的采样行为依赖于 具体texture unit + texture target 类型 + 目标纹理数据 + texture coordinate，其中coordinate 由光栅化阶段自动生成，并在fragment shader执行时传入，

18. 纹理单元的切换是否是必要条件？还是只是为了通过多数量的纹理单元绑定提高效率而已(unit仅仅是纹理数据的消费单元，unit的数量仅仅影响消费速度)？一个纹理单元必然可以承载所有纹理的映射，而不是一个纹理单元对应一个纹理数据，纹理单元虽然存在多数量，但数量非常有限，所以，1.一帧内图像(即在swapbuff之前) 只用一个纹理单元如何进行所有纹理处理 2.正常场景下一帧内图像(swapbuff之前)，处理大批量纹理，如何充分利用纹理单元？

19. API 层面 关系: glGenTextures glDeleteTextures glBindTextures 影响了 texture object 的生成，删除，和与具体unit的target的绑定，glTexImage2D 的执行依赖于glBindTexture绑定的状态结果， 通过的方式DeleteTexture的方式来进行状态解绑和状态重绑定，客户端在执行了什么函数以后，会触发fragment的绘制？这里出现疑问是没理解整个gl的执行流程，客户端设置状态和load数据以后，通过draw 函数进行触发，服务端的管线才真正的按管线流程消费load的数据 到目标buff中，即管线内部的执行流程与客户端是没有同步动作的，客户端仅仅是在管线执行前设置状态和准备数据，并且发出绘制命令，最后触发管线一次性执行，所以一帧内(swap buff之前的图像)
的数据绘制，是由 n次的draw调用*每次绘制的内容 组成的 | 类比：渲染管线等同于封闭的流水线工厂，我们只负责准备原材料和设置工厂在后续执行时的一些状态参数，然后下令开工，工厂则会自动执行所有预设操作直到输出产品到预定区域，这个过程是封闭且不与外部同步的，我们所有的操作都是在工厂运行前
activeTexture 这里的active可以理解为 当前上下文指向的unit编号，当前的纹理unit是全局的，纹理的绑定操作依赖于全局的unit编号和target类型的组合，所以当需要绑定不同的unit时，调用activetexture进行切换是必须的

为什么bindtexture必须在teximage2d之前，gentexture并没有创建纹理，仅仅是创建了句柄，而bindtexture才是真正初始化了纹理，即在绑定纹理到具体目标的时候初始化，纹理，所以teximage2d的前提是纹理必须被绑定初始化，才能在这一步进行，并且 纹理的初始化 与 具体的unit编号是无关的，这也意味着，可以使用同一个unit，直接连续bind，去初始化多个纹理，那么问题来了？如果先进行上诉的步骤，后续我需要将两个初始化的纹理绑定到不同的unit，该如何操作？？？是否bind的操作=纹理未创建则初始化第一次绑定的纹理+并切换具体纹理target与object的绑定，若纹理已经是存在的，那么bind只是改变了当前target的绑定， 这种绑定准确来说是:`具体target对object的引用 引用的切换并不会导致被引用方本身的任何变化`真正的纹理创建是bind函数，会决定纹理的类型，方向是target到object，object是被选择方，target是主动方，所以一个具体的target只能绑定一个object，不同target可以绑定同一个object 即二者不是归属关系，而是绑定关系，是可以切换的，并且当前的切换会导致target先前的绑定失效解绑， 纹理数据和unit的绑定，是由teximage2d这一操作执行时的 全局unit状态决定 

`texture image的载体到底是受 texture object 管理，还是说经过 glTexImage2d是存储在具体texture unit上的，如果是前者，是否意味着上传texture image时对应的texture unit和最后的texture unit可以不是一个，但这显然矛盾，因为active texture 是全局特性只存在一个，如此说来都只最后一个unit指向image，那么是否可以反推，首先确认的是: 基于前面的状态绑定，glTexImage2D 会将unit与image绑定，但这种绑定是否是 因为image是与object形成了绑定？本质上image只由object管理？及时后续切换了绑定，原本的image与object还是存在的？ 问题转化: how the texture stored?【纹理的数据存储与管理？】-> 【纹理内存管理的范式？】 glTexImage2D 为纹理对象分配了空间并上传数据，那是否每次为同一个object调用该函数都会不断申请空间导致gpu端内存泄漏，并且该空间如何释放？是的，glTexImage2D每次都会为当前unit绑定的object执行内存分配，如果是复用内存，则在确定数据长度不变的清空下使用glTexSubImage2D进行数据更新，内存复用，当长度变化的情况下呢？`

【如何释放/free/releasing glTexImage2D分配的内存？是否需要释放？】->bind 一个texture object to 0，会实现object与unit的解绑， opengl 的api说明中只提及了 使用delete texture 可以实现texture id与texture object的解绑，没有明确说明texture 内部存储的处理，即不同gl的驱动实现有可能不同，可能立刻free，有可能是在某个节点进行回收，有可能仅仅是进行解绑定而已，如果强制引用还是可能获取到旧数据

能否先通过同一个unit，反复bind切换object，来实现所有的纹理预加载，然后再通过active+bind的方式来绑定每一个unit对应的object用于使用


opengl 客户端纹理的处理，核心的操作就是 在上下文中 进行texture unit target 与 texture object 的绑定与切换，最后数据的加载仅仅是所有绑定完成的最后一步，并且客户端仅仅是负责纹理的准备，也就意味着客户端准备的是一个纹理集合，而渲染时真正使用的纹理，是由shader中的采样器变量决定，
 一个texture object 不仅仅是只有一个句柄属性，其还有数据指向，以及纹理属性

 bind可以切换 unit target 与 已存在 texture object 的绑定，而这也将比重新进行teximage2d进行加载更高效，所以切换的意义在于 快速复用已加载的数据，而不进行重加载

20.每次绘制前，加载的纹理数据会在GPU端占用缓冲，所以，GPU的缓存大小限制了单次能够绘制的内容多少，通过增加绘制的次数来完成所有内容的绘制，也就意味着数据的覆盖和重加载，以及客户端与服务端通信的消耗，最终影响总内容绘制的耗时占用，导致帧率上升

21. multi texture unit usage and multi texture process(load)?
    首先，毫无疑问，在fragment shader中定义的 uniform smpaler2D 变量，让客户端通过该变量名指定对应的texture unit 来决定fragment shader中的采样行为，也就意味着 fragment只能通过引用texture unit来引用对应的纹理对象，如此看来每次绘制时，申请的纹理对象数量是由准备使用的纹理单元数量决定的，是否存在申请纹理对象数量大于激活的纹理单元数量的场景？是如何处理的？(问题转换: 正确的加载大量纹理的方式是什么?) 是否load后的纹理数据与texture object绑定后，经过重新bindtexture后并不会消失，只是与texture unit的绑定切换了，如果是这样，那么 多个的纹理对象似乎存在意义，即用于切换与unit的绑定，不用重复加载
    (问题转换: 能否在一个texture unit上切换不同的已存在的texture object? texture object是否会占用gpu缓存?只有删除texture object时才会清空) (问题归纳: 多纹理数据的切换和加载？texture object的意义是否在于他是一个纹理数据的缓存单元?如果是这样，为什么load数据还要依赖于前面存在bindtexture步骤，不是指定load的纹理类型和被使用的target类型不就好了吗，是否bindtexture可以在load之后？)
    其次可以看出，texture unit 多个纹理单元的意义在于一次draw调用时，fragment shader可以同时对多个纹理采样，进行混合处理


### correspond answer
1. 
2. 通常，组合 unit 的词汇，类似 central processing unit(CPU), graphic  proccessing unit(GPU)都是表示处理器实体概念，`texture unit (TMU)` 同样也是表示 纹理处理相关的处理器实体, is designed for texture map processing. 而渲染API通常会要求硬件设备具备至少存在一定数量的 texture unit
   1. 即 texture unit 是一个硬件单元，主要执行 texture map 处理 
   2. `texture unit` 和 `texture object` 都属于 图形API发展过程的历史产物，他们都曾经是图形系统中的 `originally dedicated hardware`, 独立硬件而非芯片中的子模块单元, 甚至在opengl早期的版本中，只存在texture object，在后续现代显卡的发展中，成为图形管道的重要部分以及芯片架构中的关键模块, 渲染API中涉及到的概念往往在GPU硬件结构中有着紧密关联的模块, 二者是同步演化的

### 疑惑关键
1. 没有理解片段着色器的处理逻辑
    > 片段着色器的处理 在光栅化之后，此时 着色器的输入是 一个光栅化后的像素，像素本身携带着纹理坐标，
2. 没有理解纹理映射，纹理坐标，执行阶段，以及计算流程
   > 使用已存在的纹理数据(能够表示像素色彩的数据)，选定纹理数据的一部分，将其投射到一个已存在的范围确定的二维表面(由顶点组成的多边形图元)，纹理本身是二维平面的，因此可以建立坐标系，即让纹理中的每一个像素单位都有对应的纹理坐标，通过指定 顶点数据的同时指定`顶点对应的纹理坐标`，来确定顶点组成的图元与纹理进行映射的范围以及方向，并且在之后阶段，会为该图元在光栅化后生成的每一个像素进行插值计算，获得`像素对应的纹理坐标`，[既然已经有像素对应的纹理坐标了，最后不是直接通过在纹理中查值就可以获取了吗?为什么还有采样？采样模式？]

        opengl 的任务是 最高效的 接收三维图像的数据，组合三维世界，计算最终显示的二维画面，将对应的缓冲区更新同步到窗口管理模块或者是一块独立缓冲区中
        管道化后所有的处理都是分阶段进行，即当前阶段的输出作为下一阶段的输入，即并行处理只在渲染管线的具体阶段，阶段间是串行









#### record
```c++
question:
如果按RGBA的格式加载UYVY数据，然后数据长度是width/2，height不变，然后使用以下shader
char vs[] =
        "attribute vec4 vertexIn;       \n"
        "attribute vec2 textureIn;      \n"
        "varying vec2 texOut;           \n"
        "void main(void) {              \n"
        "    gl_Position = vertexIn;    \n"
        "    texOut = textureIn;        \n"
        "}                              \n";

    char fs[] =
        "uniform sampler2D texUYVY422;                          \n"
        "varying vec2 texOut;                                   \n"
        "const lowp vec3 offset = vec3(-0.0625, -0.5, -0.5);    \n"
        "const mediump mat3 converter = mat3(                   \n"
        "    1.164, 1.164, 1.164,                               \n"
        "    0.000, -0.391, 2.018,                              \n"
        "   1.596, -0.813, 0.000);                              \n"
        "void main(void) {                                      \n"
        "    vec3 yuvColor;                                     \n"
        "    vec2 TexCoord = texOut;                            \n"
        "    yuvColor.x = texture2D(texUYVY422, TexCoord).y;    \n"
        "    yuvColor.y = texture2D(texUYVY422, TexCoord).x;    \n"
        "    yuvColor.z = texture2D(texUYVY422, TexCoord).z;    \n"
        "    vec3 rgbColor;                                     \n"
        "    yuvColor += offset;                                \n"
        "   rgbColor = converter * yuvColor;                    \n"
        "  gl_FragColor = vec4(rgbColor, 1.0f);                 \n"
        "}                                                      \n";
那么最终的效果是否是 丢弃掉uyvy中每一堆uyvy中的最后一个y分享，相当于把一个rgba中的图像每一行间隔一个像素的像素去掉，映射到目标图元中
ChatGPT answer:
根据你提供的 Shader 代码，如果将 UYVY 数据按 RGBA 格式加载，然后使用给出的 Vertex Shader 和 Fragment Shader 进行渲染，那么最终的效果是将 UYVY 数据映射为 RGBA 格式的图像，但是只使用了 UYVY 中的 Y 分量来生成 RGB 颜色。

根据你的描述，UYVY 格式的数据中，每四个字节分别代表两个像素的数据，所以加载到纹理中后，每一行的像素数据就相当于被压缩了一半。在 Fragment Shader 中，每次采样纹理时，只使用了其中的一个像素的数据，也就是每次只使用了 UYVY 中的一个 Y 分量，因此相当于在水平方向上将像素间隔了一倍，即丢弃了一个 Y 分量。这样的处理会导致最终的图像在水平方向上分辨率降低了一半。

个人思考:
    此时的纹理只具有原图一半的分辨率，只是每一个像素被横向拉伸了，那么是否还有补救措施: 如果再来一张纹理，舍弃第一个y，使用第二个y呢，问题是这样是否会和前一张纹理被拉伸的部分重叠
    能否根据纹理坐标判断是否使用第一个y还是第二个y？
```