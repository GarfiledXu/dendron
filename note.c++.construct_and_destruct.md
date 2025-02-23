---
id: 7p61ssxd1eetatrnmay6v7i
title: Construct_and_destruct
desc: ''
updated: 1717047748869
created: 1717047447221
---

#### 构造default修饰后 静态成员变量 在程序结束时的析构异常
##### 现场还原
```c++
class ThreadlooperBase
{
public:
    // ThreadlooperBase() {
    //     SLOGD("enter ThreadlooperBase constructor");
    //     SLOGD("out ThreadlooperBase constructor");
    // };
    ThreadlooperBase() = default;
    // ~ThreadlooperBase() = default;
    virtual ~ThreadlooperBase() {
        SLOGD("enter ThreadlooperBase destructor");
        signal_stop();
        join();
        SLOGD("out ThreadlooperBase destructor");
    };

    int launch_front();
    int launch_back();

    void join();
    void detach();

    bool is_running();
    virtual void signal_stop();

protected:
    virtual bool do_init() = 0;
    virtual bool do_once() = 0;
    virtual bool do_release() = 0;


protected:
    void run_logic_();
    long loop_counter_ = 0;
    bool is_backend_ = true;
    bool is_running_ = false;
    bool to_stop_ = true;
    std::thread work_;
};
template<typename TaskReturn>
class SingleBackendThread : public ThreadlooperBase
{

public:

    // SingleBackendThread() {
    //     SLOGW("enter SingleBackendThread constructor");
    // };
    SingleBackendThread() = default;

    virtual ~SingleBackendThread() {
        SLOGW("enter SingleBackendThread destructor");
        signal_stop();
        join();
        SLOGW("out SingleBackendThread destructor");
    };
    
    int post_sync(int filter_value, std::function<TaskReturn()> task_entity);
    int post_async(int filter_value, std::function<TaskReturn()> task_entity);

    SingleBackendThread& set_result_cb(std::function<void(int, TaskReturn)> in_result_cb) {
        result_cb_ = std::move(in_result_cb);
        return *this;
    }

    virtual void signal_stop() override {
        ThreadlooperBase::signal_stop();
        to_stop_ = true;
        cv_.notify_all();
        cv_post_sync_.notify_all();
    }

protected:

    virtual bool do_init() override {
        SLOGD("enter do init");
        return false;
    };
    virtual bool do_release() override {
        SLOGD("enter do release");
        return false;
    };

    virtual bool do_once() override {
        SLOGD("enter do once");
        return wait_for_task();
    };

private:
    int wait_for_task();
    bool check_stop_signal();

    std::function<void(int, TaskReturn)> result_cb_;
    std::function<TaskReturn()> task_entity_;
    int filter_value_;

    bool to_stop_ = false;
    std::mutex mtx_;
    std::condition_variable cv_;

    std::mutex mtx_post_sync_;
    std::condition_variable cv_post_sync_;

};
class TaskLogicBase {
public:

    TaskLogicBase() {
        single_backend_thread_.launch_back();
    }

    virtual int launch_update(void*);
    virtual int terminate_update();

    virtual int launch_run_testbed(void*);
    virtual int terminate_run_testbed();

    virtual int terminate_all();

protected:

    virtual std::string get_task_tag_() { return "TaskLogicBase"; };

    static SingleBackendThread<OptReturn> single_backend_thread_;

};


```
在程序将以上源码编译之后，并且没有调用如何api, 在程序结束时，会发生异常
```bash
terminate called after throwing an instance of 'std::__1::system_error'
  what():  mutex lock failed: Invalid argument
Abort (core dumped)
```
可以确定是和single_backend_thread_静态对象有关，在该对象析构的时候，发生错误

##### 尝试改进
将子类或者父类中的一个构造函数改为非default修饰后，就能构正常，这是什么原因？

##### default 做了什么
