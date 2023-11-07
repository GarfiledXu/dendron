---
id: 14yzdk5xp41f9k1j39tspwh
title: Error List
desc: ''
updated: 1696652317311
created: 1695190099401
---

#### gradle compile error
-----------------
**build error**
```gradle
Circular dependency between the following tasks:
:app:processDebugResources
\--- :app:processDebugResources (*)
```
> 错误原因：在build.gradle中引用了模块本身 `implementation project(path: ':app')`
点击编译会立刻跳出该错误
1. 意味着错误在开头的脚本解析阶段就发现了
2. 循环引用的提示，意味着可能与引用相关

#### project configure fail
**build error**
no one variant be found
> 错误原因：仅仅是引入的lib本地库依赖不存在找不到，导致整个模块编译脚本失败