---
id: onryb5jcm6eyq72le5iechp
title: Activity
desc: ''
updated: 1694769079999
created: 1693444300153
---



##### refe
-----
- (blog-Android 之Activity的生命周期和进程保和)[https://blog.csdn.net/LoverLeslie/article/details/84671122?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-4-84671122-blog-52045128.235^v38^pc_relevant_anti_vip_base&spm=1001.2101.3001.4242.3&utm_relevant_index=7]
- (blog-Android源码分析 —— Activity栈管理)[https://blog.csdn.net/weixin_43093006/article/details/129210275?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-1-129210275-blog-116757617.235^v38^pc_relevant_anti_vip_base&spm=1001.2101.3001.4242.2&utm_relevant_index=4]
- (blog-Android四大组件与进程启动的关系)[https://gityuan.com/2016/10/09/app-process-create-2/]
- (blog-Activity、Task、应用和进程)[https://www.cnblogs.com/franksunny/archive/2012/04/17/2453403.html]
- (offical-应用基础知识)[https://developer.android.com/guide/components/fundamentals?hl=zh-cn]
- (blog-安卓的进程模型)[https://juejin.cn/post/6905989151699992583]


#### the called process and mechanism of activity

#### relationship of linux process with activity and jvm

#### lifecycle
> 准确说这应该是 抽象生命周期，抽象的意义就是集中对外暴露回调
- [offical activity 生命周期](https://developer.android.com/guide/components/activities/intro-activities?hl=zh-cn)
- [offical layout](https://developer.android.com/guide/topics/ui/declaring-layout?hl=zh-cn)
- [blog-layoutInflater](https://www.cnblogs.com/androidez/p/3164729.html)
- [offical 生命周期感知](https://developer.android.com/topic/libraries/architecture/lifecycle?hl=zh-cn)
- [blog 生命周期感知](https://www.jianshu.com/p/22295c0a48ee)
ref : Activity 类是 Android 应用的关键组件，而 Activity 的启动和组合方式则是该平台应用模型的基本组成部分。在编程范式中，应用是通过 main() 方法启动的，而 Android 系统与此不同，它会**调用与其生命周期特定阶段相对应的特定回调**方法来**启动 Activity**实例中的代码。
    > 由官方描述可以理解到，受限于移动端设备的显示范围，以视觉为导向，会突出放大交互元素，activity的诞生，其目的就是为移动端应用交互提供更便利的范式，在实现上将应用的功能与界面划分出子模块去对应不同的activity，单个activity负责管理自身的ui和交互，activity间聚焦于切换和跳转

    > 在编程行为上，开发者只负责activity的继承类定义，与信息注册，由android 的编译框架根据注册信息与定义，来生成对象实例化以及引用部分的代码，解耦框架主线程代码与应用自定义代码的逻辑关联, 即开发者只负责单元的定义和声明，不负责实例代码的组织和插入(这些都由框架操作)，不同 activity 间通过回调传递中介对象来进行交互，而 activity 的回调由 用户事件和系统事件 触发推动（**事件->activity抽象状态转变->主线程执行对应回调**） 并 由框架后台主线程执行

    > 为什么要为 activity 设计不同模式，

#### 使用阶段
- 在清单文件进行注册
  1. activity name 添加
  2. intent 过滤器声明
  3. 权限声明
  > android 的 Manifest 文件设计，将开发阶段，编译期，应用顶层的配置声明，在编译时被编译框架引用，进行代码生成并插入，简而言之就是将不必要的
- activity callback of lifecycle
  1. onCreate()
     1. 配置界面布局
     > 一个 activity 加载与其关联的界面布局xml，通过 setContent 函数，该函数重载了多个版本，可以接收资源id，也可以直接接收一个 view或者是根view ，**如何获取根view?** 在开启 data binding 的情况下，通过binding对象的**getRoot()**可以返回bind对于的根view, **那么binding自身是如何实例化的?** binding通过自身的静态函数inflate返回，输入参数为LayoutInflater对象 **即布局解析器** ，通过activity中getLayoutInflater()返回一个layoutinflater实例，**getLayoutInflater 获取布局解析器的原理是什么?** 通过activity内含的context调用获取 **Context.getSystemService()** 而  **binging本身是通过命名约定的形式与xml文件绑定** 的，所以这个过程不需要资源id的显示指定，binging类型名就附带该信息
     **总结：** 本质上最后都是通过inflater(布局加载器)将资源转化为view，而setContent则是activity中进行view解析的，一个多态函数。inflater的获取本质依赖上下文context调用返回
     2. 实例化类作用域变量
     3. 在生命周期中只调用一次
     >  onCreate 意味着创生，那么哪些事件会触发销毁？
  2. onStart()
     1. 该回调存在的意义是什么? 如何从实际case进行举例？经常提到的可见不可交互是什么含义？
     >  
     2. 回调与状态？
     ![image](image/lifecycle-states.svg) 
     > **回调不是状态，他是介于状态间插入的操作点, 是状态前的插入**
     3. 可见，前台可见


#### question
- 在 activity 进行跳转切换的过程中，包含在 activity 中的成员以及数据会被如何处理，是作为类成员被GC，还是暂存？亦或是需要进行部分操作进行上下文的保存？正确的处理规范是什么？
  