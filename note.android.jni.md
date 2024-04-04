---
id: p7st5gk8a0we81a5dog608l
title: Jni
desc: ''
updated: 1712020415316
created: 1711773670355
---
#### ref
- [google: ndk dev jni 提示](https://developer.android.com/training/articles/perf-jni?hl=zh-cn)
- [oracle: jni specification](https://docs.oracle.com/javase/7/docs/technotes/guides/jni/spec/jniTOC.html)
- [字节流动: 深入 Android 系统 - Android 的 JNI](https://cloud.tencent.com/developer/article/2122049)

#### JVM 实现原理概览
1. jvm 的生命周期 
2. jvm 的内存布局 和 内存管理
3. jvm 的简易 c++ 实现
   - [github: jvm个人实现](https://github.com/ArosyW/JVM)
4. jvm 发展历史
5. jvm 种类


#### first face
1. 首先 java 程序的实体 不是 c/c++ 的可执行程序二进制文件，而是由 javaVM 程序加载并解析的 字节码文件， 所以在 google 的 android 开发文档中，通常 调用 java 接口的代码称之为受管理代码，这些代码的直接映射并不是 c/c++ 编译后的二进制程序中的函数地址, 而是 java 字节码指令，受javaVM 在运行时动态解析和执行, 所以一个基本的思路: java 调用 c/c++, 本质上是要通过两层传递，1. java调用 => javaVM 2. javaVM => c/c++ 符号调用  而映射协议需要解决两个问题: 1. 符号的映射关联: java的接口map c/c++ 接口 2. 上下文数据的传递和共享规则: 当 java 需要将本身的数据传递到 c/c++ 接口中，必然 首先需要存在基本的数据类型映射才能数据传递， 其次 java 所有的符号数据 都是在 javaVm 中组织管理的，并没有在系统层面进行直接映射，那么 c/c++ 中要处理传递进来的 java 数据，必然还有拥有 javaVM 的有关上下文 handle 才能访问，即 c/c++ 通过 在接口中 传入 javaVM 的handle，就能按一定规则访问到 java 数据

2. 讨论 jni 交互过程中，c/c++ 调用中 和 java 数据相关的 共享性 全局性 局部性 生命周期问题 以及线程相关性，这些会涉及到 javaVM 本身的设计和布局
   1. 在 原生系统层面 每一个线程(内核线程)都有自己的执行逻辑，并拥有线程本地堆栈，用来存储函数栈帧和线程私有数据
   2. 在 java 语言中，java 线程的实现依托的还是内核线程，那么毫无疑问java线程的初始化，首先需要实例化一个内核线程，然后关联/attach 到javaVM中进行被管理，那么 java 线程对象 会有存在哪些数据结构，用于线程管理以及与内核线程的映射？这将会直接影响 jni 中 java部分的数据生命周期管理
   3. 每一个java线程 都会拥有自己的函数调用栈，记录当前线程的java函数调用，这与原生线程的调用栈不冲突，可用理解为javaVM对线程的java调用记录
   4. 由于javaVM中GC机制，内存回收器的存在，那么将会对 jni 中 原生接口内部 java 数据的处理产生什么影响?

3. 实现的基本原理: 将so 装载进进程内存空间，使得每一个符号生成了具体地址，再将so中的原生符号 进行注册(即将符号地址存入关联性的数据结构对象)，形成java符号到c++符号的映射: java 层函数名和函数签名对应的字符串，以及对应的符号地址

5. jni 原生接口的注册方式:
    1. 动态注册
       1. 起手在c++层，实现一个 固定的入口函数: JNI_OnLoad() , 在该函数内通过javaVM上下文来 手动注入 与c++层函数以及地址映射的 java函数签名以及关联的属性信息，这也就意味着在这种注册模式下，java层调用的接口函数签名和包信息，都是在 c++层的实现里 在动态注册时一起指定的
       2. android工程中的，src的cpp模块，是一个独立的cmake模块，与外部java没有任何关联，仅仅在工程的build.gradle编译脚本中被引入jni编译，并将产出的目标so和依赖so加入工程产出物进行打包，java 模块仅仅是负责定义native的java接口和在静态代码块中loadlib
       3. 优点: 所有信息的注册填写绑定都在cpp的实现中，java接口与cpp接口函数名称不必按固定规则映射，灵活度大，java层只需要编写好接口声明即可
    2. 静态注册
       1. 根据固定的映射规则，先声明java接口，然后通过jni代码生成器生成cpp接口声明，然后进行实现
       2. 每一个接口的调用都需要在 so 中进行匹配查找，来获取到目标函数地址，效率低
       3. 改动不能集中，一旦java接口改动，也就意味着复杂的cpp接口名称需要重新生成，同理，cpp想要改动意味着得从java接口角度考虑进行修改
       - [JNI动态注册、静态注册实例及其实现原理分析](https://zhuanlan.zhihu.com/p/520523247)
       - [深入探索Android热修复技术原理读书笔记 —— so库热修复技术](https://www.cnblogs.com/huansky/p/14757298.html)
       - [Android JNI 原理 - loadLibrary 动态库加载过程](https://rangerzhou.top/2019/10/25/Android/JNI-Study/#comment-gitalk)
       - [Android JNI原理分析](https://gityuan.com/2016/05/28/android-jni/)
       - [JNI函数注册及SO加载原理](https://www.jianshu.com/p/4ab830aa935e)
       - [jni pdf](https://www.uni-ulm.de/fileadmin/website_uni_ulm/iui.inst.200/files/staff/domaschka/misc/jni_programmers_guide_spec.pdf)

6. 动态库链接问题
   1. 当程序通过jni 加载多个动态库时，并且使用动态注册来进行函数映射，对于多动态库拥有同名函数，是否会有问题？
   - [同名函数在程序，静态库，动态库中的覆盖和调用顺序](https://developer.aliyun.com/article/243750)
   - 可以总结道: 多so 拥有同名函数并不会报错，只会对函数符号进行覆盖，那么意味着只要 loadlib 在整个javaVm进程运行过程中是顺序执行的，那么就可以保证loadlib中的JNI_OnLoad()可以被依次执行，也就意味着所有的符号地址映射关系会被存储到javaVM的上下文中，不会有冲突，如果原有主程序或者链接的静态中存在同名函数，那么动态库中的同名函数会被覆盖，忽略, 默认使用静态库中同名符号版本