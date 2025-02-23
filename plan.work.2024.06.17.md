---
id: ea11vh1rj5anei0dllol562
title: '17'
desc: ''
updated: 1718962193604
created: 1718589509448
---

#### 设备端代理服务
##### 优化重构
- [] 日志跟随任务滚动
- [] 日志任务信息记录
- [] 日志 以及 testbed 标准输出 core文件清理加回传 smb 回传

##### TDA4
- [ ] 整理编译工具链
- [x] 编译基本库
  - [x] samba
  - [x] curl
  - [x] rabbitmq-c
  ~~- [ ] amqpcpp 弃用~~
  - [x] miniz
  - [x] libevent
- [ ] 源码模块编译
  - [ ] spdlog
  - [ ] toml11
  - [ ] cli11
  - [ ] jsonhpp
  - [ ] mongoose
  - [ ] fmt
  - [ ] filesystem
- [ ] 封装模块编译
  - [ ] log

##### 优化
- [ ] 日志回传优化
  - [ ] 默认上传后不删除原文件
  - [ ] 补充: 每次上传后将对应的本地文件push到list中，上传后获取当前磁盘空间，小于阈值时，将list中的文件进行删除
  - [ ] testbed日志与testbed结果，以及server结果都按此规则进行回传与滚动
- [ ] reboot 开关 屏蔽调用，主动清除标志，来模拟成功重启
##### 额外功能
- [ ] 更新模块 
- [ ] 文件传输模块
- [ ] 进程控制模块: 杀死
- [ ] python 模拟调用
##### 配置
- [x] log 
- [x] server launch
- [ ] testbed_param
- [ ] update operation

##### 开发日志， 修复命令行解析 空格问题