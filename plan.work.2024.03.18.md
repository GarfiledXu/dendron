---
id: sow4rs60zug7t8ee9sl4c8w
title: '18'
desc: ''
updated: 1710761506709
created: 1710723791707
---

#### plan
- [ ] cs11 稳定性功能实现
- [ ] byd testapp 修改报警布局
- [ ] 8295 设备端服务部署
- [ ] image render 需求整理
- [ ] 实现 uyvy 渲染
- [ ] 添加 yuvlib 库
- [ ] 瑞驰曼sdk 取流testbed
- [ ] 整理 window 窗口管理机制
- [ ] 搜集ui框架设计的书籍


### 稳定性功能列表
- [x] 新增 tab 组件整理切换大功能
- [ ] loop帧率控制模块
- [x] 语言模块
- [ ] 语言切换开关组件
- [x] 触发稳定性后所有其他功能组件disable
- [ ] 终止开关
- [ ] 显示状态情况: 是运行中/运行结束/未进行
- [ ] 时长设定组件
- [x] 设定时长后的progress bar进度显示组件
- [x] 内存状况曲线图组件
- [ ] 初始化-反初始化
- [ ] 所有功能遍历并且选项随机
- [ ] 后台capture 内存状况
- [ ] 后台信息记录
- [ ] 稳定性交互逻辑
    1. 时长设置
    2. 功能选择
    3. disable 无关组件显示
    4. 记录信息的文件名和路径显示
    5. 开始运行，记录信息到本地，并且显示进度以及状态曲线组件
    6. 运行结束, 开关enable，状态显示结束