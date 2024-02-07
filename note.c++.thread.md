---
id: kje6c2cm5uo5d8mhpyllgg6
title: Thre
desc: ''
updated: 1706518523553
created: 1706517634441
---

#### threadlocal mechanism 与 线程相关对象的关联
通常使用threadlocal 机制，来实现一个通用的静态函数，在每一个线程使用，创建出与只与当前线程相关的对象
在此机制的基础上，就能实现两个关联对象的无关联使用
```c++
 1 #include "base/logging.h"
 2 #include "base/message_loop/message_loop.h"
 3 #include "base/message_loop/message_loop_current.h"
 4 #include "base/task/post_task.h"
 5 #include "base/task/single_thread_task_executor.h"
 6 #include "base/task/thread_pool/thread_pool_impl.h"
 7 #include "base/task/thread_pool/thread_pool_instance.h"
 8 #include "base/threading/thread_task_runner_handle.h"
 9 #include "base/timer/timer.h"
10 
11 void Hello() {
12   LOG(INFO) << "hello,demo!";
13 }
14 
15 int main(int argc, char** argv) {
16   // 创建消息循环
17   base::MessageLoop message_loop;
18   // 也可以使用下面的方法。它们的区别仅在于 MessageLoop 对外暴露了更多的内部接口。
19   // 在当前线程创建一个可执行 task 的环境，同样需要使用 RunLoop 启动
20   // base::SingleThreadTaskExecutor main_task_executer;
21 
22   base::RunLoop run_loop;
23 
24   // 使用 message_loop 对象直接创建任务
25   message_loop.task_runner()->PostTask(FROM_HERE, base::BindOnce(&Hello));
26   // 获取当前线程的 task runner
27   base::ThreadTaskRunnerHandle::Get()->PostTask(FROM_HERE,
28                                                 base::BindOnce(&Hello));
29 
30   // 启动消息循环，即使没有任务也会阻塞程序运行。当前进程中只有一个线程。
31   run_loop.Run();
32 
33   return 0;
34 }
```

#### 多线程运行模型
