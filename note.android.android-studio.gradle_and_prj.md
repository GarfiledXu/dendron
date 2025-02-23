---
id: uaetimg7gwwi9f5x3312ioc
title: Gradle_and_prj
desc: ''
updated: 1722242190325
created: 1694496520962
---

### reference
--------------
- [blog-zhihu-gradle在android项目中的结构](https://zhuanlan.zhihu.com/p/605490012)
- [blog-zhihu-理解Android中的gradle](https://zhuanlan.zhihu.com/p/139685763)
- [简述 系列: Android Gradle最佳实践](https://juejin.cn/column/6985104149218082829)

### prj info in gradle
------------
`rootProject.name` field determine the top project name in `settings.gradle` script, it's not a real folder name, typically viewed follow folder name with [] in Project view mode.



### view and edit of project gradle configure with GUI dialog  
------------------




### project compile flow wtih gradle process
----------------



### how to custom gradle script to implement some function applied during compilation
------------------



#### 拷贝项目，如何修改包名和项目名
- 拷贝项目工程
- 修改 `settings.gradle` 中的rootProject.name
- double-shift 全局替换关键词