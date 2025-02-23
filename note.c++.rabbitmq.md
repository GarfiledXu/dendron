---
id: bgxiajw7pme42jbdigagllt
title: Rabbitmq
desc: ''
updated: 1715000291742
created: 1711092236777
---

### http API
[.Net RabbitMQ实战指南——HTTP API接口调用](https://www.cnblogs.com/Stacking/p/rabbitmq-http-api.html)
[HTTP API 接口管理](https://zq99299.github.io/mq-tutorial/rabbitmq-ac/05/06.html#rabbitmqadmin)
[RabbitMQ Management HTTP API](https://blog.csdn.net/db2china/article/details/120366312)

[rabbitmq offical: basic model concept](https://www.rabbitmq.com/tutorials/amqp-concepts) 这篇 tutorial 介绍了 AMQP 协议的基本概念
[rabbitmq offical: basic api refe](https://www.rabbitmq.com/amqp-0-9-1-quickref#class.basic) 快速的引用预览
[rabbitmq offical: complete reference](https://www.rabbitmq.com/amqp-0-9-1-reference) 完整的引用说明
[rabbitmq offical: protocol specification](https://www.rabbitmq.com/amqp-0-9-1-protocol) 里面放置了各种协议的具体规范说明
[rabbitmq offical: the method and rules for each version AMPQ specification](https://www.rabbitmq.com/docs/specification) 里面罗列了各种版本的AMPQ协议的方法 规则

[rabbitmq offical: client library each language](https://www.rabbitmq.com/client-libraries/devtools)

[rabbitmq offical: installed tutorial](https://www.rabbitmq.com/docs/download)
[github: offical each language example](https://github.com/rabbitmq/rabbitmq-tutorials)

[rabbitmq offical: python tutorial](https://www.rabbitmq.com/tutorials/tutorial-one-python)

#### rabbitmq 与 AMQP
- [rabbitmq学习（一）：AMQP协议，AMQP与rabbitmq的关系] (https://www.cnblogs.com/wutianqi/p/10043011.html)
- 总而言之: rabbitmq 是 AMQP 的实现，以及扩展

#### http 与 AMQP
- http 是文本消息协议，报文在进行传输的前后都需要进行编码
- AMQP 是 binary message protocol, 二进制消息协议，直接进行二进制数据消息传递的

#### 插件功能
前端管理界面和http接口管理都是基于rabbitmq的插件

##### 前端管理界面
- 前提条件: 进入，代理程序已安装，并且对应插件安装
- 登录 url : http://localhost:15672
- 

##### http接口调用



##### rabbitmq c++ library 交叉编译


#### vscode  windows conda rabbitmq tutorial python pika install
install pika on conda python env on vscode env of windows
- [rabbitmq: python tutorial](https://www.rabbitmq.com/tutorials/tutorial-one-python)
- [github: about pika](https://github.com/conda-forge/pika-feedstock) 
```bash
conda config --add channels conda-forge
conda config --set channel_priority strict
conda install pika
```


#### win10 rabbitmq 服务启动
**首先最重要的一点，要管理员省份运行cmd**
```bat
net stop rabbitmq
net start rabbitmq
rabbitmqctl start_app

#为了能够登录
rabbitmqctl set_user_tags guest administrator
```

#### client operation
```c++
Login was refused using authentication mechanism PLAIN.
该情况是由于使用guest guest作为用户名和密码登录远程的broker，得用其它账号

PRECONDITION_FAILED - inequivalent arg 'durable' for queue 'helloworld_queue' in vhost '/': received 'true' but current is 'false'

```