---
id: hqrq2dx1kvmm9iperlyhu2g
title: Extern_and_global_register
desc: ''
updated: 1703136281798
created: 1702868492132
---
#### .o .c .h 和 extern
推导: 对于存疑的部分，提出针对性的限制场景，去寻求答案，再从答案中找到规律，区分细节

场景1: 只存在一个a.c，没有对应.h，其定义了一个在全局作用域里的变量 int a_v=1; 如何将其提升为全局变量，使其他的.o可以使用?
> 只需要 a.c 中对变量进行 extern 声明，在需要引用的.o的.c源码中，先引入该变量的 extern 声明，然后直接使用即可，链接器会在链接阶段，检查这些符号依赖的合规性
> 故常规场景:用一个独立的.c存储全局变量定义，再用一个对于的.h extern声明，需使用全局变量的.c 则引用该头文件即可
全局变量的定义，引用规则: 
1. 全局变量的定义和引用的关键都是 `extern` 声明
2. 要使.o 中定义全局变量，则必须在定义前声明 extern
3. 要在.o 中引用其他.o的全局变量，也必须在引用前 extern
4. 由此 在链接阶段 连接器会将 目标文件.o 中的符号进行 地址关联, 生成完整的符号表
5. 不管 .h 还是 .c，都只是源码文本， 关键还是划分好 .o 目标文件来确定符号来源和范围

**拓展问题1：**
封装了一个单例的日志模块以后，该模块被所有项目共享，模块单例源码中有初始化配置部分(日志缓存路径等)，那么如何让所有引用的项目能拥有自己的配置呢？封装为接口，只能满足在进入main函数以后，运行时修改，不能满足在main前，第一次初始化即加载的需求
scheme: 本质上还是声明 定义 引用的关系，所有的项目引用，意味着引用符号要全局唯一，但须清楚每一个.o进行引用引用全局模块时，使用的是声明符号，只需要让每一个project准备自己的一份定义实现，所有project共享位于模块中的同一个声明，即可

**拓展问题2：**
在imgui中，showdemo模块的代码引用了imgui_draw.cpp中的定义的函数C，但在示例的源码处并未有任何地方引用imgui_draw.cpp文件本身，这与自我编程时常规的文件组织(所有定义符号都有对应声明的.h来供引用方使用)有违，事实：由于固化的行为带来错误的思考，.h本质上是为当前源文件带来全局符号(函数默认为全局作用域)声明，所以引用符号的关键在于是否有符号声明，而声明与定义的关联是由链接时.o决定，意味编译时 只要存在.c文件加入编译即可，而这一行为通常由编译配置控制，而不需要源码层面源码文件之间进行关联，可以保持独立
**梳理**: 在imgui中，所有的函数符号声明都是直接组织在 imgui.h 中，定义在分类好的.cpp 文件中，所以没有按 .h .c 文件成对的形式，在示例代码中只需include imgui.h 来获取所有符号声明，而关联具体.cpp定义, 只需要确保编译配置加入了需要的.cpp保证能在链接时找到，根据实际需要的符号不同，来配置选择需要加入编译的.cpp，头文件引入部分不需要改动
**总结**: 这是一种 include 头文件超集的做法, 隐式依赖，不通过引用头文件方式，只推荐高度内聚模块时使用这种方式
#### ffmpeg file layout
**allcodecs.c** -> allcodec.o 
```c++
extern const FFCodec ff_a64multi_encoder;
extern const FFCodec ff_a64multi5_encoder;
extern const FFCodec ff_h264_decoder;
```
**a64multienc.c** -> a64multienc.o
```c++
const FFCodec ff_a64multi_encoder = {
    .p.name         = "a64multi",
    CODEC_LONG_NAME("Multicolor charset for Commodore 64"),
    .p.type         = AVMEDIA_TYPE_VIDEO,
    .p.id           = AV_CODEC_ID_A64_MULTI,
    .p.capabilities = AV_CODEC_CAP_DR1 | AV_CODEC_CAP_DELAY,
    .priv_data_size = sizeof(A64Context),
    .init           = a64multi_encode_init,
    FF_CODEC_ENCODE_CB(a64multi_encode_frame),
    .close          = a64multi_close_encoder,
    .p.pix_fmts     = (const enum AVPixelFormat[]) {AV_PIX_FMT_GRAY8, AV_PIX_FMT_NONE},
    .caps_internal  = FF_CODEC_CAP_INIT_CLEANUP,
};
```
**h264dec.c** -> h264dec.o
```c++
const FFCodec ff_h264_decoder = {
    .p.name                = "h264",
    CODEC_LONG_NAME("H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10"),
    .p.type                = AVMEDIA_TYPE_VIDEO,
    .p.id                  = AV_CODEC_ID_H264,
    .priv_data_size        = sizeof(H264Context),
    .init                  = h264_decode_init,
    .close                 = h264_decode_end,
    FF_CODEC_DECODE_CB(h264_decode_frame),
    .p.capabilities        = AV_CODEC_CAP_DR1 |
                             AV_CODEC_CAP_DELAY | AV_CODEC_CAP_SLICE_THREADS |
                             AV_CODEC_CAP_FRAME_THREADS,
    .hw_configs            = (const AVCodecHWConfigInternal *const []) {
#if CONFIG_H264_DXVA2_HWACCEL
                               HWACCEL_DXVA2(h264),
#endif
#if CONFIG_H264_D3D11VA_HWACCEL
                               HWACCEL_D3D11VA(h264),
#endif
#if CONFIG_H264_D3D11VA2_HWACCEL
                               HWACCEL_D3D11VA2(h264),
#endif
#if CONFIG_H264_NVDEC_HWACCEL
                               HWACCEL_NVDEC(h264),
#endif
#if CONFIG_H264_VAAPI_HWACCEL
                               HWACCEL_VAAPI(h264),
#endif
#if CONFIG_H264_VDPAU_HWACCEL
                               HWACCEL_VDPAU(h264),
#endif
#if CONFIG_H264_VIDEOTOOLBOX_HWACCEL
                               HWACCEL_VIDEOTOOLBOX(h264),
#endif
                               NULL
                           },
    .caps_internal         = FF_CODEC_CAP_EXPORTS_CROPPING |
                             FF_CODEC_CAP_ALLOCATE_PROGRESS | FF_CODEC_CAP_INIT_CLEANUP,
    .flush                 = h264_decode_flush,
    UPDATE_THREAD_CONTEXT(ff_h264_update_thread_context),
    UPDATE_THREAD_CONTEXT_FOR_USER(ff_h264_update_thread_context_for_user),
    .p.profiles            = NULL_IF_CONFIG_SMALL(ff_h264_profiles),
    .p.priv_class          = &h264_class,
};
```


#### 何为注册?
本质上就是将某类型的数据对象存储到某个受管理的数据容器中, 用于遍历处理

#### ffmpeg 中的编解码器等组件的定义，声明，注册的组织结构
1. 每一种具体编解码器在对应具名的.c中进行实现，并在源文件的最后，以FFCodec结构体的形式封装 ，将具体实现填充到一个 FFCodec 对象中
2. 在 allcodec.c 中 `extern`声明所有的编解码器
3. 在 configure 阶段，人为进行选项配置以后，在编译阶段，configure 脚本将自动生成源文件 libavcodec/codec_list.c , 来存储一个static codec_list数组
4. 而在 allcodec.c 中已经预先 include codec_list.c 将其作为头文件，将静态数组引入到当前 .c 文件中, allcodec.c 中同时定义直接引用该静态数组的所有操作, 这个模块可以看作是 static 修饰使用的一个范例，以文件作为隔离，去约束全局变量
> 由此可知，ffmpeg组件的注册自动化是由 configure 脚本在编译阶段，通过代码生成的方式实现的
> 了解ffmpeg编解码器的定义声明以及注册的组织结构，有何意义? 
> 可以进行自定义编码器的扩展，在源码层面，添加到ffmpeg的框架中

#### question
**为何对于所有的编解码器对象的 `extern` 声明只存在于 `allcodec.c` 文件中，其他模块如何引用?**
##### refe
[blog: short: ffmpeg中的codec_list.c文件生成过程简介](https://21xrx.com/Articles/read_article/250624)
[blog: ffmpeg 注册编解码器](https://www.cnblogs.com/545235abc/p/16282572.html)
