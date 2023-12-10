---
id: 5n9acthdptxorhaacy1hndj
title: Malloc_and_pointer
desc: ''
updated: 1700816628061
created: 1700709738386
---

#### scene
> 为什么传递出去以后，process 时crash?
```c++
inline void jpg_four2one_uyvy_process(const std::vector<std::string>& local_path) {
    SLOGD("enter jpg_four2one_uyvy_process");
    int entity_num = 4;
    // ASVLOFFSCREEN offscreens[entity_num];
    ASVLOFFSCREEN* frame[entity_num] = {0};
    for (int i = 0;i < entity_num;i++) {
    SLOGD("enter jpg_four2one_uyvy_process, i = {}", i);
    #if 1
        tv::Mat mat = tv::imread(local_path[i], 1);
        tv::FourccBuffer fourcc_buff;
        tv::cvtToFourcc(mat, fourcc_buff, tv::PIX_FMT_UYVY422);
        ASVLOFFSCREEN* arc_buff = new ASVLOFFSCREEN();
        *arc_buff = tv::cvtToArc(fourcc_buff);
        frame[i] = arc_buff;
    }
    return frame;}

inline void sdk_process(ASVLOFFSCREEN* frame_in[]) {
    for (int k = 0;k < 4;k++) {
        if (frame_in[k]->ppu8Plane[0] == NULL) {
            SLOGE("ERROR EMPTY");
        }
    }
    SLOGW("need format:{}, curformat:{}, width:{}, height:{}", ASVL_PAF_UYVY, frame_in[0]->u32PixelArrayFormat, frame_in[0]->i32Width, frame_in[0]->i32Height);
    SLOGD("enter sdk_process");
    SDK_SENTRY_MODE_RESULTS result = {0};
    int ret = SDK_Sentry_Mode_Process(hEngine,frame_in, 4, &result);
    // if (ret!=MOK && ret!=65539) {
    if (ret!=MOK) {
        SLOGE("SDK_Sentry_Mode_Process error! ret:{}", ret);
        exit(0);
    }
    SLOGD("end sdk_process");
}
```
> data 生命周期受fourcc buff管理，而cvt函数仅仅是指针赋值，如果采用栈变量，跳出作用域后将直接销毁，导致指针的副本变为悬空指针，导致crash
> 1. 将栈变量变为堆变量，手动管理堆变量生命周期来间接维持内部data指针的生命
> 2. 拷贝处采用深拷贝而不是浅拷贝

##### 指针数组与**


##### **?
new/malloc 返回出来的是一个地址常量值
地址与内存 引用关系
地址与指针变量 持有/存储关系

传递**，是为引用 *, 即指针变量，将函数内new出的内存地址赋值给外部指针变量，要么传参该指针类型的引用，要么传递指针类型的指针，达到引用效果, 所以关键还是在引用
