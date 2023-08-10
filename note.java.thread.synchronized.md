---
id: hzjntn2tq48fnccswspl9it
title: Synchronized
desc: ''
updated: 1691575146418
created: 1691549448157
---

## synchronized
-------------
#### forward
----------
- **在 `JVM` 层面提供的同步原语，底层依旧基于操作系统的 `mutex API`**
- **理解基准，应该将 `synchronized` 操作效果转义为 `mutex API` 操作**
    1. 锁在哪?
    2. 锁的作用域/范围？
    3. 锁的生命周期?
- 内存可见性 `memory visibility`
    > 该词汇描述的是一种正确的内存状态变化，在多线程多cpu多核心的系统环境下，在不同线程会有自己的本地缓冲，那么对于在线程间共享的对象内存，如果在某一线程中被修改，并未立即同步其状态在其他线程中，称为是一种内存的可见性错误，而在编程语言中一定机制的加持下，如使用互斥锁/synchronized等，可保证目标共享对象保持内存可见性
- java 对象头 `object header`，jvm 为每一个对象生成的存储一些关键信息的数据结构
  > record obj run time info for jvm operating `mark word`
- 锁类型 和 锁升级: 在使用 synchronized 锁功能时，根据不同情况来进行锁方案切换，从轻量方案向低效方案切换的过程
  > 为提高效率，即在状态达到万不得已时，才使用效率最低的机制
- `synchronized` 本身就是一个基于锁机制，提供锁功能的触发关键字，并非`锁本身`
  > 所以在理解 synchronized 效果时，应该进行锁机制的转义


#### reference
-------
- [Java Synchronized 理解](https://zhuanlan.zhihu.com/p/339841321)
- [关键字: synchronized详解](https://pdai.tech/md/java/thread/java-thread-x-key-synchronized.html)
- [synchronized-知乎](https://www.zhihu.com/question/57794716/answer/606126905)
- [廖雪峰-java](https://www.liaoxuefeng.com/wiki/1252599548343744/1306581033549858)
  
#### feature
--------------
1.**锁实体**
锁实体决定了哪些代码块是属于关联的临界区，synchronized 使用的锁实体有两个来源: 一个是每一个实例化的对象，一个是每一个class类
当使用synchronized关键字未指明锁来源时，默认使用当前对象自身的互斥锁，如果函数是静态的则切换为类锁
- 对象锁
  > 每一个实体对象自带的**唯一**互斥锁
  > 等同于对象内部包含一个锁对象
- 类锁
  > 每个class 自带的**唯一**互斥锁
  > 等同于类定义中包含一个静态锁对象，一个类只有一个
- 自定义锁
  > 即在表达式上，通过引用其他对象来引用其包含的互斥锁

2. **代码块进入则获得并加锁，出代码块则解锁**
3. **可重入性即 `recursive lock`**
#### usage and expression
--------------
- 强指定 对象锁-使用具体对象名标识
  ```java
    Object block1 = new Object();
    Object block2 = new Object();
    public void run(){
        synchronized (block1){

        }
        synchronized (block2){

        }
    }
  ```
- 强指定 类锁-使用具体类名标识
  ```java
    static SynchronizedObjectLock instance1 = new SynchronizedObjectLock();
    static SynchronizedObjectLock instance2 = new SynchronizedObjectLock();
    public void run() {
        synchronized(SynchronizedObjectLock.class){
  
        }
    }
  ```
- 默认锁 修饰非静态方法-在函数返回值前-使用当前对象锁
   ```java
    public synchronized void method() {
      
        }
   ```
- 默认锁 修饰静态方法-在函数返回值前-使用当前类锁
  ```java
    public static synchronized void method() {
            System.out.println("我是线程" + Thread.currentThread().getName());
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "结束");
        }
  ```
#### with condition
------------
> 能否直接应用 synchronized 替代 c++ 中 lock 来配合 condition 的使用?
> synchronized 对象本身已经提供了wait() notify() 方法，在进入synchronized修饰的代码块时，就能够使用上述函数进行同步
> 而condition则依附于lock对象，从lock对象获取

|||
|-----|----|
|synchronized+wait+notify|ReentrantLock+Condition|

##### usage

#### depth
-----
-  synchronized 的实现机制
-  synchronized 的使用思路与规范


