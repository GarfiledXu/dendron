---
id: bt6ob0c03q0o1bnxhclr0oo
title: '13'
desc: ''
updated: 1716003674087
created: 1715566263879
---

#### plan 
- [ ] samba
  - [ ] 添加后台下载模式
  - [ ] 添加一个后台下载基类
  - [ ] 继承对应的 http 以及 smb下载子类 
- [ ] rabbitmq
    - [ ] 通讯模型基类
      - [ ] 队列模式 监听-回调-发送的通信
      - [ ] 客户端 服务端 模式 请求+回应
      - [ ] 发布 订阅 模式 publish + 
    - [ ] 完全模拟平台队列操作
    - [ ] 并实现两个命令绑定
        - [ ] 输入数据列表文件，发送数据
        - [ ] 接收数据，并保存到数据存储文件中
    - [ ] 模拟终止情况
        - [ ] 外部public 终止调用
        - [ ] ctrl c 信号终止
        - [ ] 异常情况终止
    - [ ] 连接断开-重试情况
        - [ ] 错误-链接 记录
        - [ ] 重试机制
        - [ ] 最终错误抛出
- [x] samba 旧版封装代码替换
- [x] rabbitmq 旧版封装代码替换
- [ ] 自测条件准备
  - [x] testbed 以及压缩包准备
  - [x] http testbed url
    - [x] D:\url_test
    - [x] http://172.17.186.67//xjf//url_test//
    - [x] http://172.17.186.67//xjf//url_test//platform//2024-05-15_21-07.zip
    - [x] http://172.17.186.67//xjf//url_test//platform//license.data
  - [x] smb 账号 smb 文件夹
    - [x] C:\Users\xjf2613\Desktop\smb_test
    - [x] C:\Users\xjf2613\Desktop\smb_test\platform
    - [x] smb://172.17.186.67/smb_test/platform/result
    - [x] smb://172.17.186.67/smb_test/platform/material
- [x] 素材准备
  - [x] 根据素材目录生成 rabbitmq 消息列表文件
  - [x] 生成 smb 路径 校验
- [x] 准备所有请求
  - [x] 更新请求
  - [x] rabbitmq info 请求
  ```json
  http://172.17.185.251:12345/device_service/update/testbed_metiral
  {
    "task_id":3880,
    "testbed_pkg_path":"http://172.17.186.67//xjf//url_test//platform//2024-05-15_21-07.zip",
    "metiral_pkg_path":"x",
    "license_file_url":"http://172.17.186.67//xjf//url_test//platform//license.dat",
    "testbed_type":"2",
    "ip":"172.17.11.202",
    "createdTime":"2024-04-22 17:29:47"
}
{
    "createdTime": "2024-04-19 19:06:58",
    "exchange": "",
    "hostname": "172.17.186.67",
    "json_config": null,
    "metrial_number": 1000,
    "parent_task_id": 3863,
    "password": "xjf2613",
    "password_smb": "xjf.4776289f",
    "port": 5672,
    "receive_queue_name": "test_receive",
    "receive_routing_key": "test_receive",
    "send_queue_name": "test_publish",
    "send_routing_key": "test_publish",
    "testbed_type": "2",
    "upload_result_dir_path": "smb://172.17.186.67/smb_test/platform/result/01",
    "username": "xjf2613",
    "username_smb": "smb",
    "vhost": "/"
}
  ```
- [ ] 服务代码准备
- [ ] 分步验证
  - [x] 消息接收
  - [x] 消息返回
  - [x] 终止
  - [x] 运行
  - [x] 素材下载
  - [x] 结果上传
  - [x] 单线程版本
- [ ] 代码优化
  - [ ] 拆分额外逻辑
  - [ ] 只保留跑客观逻辑
  - [ ] 每一项操作具体逻辑图
  - [ ] 压力测试
    - [ ] 终止和初始化
    - [ ] 初始化 运行 终止
    - [ ] 异常情况
      - [ ] 线程池阻塞
      - [ ] 下载失败
  

- [x] android demo编译
  - [x] 源码库尝试编译
  - [x] 简单apk
  - [x] 验证c程序 进行apk操作

- [ ] face 工具
  - [ ] 新增素材列表文件 输入支持
  - [ ] 新增 多种素材格式