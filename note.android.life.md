---
id: xlztymkhvhv0ndhat5uimfp
title: Life
desc: ''
updated: 1713068534166
created: 1713068360253
---

### activity 声明回调 与 permisson
按理进入activity后执行顺序为 onCreate() -> onStart() -> onResume() 
但是遇到: onCreate() -> onStart() -> onResume() -> onPause() -> onResume()
发现: 由于在onCreate处执行了requestPermissions(new String[]{WRITE_EXTERNAL_STORAGE, READ_EXTERNAL_STORAGE}, 1000);而触发
如果先check再request就不会

所以 onResume 和 onPause 也不是非常合适作为渲染生命周期的触发函数