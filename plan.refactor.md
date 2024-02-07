---
id: yj95dk1anap85lazb9xz688
title: Refactor
desc: ''
updated: 1707112445329
created: 1706836386908
---

##### tool
qnx:

    byd
    ep37
    ep40

android:
    
    honda25m

##### service
上层团队不同，下层的平台不同，提供的东西要标准化，统一
很多操作是与平台耦合的
面临重构
1. 文件更新: 具体包，具体文件, 内涵规则
2. 程序执行
3. 命令行执行
4. 重启控制
5. connect rabbitmq
6. connect samba
1. 设置状态机内部配置
5. 设备状态: 服务状态，磁盘空间，查询配置
error:
    samba non init
    rabbitmq non init

##### testapp


##### collection tool



##### python script
pkg compile simple script
complex compile script
ssh command script

##### 错误码重构
错误码机制是配合源码编写的，所以首要要一套命名规则，符合该规则的则表明函数实现使用了对应的错误机制，那么就可以放开写了
