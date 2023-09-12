---
id: v7sk4m6c4qofaadwelre02f
title: Opengl
desc: ''
updated: 1694437100845
created: 1691395237731
---

## android opengl framework API
- [OpenGl ES](https://developer.android.com/guide/topics/graphics/opengl?hl=zh-cn)
- [android.opengl](https://developer.android.com/reference/android/opengl/package-summary)
- [use opengl display graphic](https://developer.android.com/training/graphics/opengl?hl=zh-cn)

> Android 通过 `NDK` 以及 `框架 API(java)` 来支持 OpenGL
> 本篇着重聚焦于在 android 平台上，使用框架 API 进行OpenGL基本功能的调用

### OpenGL version and platform
-------
#### OpenGl ES
- [GL es3 reference](https://registry.khronos.org/OpenGL-Refpages/es3/)
- [GL es3 shader reference](https://registry.khronos.org/OpenGL/specs/es/3.2/GLSL_ES_Specification_3.20.html)
- [GL es2 quick reference](https://www.khronos.org/files/opengles20-reference-card.pdf)
- [GL 3.2 quick reference](https://www.khronos.org/files/opengl-quick-reference-card.pdf)
- [GL es2 online reference](https://registry.khronos.org/OpenGL-Refpages/es2.0/)

-------

### GLSurfaceView
-----
- [android.GLSurfaceView](https://developer.android.com/reference/android/opengl/GLSurfaceView)

- GLSurfaceView and GLRender
  - GLSurfaceView 托管了gl渲染的EGL上下文窗口管理，以及窗口所在的view的绘制，渲染线程的创建，以及绘制动作的控制
  - GLRender 则是渲染流程中关键步骤的回调集合，由用户继承并实现绘制逻辑，在 GLSurfaceview 实例化时，传入对应 surfaceview 中，由内部线程进行调用
  - the `requestDraw` of GLSurfaceView , 该函数并非同步主线程与渲染线程动作，内部实现仅仅是设置了标志位，渲染线程以最大频率轮询 `onDraw` 回调, 并在执行 onDraw 之后进行 egl 的 swapbuff, 在此流程的基础上，GLSurfaceView 对外提供了几个渲染流程控制的模式，`持续模式` | `脏标志位模式`, 持续模式: 以最大频率不跳过任何一次 onDraw 回调以及swap buff, 脏标志位模式: 渲染线程每一次在调用 onDraw 之前都会判断标志位，为脏则绘制，并且绘制完毕将其清理，在主线程代码逻辑中通过 requestDraw 设置标志位为脏，表示可渲染。
  - 控制效果: 当主线程发出requestDraw时，渲染线程正在执行onDraw时，那么也就意味着这一次的request无效，实际效果仅仅是主线程主动更新了渲染相关的buff，因为onDraw调用结束会将标志清理
  - 通常，主线程更新buff的频率远低于渲染线程的频率，那么基本上主线程每次的request都会被消费，因为消费的快，等下次生成出来时，渲染线程已空闲，但这仅仅是基本，还是有几率在update的同时，渲染线程在执行ondraw，那么此时的并行场景下，倘若渲染直接引用update那么就有可能数据冲突
  ```java
  //冲突场景
  //渲染线程
  void ondraw(){
    rectangle.read();
    render();
    rectangle.clear();
  }
  //主线程
  void update(){
    rectangel.update();
  }
  request();
  ```
  > 上诉场景，当update与ondraw并行时，就有可能update()->clear()->dirty ready -> request() 最后丢失了update的数据
  - **设计思考:**
    - 关于绘制buff的更新，是直接引用传递+引用代码块上锁？
    > 直接让渲染线程使用未知的引用必然存在安全问题，而要让引用来源安全则只有copy，将request与ondraw互斥，在持续模式下，将严重阻塞主线程的代码执行，而仅仅在脏标志模式且，低频的request调用下能够保证每次的request消费，由此结论：不可取
    - 直接buff拷贝+拷贝代码块上锁？
    > 直接的buff拷贝意味着每次主线程最后的request无效时，拷贝将做无用功，若自增标志位来在buff拷贝前判断是否ondraw结束，来决定是否进行buff拷贝，意义并不大，极有可能在拷贝前ondraw，拷贝后ondraw结束，正好可用进行渲染，否则将错过一帧的数据渲染

  - 流程思考:
      - 主要还是临界区中不可靠/不可控代码逻辑造成的另一方互斥对象的被阻塞
      1. 使用一块buff，用于主线程更新拷贝，以及渲染线程使用？
      > 若渲染线程使用时，意外阻塞，主线程将阻塞
      2. 一块buff专用于外界拷贝，另一块buff则是在真正引用时对第一块buff进行用时二次拷贝
      >  如此，拷贝将是可靠的，并且在持续模式下，不会频繁阻塞主线程拷贝，成本则是每次多了一次拷贝
      3. 1块buff，加拷贝时timeout机制，一旦拷贝超时则跳过当前更新，直接进入下次更新，解决了主线程被阻塞问题，但相当于跳过拷贝，如果场景：跳过拷贝->渲染线程ondraw刚执行完毕->二次ondraw,此时二次拷贝未完成->那么就会渲染旧帧，最后的效果是跳帧，关键跳过了拷贝不意味着节省了拷贝时间，一旦跳过拷贝必然是耗费了timeout，故仅仅是跳出阻塞并没有提高效率以及效果
      4. 2块buff，二次拷贝，在拷贝时加入timeout机制，来防止意外
      5. 2块buff，一次拷贝，用时置换，保持一次稳定拷贝，对比第四种，减少了用时拷贝，而是交换引用

  - 数据结构设计:
      1. 固定双buff 数据结构? simple，场景针对
      2. 封装pipeline 数据结构？添加模式选择，双buff or queue 模式，阻塞和非阻塞，支持引用、拷贝传递，以及重复，唯一，分发的拷贝、引用获取