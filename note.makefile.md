---
id: 3i0sqlecncb2l3g3uguqvq6
title: Makefile
desc: ''
updated: 1735698218042
created: 1735649856405
---

### refe
- [知乎: 手把手教你写一个 Makefile 文件](https://zhuanlan.zhihu.com/p/583565789)
- [blog: 和我一起写makefile title](https://wiki.ubuntu.org.cn/%E8%B7%9F%E6%88%91%E4%B8%80%E8%B5%B7%E5%86%99Makefile:%E6%A6%82%E8%BF%B0)
- [blog: 和我一起写makefile](https://www.cnblogs.com/chien/p/17328638.html)
- [blog: 和我一起写makefile list](http://bbs.chinaunix.net/thread-408225-1-1.html)

### gcc 编译命令

### makefile的基本原理
make 可执行文件 解析 + makefile格式的描述文件

#### 语法1: 定义target
```bash
<target>: <prerequisite>
    <command>

```

    makefile通过 target的定义来显示描述依赖关系, target可以是一个目标结果文件如*.o的文件，也可以是一个逻辑变量，来作为一个prerequisite变量被其他target所引用，当然在顶层的终端命令行调用，也可以通过 `make <target>` 的方式，来触发具体target的构建
#### 语法2: make变量
**变量类型**
1. 环境变量
2. make命令行参数变量 `user$ make all var1=xx var2=xx`
3. makefile 变量 
4. makefile 目标变量 `<target_name>: <var_name> <operat> <value>`
5. 多行变量 define + endef 

**变量定义和赋值方式**
1. = 延迟赋值 只有在变量使用时才去解析目标变量
2. := 立即赋值 立刻赋值 在定义的时候就解析引用的目标变量，赋值给当前的变量
3. ?= 条件赋值 条件指的是变量为未定义变量，符合条件则获得默认值
4. += 追加赋值 
#### 语法2: make函数

#### 语法3: 引用
- `$@` 
#### .PHONY 伪目标


#### 疑问
1. 一个makefile中会定义很多个target，那么make的处理流程是怎么样的？
2. make是如何处理重复编译问题的，即如何判断makefile中哪些target需要重新构建，哪些不需要?
3. 对于命名为具体.o文件的target，make可以通过其文件时间戳属性来判断是否执行构建？但对于命名为逻辑变量的target呢? 判断依据又是什么？command的内容是否会被make进行解析判断？
4. 对于 定义target的prerequisite时，在prerequisite中引用target自身定义的变量不生效，只能在target的command中引用？
5. 在定义target时，都是直接定义名称吗，会不会包含路径？