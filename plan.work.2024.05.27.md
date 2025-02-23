---
id: 3v4ag58ygro6s4yzfy6hbz4
title: '27'
desc: ''
updated: 1716776285967
created: 1716726539855
---

#### 平台设备端代理 重构优化与开发计划

- [ ] 重构优化: rabbitmq  + smb 模块重构 (预计进行时间段: 5.30~6.4)
  - [ ] rabbitmq 通信模块 [qnx700 | qnx710]
    - [ ] publisher 封装
    - [ ] receiver 封装
    - [ ] 本地rabbitmq 代理环境搭建
    - [ ] 命令行功能模块测试
    - [ ] 集成测试
  - [ ] smb 传输模块 [qnx700 | qnx710]
    - [ ] downloader 封装
    - [ ] uploader 封装
    - [ ] 本地smb 代理环境开启
    - [ ] 命令行功能模块测试
    - [ ] 集成测试
  - [ ] 舱内设备端sdk 以及 设备端算法测试 平台 模块替换更新 [qnx700 | qnx710]
    - [ ] 替换基线重构模块: rabbitmq + smb
    - [ ] 上线进行集成测试
    

- [ ] 开发: 舱外业务开发 (预计进行时间段: 6.5 ~ 6.15)
  - [ ] 交叉编译环境调试
    - [ ] 模块编译测试
      - [ ] rabbitmq 模块 [arm linux tda4]
      - [ ] smb 模块 [arm linux tda4]
    - [ ] 自定义模块 编译测试
      - [ ] (log 解压缩 filesystem cmd cfg 线程 队列 组件) [arm linux tda4]
  - [ ] 任务管理: testbed 与服务进程间通信  [ arm linux tda4 | qnx700 | qnx710]
    - [ ] mqtt 服务端 客户端 封装
    - [ ] 协议内容定义
    - [ ] 命令行功能模块测试
    - [ ] 集成测试
  - [ ] 任务管理: testbed 命令行解析管理模块 [arm linux tda4 | qnx700 | qnx710]
  - [ ] 任务管理: 设备更新 模块开发 [arm linux tda4]
  - [ ] 素材管理 模块开发 [arm linux tda4 | qnx700 | qnx710]
    - [ ] 挂载管理功能
    - [ ] 素材缓存功能
    - [ ] 集成测试
  - [ ] 与设备管理平台 心跳功能测试 [arm linux tda4]
  - [ ] 完整的平台联调 业务流程 集成测试 [arm linux tda4]


- [ ] 重构优化: 任务管理模块 多平台 基线统一 (预计进行时间段: 6.17~6.21)
  - [ ] 舱内 | 算法测试 | 舱外 i30 [arm linux tda4 | qnx700 | qnx710]
    - [ ] 任务/设备状态管理
    - [ ] 任务线程管理
    - [ ] 设备更新管理
  - [ ] 舱内 | 算法测试 | 舱外 objective [arm linux tda4 | qnx700 | qnx710]
    - [ ] 任务/设备状态管理
    - [ ] 任务线程管理
    - [ ] 设备更新管理
  - [ ] 集成测试
    - [ ] 舱内平台测试
    - [ ] 设备端算法平台
    - [ ] 舱外平台测试

#### 0526周计划
- [ ] 商用车 宇客 avm testapp 
  - [ ] bsd功能验证
    - [ ] 视频解码以及渲染
    - [ ] bsd jni 封装 接口验证
  - [ ] 在线激活
  - [ ] avm自动标定功能验证
  - [ ]  工程结构优化
    - [ ]  抽离native渲染 jni封装 module
    - [ ]  抽离avm sdk jni封装 module
- [ ] byd pa分支 客观测试工具新接口接入
- [ ] 商用车 雅迅 avm testapp 接入
- [ ] 平台设备端 模块重构优化
