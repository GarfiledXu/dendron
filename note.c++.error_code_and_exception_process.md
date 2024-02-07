---
id: t7rtuwvp2mwtvcwbyi6311x
title: Error_code_and_exception_process
desc: ''
updated: 1707298303879
created: 1700200739897
---


##### 方案分析

#### 现有方案的缺点
##### 异常方案


##### 返回值方案
缺点:
操作频繁，容易漏检
值与错误情况形成映射，由于很难/无法统一 在所有调用栈中使用   全局的唯一的错误值，并且只能以追加的方式扩展错误集合
所以通常只能做到，一个错误集只归属于具体函数，同一个错误值在不同函数中的错误意义不同，要全面检查错误，就需要每一个函数都进行错误值检查，值含义只归属对应函数，并且在当前layer，需要对所有调用的函数返回值进行归纳重新映射，返回给下一层
这就意味着如果通过log排查错误，是无法通过 返回值获知 错误状况的，必须 对应函数 + 返回值 + 函数内错误值映射关系

最内层的错误，传递到外层，值层层转义，在最外层只能知道发生了错误
无法获取到最内层的引发错误的value和message

优点:
按流程，进行值判断 操作简单  容易判断目标函数的具体错误

##### 异常机制
优点: 可以抛出，在外围一次性捕获所有，错误的模糊处理，属于小麻烦解决大麻烦，但是 do while()可以进行模拟

缺点: 
无法从函数签名上获知一个函数是否会抛出异常? 具体抛出什么异常? 是否会用什么错误值?
即 捕获操作可能是无效的，错误返回可能通过错误码，单靠异常机制并不能百分百捕获一个接口可能发生的错误
异常处理的流程繁琐，代码结构破坏大
###### c++的异常实现
[C++ 异常机制的实现方式和开销分析](https://baiy.cn/doc/cpp/inside_exception.htm)
[C++ 异常是如何实现的](https://zhuanlan.zhihu.com/p/406894769)
[C++ exceptions under the hood](https://monkeywritescode.blogspot.com/p/c-exceptions-under-hood.html)
[函数调用栈](https://zhuanlan.zhihu.com/p/103454656)
[函数调用栈之彻底理解](https://www.yigegongjiang.com/2022/%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8%E6%A0%88%E4%B9%8B%E5%BD%BB%E5%BA%95%E7%90%86%E8%A7%A3/)
[异常处理与MiniDump详解(1) C++异常](https://www.jtianling.com/%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86%E4%B8%8Eminidump%E8%AF%A6%E8%A7%A3-1-c-%E5%BC%82%E5%B8%B8.html)
1. 源码中异常相关的编写和编译处理
   1. throw
   2. try catch
2. 编译器插入的数据结构和函数
   ```c++
        strcut EXP{
            EXP* pre;
            
            int nstep;
        };
   ```
   1. 一个异常专用链表，用于遍历匹配处理
   2. 每一个函数对应一个try表，记录存在try块的函数内try块相关的函数调用语句的位置，
   3. 每一个函数的上层函数(上层栈帧)是唯一的，也意味着每一个函数栈帧只需要用一个变量存储 在上一层栈中的位置标记，nstep，那么在栈增长过程中，当前栈帧的nstep是怎么被赋值的？直接在语句执行前，插入一段赋值语句
   6. 每一个函数对应一个unwind表，记录函数内应该顺序执行的析构操作以及对应的nstep + 具体对象指针(析构函数是编译器自动生成的)，配合nstep标志，来确定发生异常时应该执行表中的析构范围
   7. 栈增长(函数嵌套调用)的记录，记录当前栈帧时，给每一个存在try块的函数内的存在函数调用的语句进行递增编号，就可以确定try块的编号范围，这样，当有异常发生时，栈进行回退，只要获知当前栈在上一个栈的try相关编号值
        
3. 栈回退(stack unwind):
   1. 栈回退 不等于 栈回溯，栈回退是一个具体机制，是伴随着c++ 异常机制而生的机制，用于资源清理和析构释放
   2. 数据结构: unwind table + nstep
   3. 过程: ...

##### 方案1
 抽象一个错误码类型
 本质上是一个析构时抛出异常的类，构造后 默认 析构时 value 非0 则自动抛出异常，再按异常方式进行处理, 配合一个标志位决定是否 enable/disable 这一行为
 > 从原理上，析构抛出异常是可行的，但在通用的编程原则上禁止，在这里巧用了这一原理，在特殊类中进行异常抛出
 如何更换方式处理错误(绝对不会抛出异常，但确认错误被处理了):
  1. 通过触发类型转换，一方面转换出的错误码用于错误判断，另一方面间接使得析构抛出异常 强制disable  总结: 触发类型转化-> disable 析构异常
    1. 隐式转化
        erc = > int `通过 单参数(多参数，除头参数外指定默认值)的erc构造函数 隐式转化实现`
        ```c++
            erc(int var){}
            erc(int var, int c = 10, int b = 5){}
            int bb = 0;
            erc back_convert_erc = 10;
            erc back_convert_erc2 = bb; 
        ```

        int => erc `通过 int() 成员函数实现`
        ```c++
            #include <iostream>
            class erc{
            public:
                erc() = default;
                ~erc() {
                    printf("enter destructor\n");
                };
                erc(int var, int c = 10, int b = 10) {
                    printf("enter one argument construct\n");
                }
                operator int() {
                printf("enter int() convert\n");
                return 100;  
                };
            };
            //erc to int
            //int to erc
            int main()
            {
                erc cur_erc{};
                //temporary erc assigned to int
                int a = erc();
                //variable erc assigned to int
                int b = cur_erc;
                
                //temporary/const int assigned to erc
                erc cur_erc2 = 10;
                int bb = 0; erc back_convert_erc2 = bb; 
                
                // erc cur_erc_4{};
                // erc cur_erc_5{};
                // ( cur_erc_4 );//no trigger
                // ( (cur_erc_4) ||  (cur_erc_5) );
                return 0;
            }
        ```
        [c++ 隐式类型转换](https://www.cnblogs.com/zhuyf87/archive/2013/02/01/2888832.html)
        算术表达式
        逻辑表达式
        初始化和赋值操作
        ```c++
            int ival = 3.14 // 3.14 converted to int
            int *ip;
            ip = 0; // the int 0 converted to a null pointer of type int *
        ```
        条件表达式 => bool
        ```c++
            if (ival)
            while (cin)
            //括号中的都属于条件表达式，而条件表达式会被转化为bool值
        ```
        条件操作符（? :）中的第一个操作数，逻辑非（!）、逻辑与（&&）、逻辑或（||）
        这些逻辑运算符的操作数都是条件表达式。
        if、while、do while、以及for的第2个表达式都是条件表达式。
        ###### 逻辑运算符 对条件表达式的执行顺序?
        1. && 看做一个do{}while(false) 从左向右，执行条件表达式，只要有一个条件表达式为假，则break, 返回false，相反全部true，则最后返回true，执行过程会将条件表达式的类型值转化为 bool值
        2. || 看作是一个do{}while(false) 从左向右，执行每一个条件表达式，存在一个值为true，则break，相反，全部为false，则最后返回false
        3. ?: 条件操作符 的第一个数为条件表达式
        4. 逻辑非 (!) 的第一个数为条件表达式
        ###### int 和 bool的转化
        0 对应 false， 非0 全部对应true        
        ###### erc 与 条件表达式的结合使用
        ```c++
        
        ```
    
    2. 显式强转
  2. 在合理情景下，调用接口 进行 enable/disable 析构异常
  3. c++ throw 是可以抛出自定义类型的

 在函数签名上就能判断 对应的错误处理方案
 通过类型转换运算符，来实现 复杂错误码对象 直接返回 标量错误值 返回, 能否return标量直接转复杂错误对象返回
 隐式转换:

    1. return type = erc， return erc variable
    2. return type = erc,   return erc temporary
    3. return type = erc， return int
    4. return type = int， return erc variable
    5. return type = int， return erc temporary
    6. receive type = int，assigned temporary erc
    7. bracket and condition expression, assigned variable erc 
    8. bracket and condition expression, assigned temporary erc 

    9. copy assigned => yes
    10. move assigned => yes
    11. copy construct no
    12. move construct no
 通过内置标志位 与  类型转换操作符 以及 析构交互，来实现 函数错误未被处理的检查判断
 通过以上机制，实现了 同时兼容两种函数的错误处理，可以二选一进行，并且不会遗漏
 能否返回完整的错误信息，在错误抛出点整理好错误message? 错误对象的传递和链接? 更精细的异常行为控制和定义，而不是直接强行抛出?
 在这里继承能做什么事情? 
 通过额外的继承，附带额外的核心错误数据，参与最终message生成

##### 结构
1. facility 负责 提供自身类型的默认对象  提供 message生成 并且在最后rasie(log + throw)的功能
2. erc 在构造时 创建对应的facility对象负责部分职能

##### 方案1 改造
1. log 模块支持自定义，而不是简单printf
2. 支持错误被处理时，自定义回调处理
3. 能否支持回溯栈输出
4. spdlog 中的 backtrace 功能是什么？
    >  store the debug message to ring buffer, and post list message when error haapened and to trigger dump backtrace
5. c++ 的 异常实现 ？

##### c++ 类型转换运算符
1. 类的非静态成员函数
2. 定义时指定 转换的目标类型，也就意味着，可以通过当前类型转换运算的内容，将一个当前所在类型的对象转换为转换运算符的目标类型
3. 哪些操作触发 类型转换运算符的操作?
   1. 隐式转换 
      1. = 赋值操作符号
        ```c++
            int a = erc_obj;
        ```
      2. 对象处于逻辑表达式中
        ```c++
             (result = open_socket ())
            || (result = resolve_host ())
            || (result = connect ())
            || (result = send_data ())
            || (result = receive_data ());
        ```
   2. 显式转换 : static_cast<>

##### 当前项目需求
1. 兼容 返回值和异常的错误捕获手段, 或者封装捕获宏，erc就按原思路照常捕获，而std则重复一次捕获操作
2. 混合处理，用错误码方式进行错误判断，最后一次性抛出
    ```c++
    // 函数声明，返回值使用erc
    erc open_socket ();
    erc resolve_host ();
    erc connect ();
    erc send_data ();
    erc receive_data ();
    erc close_socket ();

    erc get_data_from_server(HostName host)
    {
    erc result;

    (result = open_socket ())
    || (result = resolve_host ())
    || (result = connect ())
    || (result = send_data ())
    || (result = receive_data ());

    close_socket ();      // 清理
    result.reactivate ();
    return result;
    }
    //那么这里的result 返回有什么意义? 不是相当于拷贝了一份到外面吗？
    ```
3. 理想状态：按错误码的思路进行错误返回，按异常的方式进行错误判断
4. 在错误发生时，异常对象抛出并传递累计回溯栈消息
5. 按原方案思路，erc是独立类型，通过异常捕获也只能捕获该独立类型，我需要的是兼容std的异常处理，在捕获到时，判断若是erc则特殊处理
**在这里可以看出，继承这一机制，实现了类型兼容，也就意味这操作复用（异常的处理操作）**

#### refe
[C++编码规范与指导](https://baiy.cn/doc/cpp/index.htm#%E4%BB%A3%E7%A0%81%E9%A3%8E%E6%A0%BC%E4%B8%8E%E7%89%88%E5%BC%8F_%E5%BC%82%E5%B8%B8)
[baiyang pdf](https://baiy.cn/doc/asp_whitepaper.pdf)
[白杨的原创免费作品](https://baiy.cn/)
[microsoft: 现代 C++ 处理异常和错误的最佳做法](https://learn.microsoft.com/zh-cn/cpp/cpp/errors-and-exception-handling-modern-cpp?view=msvc-170)
[错误处理](https://github.com/Cpp-Club/Cxx_HOPL4_zh/blob/main/07.md)

[方案1](https://www.cnblogs.com/qinwanlin/p/12669347.html)
[方案1 end](https://www.cnblogs.com/qinwanlin/p/12681178.html)
[The sad history of the C++ throw(…) exception specifier](https://devblogs.microsoft.com/oldnewthing/20180928-00/?p=99855)
[RTTI、虚函数和虚基类的实现方式、开销分析及使用指导](https://baiy.cn/doc/cpp/inside_rtti.htm)