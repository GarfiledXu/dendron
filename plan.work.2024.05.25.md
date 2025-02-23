---
id: l960k2lfx51fbcso6x6eaal
title: '25'
desc: ''
updated: 1716689852103
created: 1716603701058
---

#### todo
- [x] jni sdk 分离模块
- [x] jni gl render 分离模块
- [x] 修改 android studio 项目 根目录
- [x] 将模块建立共享
  - [x] 问题: 在一个project中设置并引用外部module以后，第一android视图结构体中 外部module无法查看到cpp内容，只有一个空的cpp文件夹，其次project引用能够成功，但在运行是会报错native接口没有找到实现
  ，最后发现无关，是由于jni 与java的包映射错误
- [ ] 为模块建立单元测试
- [x] wz 大号 三颗星
- [x] 渲染viewport 封装