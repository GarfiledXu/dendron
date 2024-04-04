---
id: rqoizpo5vnaf1wvzobz23po
title: '26'
desc: ''
updated: 1711759163134
created: 1711441329170
---

#### must
- [x] ss528 工程编译通过
- [x] testbed 移植编译通过
- [x] 阅读所有vbo相关API文档
  - [x] glBindBuffer
    0. 做了什么: create or use (first bind to create and initial, after exist binding )
    1. initialization status: GL_STATIC_DRAW and zero 
    2. bind buffer 的 类别: 1. vertext array data 2. element array buffer (element -> individual vertext's indices)
    3. 1.存储的是每一个vertex的每一个分量数据 2.存储的是每一个指定vertex在vertex array中的位置索引
    4. bind => buff object 和 target
    5. 为什么该参数的定义是 target 而非 type，target 概念是具象化的，是一个具体的被操作对象，是 shader 最终引用和操作的对象
    6. 既然是具象化的，那么这两种buff target与buff object的绑定, 就类似于 定义了两个全局的target指针，该指针在客户端与buff object绑定，在shader端被引用
        ```c++
        //global
        ArrayBuffer* g_array_buff;
        ElementArrayBuffer* g_element_array_buff;

        //client custom call
        BufferObject obj1;
        BufferObject obj2;
        //bind to obj1
        g_array_buff = (convert)obj1;
        //bind to obj2
        g_array_buff = (convert)obj2;
        //bind to obj2
        g_element_array_buff = (convert)obj2;
        //current bind status: g_element_array_buff + obj2, g_element_array_buff + obj2

        //shader
        //read data
        process_array_buff(){auto& cur_buff_obj = get_obj(g_array_buff);};
        process_element_array_buff(){auto& cur_buff_obj = get_obj(g_element_array_buff);};

        ```
    可以定义一个buff object，将所有target需要的数据按序放入同一个obj，然后让两种target同时绑定，只要设置好偏移长度，就可以获取各自需要的数据
    7. 使用 0 去作为obj 和 具体target绑定，可以接触该target当前的指向
    一个target只能指向一个 buff object，也就意味着全局，对于某个target只有最新的绑定操作状态是有效的
  - [x] glBufferData
    
  - [x] glBufferSubData
  - [x] glDeleteBuffers
  - [x] glEnableVertexAttribArray
  - [x] glDisableVertexAttribArray
  - [x] glDrawArrays
    从array中提取 individual primitive 进行绘制
    if not use vbo, vertext data be specified and stored on client，only call Draw func will copy this data to graphic
    `基于上面的讨论，vbo的关键作用在于，他是提前在 gpu 端进行数据存储来提高交互效率`
  - [x] glDrawElements
    draw element 的基础是已经加载和传递了 vertex array data
    存储的是 vertex array data 中每一组vertex的索引
    draw array 和 draw element data 都可以不使用vbo，vbo的意义仅仅是在draw之前在gpu端存储数据
  - [x] glGenBuffers
    1. just get a name not create and initialization
  - [x] glGetBufferParameteriv
  - [x] glGetVertexAttrib
  - [x] glGetVertexAttribPointerv
  - [x] glVertexAttrib
        vertex attribute data 与顶点着色器的交互，就是客户端将数据进行映射和加载，顶点着色器负责对数据进行采样，每一次执行为vertext shader中的所有attribute变量进行数据的采样赋值
        vertex attribute n 的数据来源类型
            1. const 类型: 将一个attribute variable 与之绑定，那么shader每次对于所有顶点的处理，该attribute变量的取值将会保持，相当于常量
            2. attribute array 类型: array 中存在的 attribute单位数量，表示了一共有多少个顶点数据被shader采样处理，所以，所有与attribute 变量绑定attribute array，他们表示的attribute成员数量要匹配, 否则解析时可能遇到内存越界
            3. shader对于的属性变量的处理，根据属性变量的位置由0到n依次为对应变量进行数据赋值(采样)， 两种选择: const attribute和attribute array，默认使用了const attribute，当调用enableattributearray/disableattributearray时，使能和关闭attribute array，可以提前准备两种类型的数据，然后通过该调用决定 shader中对应attribute variable 数据获取的方式
        对于数据单位的划分: 
          1. 纯数组数据 (data pointer) 有可能所有的顶点属性数组都存储在一起，按一定规律排列
          2. vertex shader 中 每一个attribute 变量 对应的采样数据源以及源属性 (vertex attribute array)
          3. 每一个 attribute 变量 所存储的 数据 (attribute)
          4. 一个attribute 变量数据中的每一个分量 (component)
          5. 要指定一个 shader vertex attribute 变量 所采样的数组，需要 
             1. 对应的数据指针，不适用vbo的情况下直接通过attributeArray，使用vbo的情况下就是当前绑定的vbo对应的数据指针
             2. 目标 一个 attribute 单元 分量的数量 component size
             3. 每一个 attribute 单元 之间的偏移 offset
             4. 第一个 attribute 单元 的第一个分量 的地址，即与数据指针的 offset
  - [x] glVertexAttribPointer
  - [x] glIsBuffer
    传入一共 buffer id/name 返回true/false 判断是否是已经bind the buffer object
  - [x] 在gles中一共有哪几种方式进行顶点数据的发送
  - [x] 各种方式的交互流程是怎么样的

#### plan
- [x] 阅读 opengles programming guide vbo 部分
- [x] 阅读 learn opengl vbo 部分，与 opengles 对比
    概念梳理
    一个顶点 拥有许多属性，位置属性，颜色属性，每种属性拥有不同数量的分量
    首先毫无疑问，一个顶点的一种属性数据 是不可拆分必须连续存储的，即一个vec3的属性，必然在数组中紧密连续存储三个分量，这是存储上基本的`逻辑单位`, 注意这是逻辑单位，即将一定数量的分量作为一个单位，而真正的存储单位是 具体分量
    对于所有顶点的数据存储，
    1. 一种是依次将每一个顶点 的 每一种属性数据 紧密连续存储，存储完一个顶点的所有数据，再继续存储下一个顶点的所有数据，也就意味着 一个缓冲指针 会存储所有的顶点数据
    2. 另一种，是将每一个顶点 的 每一个属性数据 进行分离，同种属性的数据存储在同一个缓冲区指针，一个顶点拥有5个属性，那么意味着有5个缓冲区指针分别存储5种属性，属性在其缓冲区的索引都表明其归属于 哪个次序的顶点，也就意味着通过同一个索引值引用到的属性 都是属于同一个顶点的，也就意味着改方式存储的数据，在缓冲区中的属性索引 indices 等同于顶点序号，所有缓冲区存储的属性数量是相等的，因为所有数据都是从 顶点中拆分出来，是成组的
    
    1. opengl     中存在的: const vertex data, vertex array data, VBO, EBO, VAO
    2. opengles 中存在的: const vertex data, vertex array data, VBO, EBO
    const vertex data: 所有顶点的某一个属性都为该指定值，故为const 常量的
    vertex array data: 描述的不仅仅是 一种属性 对应的 所有数据这么一个逻辑概念，他的关键在于，他是shader中一个attribute 变量 获取数据的唯二 来源之一，即除去 const vertex data的方式，只有在客户端通过调用 指定 vertex array data的方式，才能真正让attribute变量获取到数据

    VBO 和 EBO 本质上都是 buff object, 是两种类型的buff object，是句柄对象，该句柄对象的作用只有一个: 管理一个发送到 gpu 端的缓冲区，即buff指针，该指针是没有额外属性描述的，仅仅是指针+数据长度, 而BO的类型V和E决定了 opengl 对该缓冲区操作的默认意义，gl 会认为 VBO的缓冲区就是存储的属性数据，单位是属性的分量，EBO的缓冲区存储的就是索引indices，即 对应 vertex array 中属性排列顺序的顶点的次序

    BO与其他存储的关联性: 
    VBO 对象绑定的缓冲区存储顶点属性数据，目的在于仅仅是提前存储， 和绘制操作的关联性： bindbuff 操作如果将GL_ARRAY_BUFF类型的target与有效vbo绑定，那么 最后调用 vertex array 的时候，会默认使用 vbo 中存储的指针数据   
    EBO 对象绑定的缓冲区存储索引数据，即是表示的序号/位置信息(顶点序号信息)，和绘制操作的关联性: 1.存在前提，已经有vertex array data被加载了，那么此时 bindbuff 对应target类型后，在最后调用DrawArray时使用element类型的绘制，会使用ebo中存储的指针数据

    VAO (genvertxarray, bindvertexarray)对象绑定的缓冲区存储什么？在什么时候存储？在什么时候使用?
    VAO 设计的目的就是用于 存储 当前VBO 中 vertex array data 的引用配置快照
    VBO的存在，使得顶点数据在gpu端缓冲可以进行 复用，而不是反复以vertex array data为单位发送，
    但是关键还是在于 vertex array data 的绑定，每次绘制 都对应着n个vertex array data的切换和绑定，
    不存在VAO的情况下，每次绑定一个vertex array data都需要执行绑定VBO，然后调用vertexArray接口进行数据描述的操作
    若是存在VAO，那么第一次执行VBO的操作依旧保持，但是如果在执行时，bind了一个vertexArray对象，那么此时全局的VBO执行状态就被快照到VAO中，
    在后续的操作中如果想执行VAO之前对应的操作，只需要重新bindvertexArray就可以直接切换回当时对应的VBO全局状态，直接调用drawarray即可，所以重点在于VAO是用于简化客户端操作的语法糖，存储VBO相关的全局状态, 切换bind状态以减少操作次数
    
