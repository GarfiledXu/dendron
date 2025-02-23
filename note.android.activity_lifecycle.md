---
id: ona0pe3jslkfs78byg61831
title: Activity_lifecycle
desc: ''
updated: 1717310653711
created: 1717245211060
---

### 应用行为 与 activity 回调触发

#### 子activity 的进入与进出
**onStart onStop会唯一触发**
进入
activity: onStart
GLSurfaceView: onResume
activity: onResume
GLSurfaceView: onAttachedToWindow
activity: onPause
activity: onResume
退出
activity: onPause
activity: onStop
GLSurfaceView: onPause
GLSurfaceView: onDetachedFromWindow


#### activity 生命周期与 GLSurfaceView
官方源码注释链接
[GLSurfaceView](https://android.googlesource.com/platform/frameworks/base/+/master/opengl/java/android/opengl/GLSurfaceView.java)
```c++
 * A GLSurfaceView must be notified when to pause and resume rendering. GLSurfaceView clients
 * are required to call {@link #onPause()} when the activity stops and
 * {@link #onResume()} when the activity starts. These calls allow GLSurfaceView to
 * pause and resume the rendering thread, and also allow GLSurfaceView to release and recreate
 * the OpenGL display.
```
GLSurfaceView 对外提供接口 对glthread的资源进行初始化和释放，调用前提是 render初始化成功
在activity中需要进行显示调用，跟踪activity的生命周期 onStart 和 onStop
**不仅要停止glthread，还需要停止调用render部分的逻辑， 否则会和其它activity glthread内容混合**
```java

    @Override
    protected void onStart() {
        Log.w(TAG, "onStart");
        super.onStart();
        if(mMyGLSurfaceViewRender != null){
            mBinding.myGLSurfaceView.onResume();
        }
    }

    @Override
    protected void onStop() {
        Log.w(TAG, "onStop");
        super.onStop();
        if(mMyGLSurfaceViewRender != null){
            mBinding.myGLSurfaceView.onPause();
        }
        if(mThreadLooper != null){
            mThreadLooper.signalStop();
            mThreadLooper.join();
        }
    }

```


##### Activity 和 Application 的实例化
在应用代码编写中，并没有进行实例化对象存放的入口, 这是因为 activity 和 application 的实例是由框架自动生成，并且进行注入和调用，而具体的 框架该实例化哪种类型的activity和application 则是由开发人员进行 手动注册，这就是manifest的作用，配置和控制整个应用顶层的对象实例
即框架会管理 关键类和工具类的实例，开发者通过对应的接口直接获取，只继承定义，而不进行实例化


##### 为什么进入activity后 onResume会被执行两次
[Activity在各种状态下的生命周期](https://zhaokaiqiang.gitbook.io/the-analysis-of-android-key-point/activity/activity_lifecycler)
[other: Launcher是如何实现开启一个App的？](https://zhaokaiqiang.gitbook.io/the-analysis-of-android-key-point/activity/activity_launcher)