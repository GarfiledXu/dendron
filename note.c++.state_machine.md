---
id: y96wzbc8btfca6dejeobsk3
title: State_machine
desc: ''
updated: 1705912684349
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
[zhihu: 嵌软开发思维：状态机的三种实现方法](https://zhuanlan.zhihu.com/p/645672655?utm_id=0)
[blog: 干掉项目中杂乱的 if-else，试试状态模式，这才是优雅的实现方式](https://www.shangyexinzhi.com/article/4530925.html)
[zhihu: 下推自动机（Pushdown Automata）](https://zhuanlan.zhihu.com/p/575664217?utm_id=0)
[blog: c语言实现简单状态机](https://www.cnblogs.com/lifeislife/p/17034602.html)

#### what machine doing?
以context中抽象的transition作为绑定外界的入口，生成从每一个transition的实现以及owner引用调用，以及内部状态转变设置
如此，用户只需要: 表述好状态流程(抽象出状态名称，以及状态流程中进行的外部函数引用名称)，将外界s
(在实现层面, 拥有所有transition virtual搜集在一起作为一个超集 以及基类，每一个状态对象继承这个超集，只重写有关联的transition，transition的内部实现即设置状态的变化), 静态实例化所有种类的状态对象放置在map中用于后续引用，并且在上下文中内置一个状态对象的指针，用于指向当前状态对象, 再定义一个context对象，1.封装了对这个状态指针的操作 2.包含外部appclass对象的引用 即owner，why？在定义状态流程时，默认规则 在状态流程中引用的条件表达式 ctxt.condition() 以及状态转化结束后执行的action 来源是owner，即 appclass
user 应用实现的流程: 1.封装并实现所有在流程中的owner 函数引用 2.实例化一个smc的context并传入自身引用 3. 将外部的响应输入 接入 绑定 smc context 抽象的 transition 操作
适合 是基于状态的异步回调响应 逻辑

#### 关于transition的argument
由于transition是作为外部输入绑定的入口，即每次整个状态转换链的起始，在整条链路中需要用到参数的owner引用则通过transition来获取，表示此时调用transition传入的一些值
注意 transition 传入参数，在 condition guard 处的ctxt调用以及enter state的action 处的 ctxt调用都可以引用

#### 关于smc的exit 和 entry
在.sm状态流程描述中，在一个转化前的状态(起始状态)，进入transition括号前，添加Exit和Entry的action，
随后打开生成的_sm.cpp文件，会发现首先Exit 和Entry是作为了类似transition的虚函数在state基类中(默认空实现)，所有状态对象的transition函数实现都会进行以下流程:Exit-setstate-Entry, 而对应流程表中真正添加了Exit 和 Entry的状态对象则会复写Exit Entry,那么 这里的Exit和Entry是什么含义?
> 首先transition操作代表的是将一个状态转变为另一个状态(一个状态值设置为另一个状态值), 那么这个过程就是 初始状态 消亡，到结尾状态 进入的过程，即这里smc的起始状态Exit，结尾状态Entry

> 用官方 manual 的表述，将transition也看作一个实体，具体 transition 在 transition 操作之前 leave 起始状态 "from state"，在transition操作之后，enter 一个状态 "to state"，即exit entry分别指向的是transition过程的头尾状态的退出和进入

##### about push transition and pop transition of smc
从官网的offical manual中很难理解push的语法运作流程，
只能从生成的代码中对比出语法作用
> pushState 和 popState是smc中context部分的自实现函数
栈状态和存储结构:
首先一个context包含了一个state指针指向当前状态，以及StateEntry(即状态stack)类型的state_stack(链表结构的stack)，以及state_previous(每次transition操作时记录transition前的状态),每一次进行pushState操作会将当前是state指针入栈，更新包含的state_stack，更新 state_previous 记录入栈的state. popState(transition_name) 则会先清空状态，执行末尾对应的action，然后抛出状态用于赋值给当前状态，最后执行一次pop附带的transition，对pop出的状态二次转化，也意味着会有尾随action
```c++
//在这里push前面多出一个设置状态位，按执行流程分析必定会被后面push传参的state覆盖
//那么目的主要是想要触发对应的 entry action
//总结流程： 执行leave state的exit操作，清空当前状态(clearState)，执行末尾action， 设置 enter state1 => 执行enter state1的entry action => 存储当前状态进栈 => 设置当前状态位state2=> 执行state2的 entry action
//注意 由于在 enter state处有两个状态，那么整个操作末尾的action 跟随的state 合理性 就有待讨论了, 在smc中，是直接让action 跟随在 两个enter state之前，clear state之后
pushState1: 
    <leave state> 
    {
        <transition> <current enter state 1>/<push(current enter state 2)> {action}
    }
//总结流程:
// 问题来了， 只有一个enter state的push，整行末尾的action是在enter state前执行还是enter state后执行?
//问题不够准确，由于最终状态必然是enter state，所以这里action的执行顺序问题关键应该为: 是在enter state 设置后的entry操作之后还是其之前? 
//由代码生成结果来看，还是在entry操作之前，那么其实可以统一看作，所有的action都是在push/pop 以及他们的entry 操作之前的
pushState2:
    <leave state>
    {
        <transition> <push(current enter state1)> {action}
    }
////总结流程:
//执行leavel state 的exit操作， 并且clearState(), 执行pop之后的action操作 => 将堆栈中的state抛出，并且设置状态指针为抛出的stat=>执行pop的transition转换操作
//这里有个关键点: 包含pop的转换是先执行末尾action然后再进行pop状态，并执行pop的transition，pop的transition后续会有一系列的操作, 也就意味着有后续状态，这里action的跟随与前面带有两个enter state的push相同，也是在clearState之后执行
popState1:
    <leave state>
    {
        <first transition> <pop(second transition of after set state)> {action}
    }
```




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
3. 最后以查表的方式进行逻辑确认和问题排查

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