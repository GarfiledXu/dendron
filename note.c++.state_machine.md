---
id: y96wzbc8btfca6dejeobsk3
title: State_machine
desc: ''
updated: 1705825340809
created: 1705540749952
---
#### why?
**reference the passage from offical of smc: Your application lives in a world of asynchronous, unordered events: mouse clicks, timeouts, messages, and OS signals.**
#### refe
[offical: smc](https://smc.sourceforge.net/)
[offical: smc using](https://smc.sourceforge.net/SmcManSec3.htm#C++)
[github smc example](https://github.com/MRkuan/SMC_Study/tree/master)
[smc tutorial ppt1](https://smc.sourceforge.net/slides/smc.pdf)
[smc tutorial ppt2: very nice](https://smc.sourceforge.net/slides/SMC_Tutorial.pdf)
[smc source](https://sourceforge.net/projects/smc/files/SMC%207.6.0/)
[smc maunal](https://smc.sourceforge.net/SmcManual.htm)
[smc syntax variable set](https://smc.sourceforge.net/SmcManual.htm)

#### what machine doing?
以context中抽象的transition作为绑定外界的入口，生成从每一个transition的实现以及owner引用调用，以及内部状态转变设置
如此，用户只需要: 表述好状态流程(抽象出状态名称，以及状态流程中进行的外部函数引用名称)，将外界s
(在实现层面, 拥有所有transition virtual搜集在一起作为一个超集 以及基类，每一个状态对象继承这个超集，只重写有关联的transition，transition的内部实现即设置状态的变化), 静态实例化所有种类的状态对象放置在map中用于后续引用，并且在上下文中内置一个状态对象的指针，用于指向当前状态对象, 再定义一个context对象，1.封装了对这个状态指针的操作 2.包含外部appclass对象的引用 即owner，why？在定义状态流程时，默认规则 在状态流程中引用的条件表达式 ctxt.condition() 以及状态转化结束后执行的action 来源是owner，即 appclass
user 应用实现的流程: 1.封装并实现所有在流程中的owner 函数引用 2.实例化一个smc的context并传入自身引用 3. 将外部的响应输入 接入 绑定 smc context 抽象的 transition 操作
适合 是基于状态的异步回调响应 逻辑

#### 关于transition的argument
由于transition是作为外部输入绑定的入口，即每次整个状态转换链的起始，在整条链路中需要用到参数的owner引用则通过transition来获取，表示此时调用transition传入的一些值

#### 关于smc的exit 和 entry
在.sm状态流程描述中，在一个转化前的状态(起始状态)，进入transition括号前，添加Exit和Entry的action，
随后打开生成的_sm.cpp文件，会发现首先Exit 和Entry是作为了类似transition的虚函数在state基类中(默认空实现)，所有状态对象的transition函数实现都会进行以下流程:Exit-setstate-Entry, 而对应流程表中真正添加了Exit 和 Entry的状态对象则会复写Exit Entry,那么 这里的Exit和Entry是什么含义?
> 首先transition操作代表的是将一个状态转变为另一个状态(一个状态值设置为另一个状态值), 那么这个过程就是 初始状态 消亡，到结尾状态 进入的过程，即这里smc的起始状态Exit，结尾状态Entry
> 用官方 manual 的表述，将transition也看作一个实体，具体 transition 在 transition 操作之前 leave 起始状态 "from state"，在transition操作之后，enter 一个状态 "to state"，即exit entry分别指向的是transition过程的头尾状态的退出和进入

##### about push transition and pop transition of smc
从官网的offical maunal中无法理解push的运作流程，
只能从生成的代码中寻迹


#### what is ocp ?
0. Open-Closed Principle
1. open for extension
2. close for modification
3. extending the system requires modifying existing code
4. the state pattern respects the OCP: soft entities should be open for extension but closed for modification

#### state pattern issue
1. The state logic is distributed across a
number of classes, and so there’s no single
place to see it all
> 同样也是静态注册需思考的问题，所有对象之间是间隔的，没有一个统一的地方能够 overview 所有 对象的 关联
2. Tedious to write


#### what smc could do?
1. 用户可以集中编写描述文档，弥补 state pattern 的 distribute across 的缺点
2. 映射类由 smc 自动生成，弥补了 tedious 问题

#### concept


#### using
    状态机在计算机科学和软件工程中有广泛的应用。下面是一些常见的状态机应用场景：

    自动机和编译器：状态机被广泛用于编译器设计和语法分析中。例如，正则表达式引擎和词法分析器使用状态机来解析和匹配输入文本。

    通信协议：状态机常用于建模和实现通信协议。状态机可以描述协议中的各种状态和状态转换，从而实现有效的通信和协议约束。

    游戏开发：状态机可用于游戏中的角色行为、游戏关卡和游戏状态管理。通过状态机，可以实现角色状态转换、敌人行为决策和游戏流程控制。

    用户界面：状态机可以用于用户界面的控制和交互。例如，按钮、菜单和对话框等用户界面元素可以使用状态机来管理其状态和响应用户的操作。

    控制系统：状态机广泛应用于实时控制系统，如机器人控制、工业自动化和交通信号控制。状态机可以帮助实现复杂的控制逻辑和状态转换。

    故障检测和恢复：状态机可用于故障检测和系统恢复。通过定义系统的各种状态和故障处理策略，可以实现故障自动检测、错误处理和系统恢复。

    协议解析和数据处理：状态机可以用于解析和处理各种协议和数据格式，如JSON解析器、XML解析器和网络协议解析器等。

    电路设计和硬件验证：在硬件设计中，状态机被用于描述和验证硬件电路的行为和状态转换，以及验证电路的正确性和时序性。