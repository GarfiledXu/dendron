---
id: 6hs9gi58ug13nxkarh5riuq
title: Spdlog2
desc: ''
updated: 1695893258595
created: 1695620465455
---

```c++
namespace spdlog {
// Async overflow policy - block by default.
enum class async_overflow_policy
{
    block,         // Block until message can be enqueued
    overrun_oldest // Discard oldest message in the queue if full when trying to
                   // add new item.
};
namespace details {
class thread_pool;
}
class SPDLOG_API async_logger final : public std::enable_shared_from_this<async_logger>, public logger
{
    friend class details::thread_pool;

public:
    template<typename It>
    async_logger(std::string logger_name, It begin, It end, std::weak_ptr<details::thread_pool> tp,
        async_overflow_policy overflow_policy = async_overflow_policy::block)
        : logger(std::move(logger_name), begin, end)
        , thread_pool_(std::move(tp))
        , overflow_policy_(overflow_policy)
    {}

    async_logger(std::string logger_name, sinks_init_list sinks_list, std::weak_ptr<details::thread_pool> tp,
        async_overflow_policy overflow_policy = async_overflow_policy::block);

    async_logger(std::string logger_name, sink_ptr single_sink, std::weak_ptr<details::thread_pool> tp,
        async_overflow_policy overflow_policy = async_overflow_policy::block);

    std::shared_ptr<logger> clone(std::string new_name) override;

protected:
    void sink_it_(const details::log_msg &msg) override;
    void flush_() override;
    void backend_sink_it_(const details::log_msg &incoming_log_msg);
    void backend_flush_();

private:
    std::weak_ptr<details::thread_pool> thread_pool_;
    async_overflow_policy overflow_policy_;
};
} // namespace spdlog
```
#### dynamic memory manager
- [cpp.reference.dynamic memeroy manager](https://en.cppreference.com/w/cpp/memory)
- sp 针对堆内存 低配的作用域GC机制
#### dangling pointer and wild pointer
- dangling pointer: 当指针指向对象被销毁时，指针仍然指向已回收地址
- wild pointer: 指针未经过初始化赋值
#### dangling pointer and share_ptr
- 问题场景: 通常构造share_ptr是在目标对象外的行为，即首先假定对象外的调用存在一个share_ptr的构造，即对象外围已经拥有一个计数为1的指针，那么当对象成员函数需要一个包装this指针的share_ptr时，并且是通过this指针进行构造的share_ptr，那么此时，存在了两个count为1，包装同一个指针的shared_ptr，那么当正常析构时，this的析构将会触发两次。总结: 在成员函数内生成包装this的shared_ptr指针是一个错误行为，大概率会变成对share_ptr的克隆，最后包含了悬空指针，触发二次析构。
- 如何规避问题: 
  1. 当对象的成员函数，需要用到当前对象的share_ptr时，通过传参外部share_ptr到内部进行赋值。
   但如此，就多出了一个强制性行为，即需要外部实例化一个sp，并且传入
  2. public inherit enable_shared_from_this, using shared_from_this return a sp which shares ownership with *this
  3. return shared_ptr from this 的前提是当前对象是堆内存管理的
```c++
//inner myclass
myclass::process_this(){
    auto sha2 = std::shared_ptr<myclass>(this);
    //sha2 transfer or process
}
//main.cpp
auto sha1 = std::shared_ptr<myclass>();
//in main or other function
sha1->process_this();
//on destructor
sha2->~();
sha1->~();
```
#### best specification
- control constroctor behavior, instead of factory function
- by uppon way, there's no way to have getptr return nullptr.
  ```c++
  [[nodiscard]] static std::shared_ptr<Best> create()
    {
        // Not using std::make_shared<Best> because the c'tor is private.
        return std::shared_ptr<Best>(new Best());
    }
  ```
#### circular reference, weak_ptr and shared_ptr
- [cpp.reference](https://en.cppreference.com/w/cpp/memory/weak_ptr)
- 智能指针本质上是(智能指针对象)析构行为驱动(引用对象)析构，更具体点是(智能指针对象)析构改变计数，计数决定(引用对象)析构
- 智能指针-行为-->引用计数-->引用对象-析构
- 还是(智能指针对象)行为(离开作用域，构造，赋值，拷贝)驱动(引用对象)行为(析构)
- 循环引用，现象仅仅是只触发了智能指针的析构一次，计数减1，触发条件，两个类型的对象，双方都有持有对方类型的share_ptr成员，对象外，这两个对象又由share_ptr托管，那么此时实际上存在四个share_ptr, a_sp 和 b_sp 引用计数都为2，与死锁类似，发生流程：a_sp 构造=1, b_sp 构造=1, a_sp.b 赋值=2, b_sp.a 赋值=2, 作用域结束，a_sp 对象本身析构一次=1, b_sp 对象本身析构一次=1, 此时期望的代码逻辑是 外部a_sp-1=1,b_sp-1=1, a_sp.b-1=0, b_sp.a-1=0,资源正常释放。而锁死位置即是：a的析构-->需要a_sp计数为0-->需要外部a_sp离开作用域+b内部被持有的a_sp被析构这两种行为-->而内部a_sp的析构需要 b的析构-->需要b_sp计数为0-->需要外部b_sp离开作用域+a内部持有的b_sp被析构两种行为-->而内部b_sp的析构 需要a的析构
- 最后形成闭环，总结: 由于对象持有特殊智能指针成员，这一行为导致，将智能指针本身的作用域析构与某对象`析构关联`，双方对象的 `析构互相依赖`(各自智能指针计数的归零依赖于对方引用对象的析构)，互相等待依赖
- 观察：对于智能指针，各种智能指针持有对象，内部持有智能指针的行为，往往会带来智能指针问题
  > 何为内部持有智能指针的行为: 对象的内部定义关联的智能指针成员、对象的成员函数构造包装this的智能指针等行为
#### 何为不正常行为，正常行为
- 何为正常行为？正常计数？在预期的调用行为下，计数的变动能够最终驱动资源正常的析构释放，包括使用相关语法特性，编程规则，来调配智能指针的引用计数。
- share_ptr的计数变动规则是有限固定的，必然在一些代码场景下，预期的-和+受到代码上下文结构影响，并没有执行，那么此时计数不能正常驱动资源的析构，就需要用额外手段进行计数调配，让sp在预期的代码行为下，达到正常计数，释放资源
- share_ptr的普遍稳定行为是标记栈帧，即计数跟随函数作用域变动，若作为对象成员与对象生命周期关联，则有可能引发异常行为
- 即 shared_ptr 缺陷 ( `defect` )
#### reference count with share_ptr action
- create 0->1, or +1
- assign +1
- destructor -1
#### recursive with destructor and share_ptr
[cpp.reference.weak_ptr.use_count](https://en.cppreference.com/w/cpp/memory/weak_ptr/use_count)
```c++
#include <iostream>
#include <memory>
 
std::weak_ptr<int> gwp;
 
void observe_gwp()
{
    std::cout << "use_count(): " << gwp.use_count() << "\t id: ";
    if (auto sp = gwp.lock())
        std::cout << *sp << '\n';
    else
        std::cout << "??\n";
}
 
void share_recursively(std::shared_ptr<int> sp, int depth)
{
    observe_gwp(); // : 2 3 4
    if (1 < depth)
        share_recursively(sp, depth - 1);
    observe_gwp(); // : 4 3 2
}
 
int main()
{
    observe_gwp();
    {
        auto sp = std::make_shared<int>(42);
        gwp = sp;
        observe_gwp(); // : 1
        share_recursively(sp, 3); // : 2 3 4 4 3 2
        observe_gwp(); // : 1
    }
    observe_gwp(); // : 0
}
//output
use_count(): 0   id: ??
use_count(): 1   id: 42
use_count(): 2   id: 42
use_count(): 3   id: 42
use_count(): 4   id: 42
use_count(): 4   id: 42
use_count(): 3   id: 42
use_count(): 2   id: 42
use_count(): 1   id: 42
use_count(): 0   id: ??
```
可以发现，如果每次递归在函数入口都拷贝一次share_ptr，那么引用计数是跟随栈帧的，入递归前是n，出递归后也是n，因为递归的执行顺序，必然是内部执行完析构完退到当前栈帧的

#### owner_before
- [cpp.reference.owner_before](https://en.cppreference.com/w/cpp/memory/weak_ptr/owner_before)
- [zhihu](https://www.zhihu.com/question/24816143)

#### weak ptr
- 可以看作是一个可以持有shared_ptr对象引用的对象，`监视并持有`
- 仅仅是持有，不能做任何操作
- 在关键时，使用 lock 创建并返回所引用的shared_ptr对象，原子操作，安全系数高，若引用shared_ptr已析构，则返回空sp
- 用 weak_ptr 的持有sp引用特性，来`替代部分存储sp的行为`，消除sp缺陷，在经典的引用循环中就用wp成员替代了类中的sp成员，来消除sp本身的析构与管理对象析构的互相依赖冲突。

#### shared_ptr的主要用途
- 管理第三方接口的内存，通过自定义删除器来实现用三方专用释放接口替换默认的delete行为
- 实现特定数据结构以及内存控制行为，基本上也是通过自定义删除器，分配器，经典场景:c++11下，实现内存回收的对象池(通过替换对象池内外shared_ptr删除器，来实现资源的正常释放)
- 避免内存泄漏，通常运用在多线程场景下，各种dynamic共享对象的传递和引用，加之weak_ptr部分操作原子特性，消补shared_ptr的一些行为缺陷