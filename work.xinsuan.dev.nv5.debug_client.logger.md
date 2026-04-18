---
id: 31xjr1uurql4htcuye37yn7
title: Logger
desc: ''
updated: 1776505465010
created: 1776499726099
---

## ref

- [offical manual](https://loguru.readthedocs.io/en/stable/overview.html)

## self formatter

```c++

```

## feature list

1. 自定义sinks，可以理解为 message 的处理后端，通常在代码中调用日志打印时，会将消息传递给对应的sinks进行分流处理，可以1对多
2. 可以覆盖默认sinks，通过remove接口
3. 自定义level
4. 可以灵活定义sinks的格式化format形式，包括灵活定义每种字段的颜色
5. 支持异步记录，并且多线程安全，多进程安全需要开启enqueue参数为True
6. catch包装器，可以让函数抛出异常时，自动通过loguru日志打印重要信息
7. 支持sinks定义时，添加backtrace参数来支持exception方法打印，自动打印回溯栈和相关变量
8. 支持sinks定义时，添加serialize参数来支持，会将record字典所有内容，序列化为json，再附加上message，合并为一个json
9. 支持sinks定义时，添加filter参数来支持过滤
10. 支持库内集成，合理使用流程，库内默认disable，然后引用库的地方进行enable开启
11. 原生日志的兼容，可以将原生log句柄注入loguru使用，也可以将loguru句柄转化为原生handler使用，以及直接拦截原生log，转发给loguru sinks
12. 支持以环境变量的形式配置loguru格式
13. 提供解析器parse，来方面反向解析loguru日志格式，提取出字典
14. 支持sinks定义时，关联通知库，以邮件形式发送日志
15. 支持灵活配置日志文件的处理，按大小滚动，以及保留时长retention即时间到清空，还有压缩
16. sinks formatter 的内建字段

## 自定义sink模板

```c++
from loguru import logger
import sys

def gui_sink(message):
    # 1. 获取根据 format 渲染出的完整最终字符串 (例如："10:00:00 | INFO | 网络连接成功\n")
    full_text = str(message)
    
    # 2. 获取你最初传入的纯净业务字符串 (例如："网络连接成功")
    raw_payload = message.record["message"]
    
    # 3. 提取你要的“前缀”部分 (去掉尾部的原始信息和换行符)
    # 使用 rstrip 去掉 loguru 自动加在尾部的 \n
    prefix = full_text.rstrip('\n').replace(raw_payload, "")
    
    print(f"【提取到的前缀】: {prefix}")
    print(f"【纯净业务内容】: {raw_payload}")

# 配置 format，这决定了前缀的长相
logger.add(gui_sink, format="[{time:HH:mm:ss}] <{level}> ---> {message}")

# 触发日志
logger.info("TCP握手完成")
```

## schema parse

1. record、message 和 extra 的真实关系
record 就是一个超级大字典（Dictionary），它代表了“一条日志的完整生命周期数据”。

当你调用 logger.info("系统启动") 时，Loguru 在底层瞬间创建了一个 record 字典。它的内部结构大致如下：

结论：message 是 record 的第一层成员，extra 也是 record 的第一层成员，并且 extra 本身就是一个纯粹的 Python 字典。

1. 内建字段 vs extra 字段
是的，像时间（time）、行号（line）、模块名（module）、进程/线程ID（process/thread）这些，都是 内建（Built-in）字段。Loguru 会在底层自动帮你计算并填入 record 中。

除官方文档规定的内建字段外，任何你自己想加的数据（比如 ip, username, device_id），都必须装在 record["extra"] 这个抽屉里，不能直接塞到 record 的第一层。如果这些自定义数据是动态计算的，确实就需要用 patch。

1. extra 到底会不会出现在最终的日志里？
这取决于你如何配置 logger.add() 的两个参数：format 和 serialize。

情况 A：普通的纯文本输出（serialize=False）
extra 里的内容默认是隐形的。除非你在 format 字符串里显式地把它写出来（这就是你说的“作为格式化变量替换”）。

情况 B：JSON 结构化输出（serialize=True）
这时候 format 怎么写都不重要了。Loguru 会把整个超级大字典 record 直接 dump 成 JSON 文本。此时，extra 字典里的所有内容，无论你有没有主动声明，都会完整地输出在 JSON 字符串里，方便传给 Elasticsearch 这种日志分析引擎。

1. 为什么说 bind 是“死”的？每次重新 bind 不行吗？
你完全可以每次打印前重新 bind，比如：

官方文档之所以暗示它“死”（静态绑定），是指 bind() 会克隆并返回一个新的 logger 对象。它内部的 extra 字典就被固定写入了 {"user": "A"}。

这在处理网络请求时很有用：

如果你不用 bind，你就得在每次调用时写 logger.info(f"[{client_ip}] 解析包头")，这就是 bind 解决的痛点。

1. patch 到底在干什么？（解析那个难懂的 Lambda）
patch 不是给 extra 的字段注入回调函数。
patch 的本质是：给 Logger 注册一个拦截器（钩子函数）。每次有一条新日志产生，在它被打印出来之前，先把 record 这个大字典交给你拦截处理一下。

如果你觉得官方的 lambda （匿名函数）难懂，我们把它翻译成标准的 def 函数，逻辑就清晰了：

输出：

官方那句 logger.patch(lambda record: record["extra"].update(utc=datetime.utcnow()))，其实就是用 lambda 把上面这种拦截器函数的定义压缩成了一行。update() 是 Python 字典的标准方法，用于向字典中添加键值对。
