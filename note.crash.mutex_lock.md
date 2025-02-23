---
id: 5ttylsessxwag4j0i38k9hy
title: Mutex_lock
desc: ''
updated: 1726730362007
created: 1726728682044
---

#### 线程安全的queue，在进行push时发生崩溃
```c++
void push_sync(const T_item& lv) {
    inner_push_sync_(lv);
};
void push_sync(T_item&& rv) {
    inner_push_sync_(std::forward<T_item>(rv));
};
template<typename T>
void inner_push_sync_(T&& value) {
    std::unique_lock<std::mutex> ul(mutex_);
    {
        condition_push_.wait(ul, [&]() {
            return (inner_queue_.size() < limit_len_max_.load()) || (is_stop_flag_.load());//already lock, shouldn't use size_();
        });
        if (is_stop_flag_.load()) {
            return;
        }
        inner_queue_.push_back(std::forward<T>(value));
    }
    condition_pop_.notify_one();//correspond to push one unit
    return;
}

int pop_async(T_item& lv) {
    return inner_pop_async_(lv);
}

int inner_pop_async_(T_item& out_value) {
    std::unique_lock<std::mutex> ul(mutex_);
    if (is_stop_flag_ || (inner_queue_.size() < 1))
        return -1;
    out_value = inner_queue_.front();
    inner_queue_.pop_front();
    condition_push_.notify_one();
    return 0;
}
```
**破案: crash原因: 空指针，由于该队列所在模块指针初始化较慢，前一个模块初始化后异步线程开始工作并且引用队列所在模块，导致空指针引用**