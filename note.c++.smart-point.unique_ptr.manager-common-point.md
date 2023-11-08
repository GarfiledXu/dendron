---
id: 8d5e7xbq6dq94a7pesadqqt
title: Manager Common Point
desc: ''
updated: 1695278123798
created: 1695266026230
---

### sence 1
pass common point to member var of class on contructor, if point is null, will new point assign to member
- should to free outside point?
- what is the best way to free the member point which alloc by contructor when outside point is null?
1. use one mark correspond to if outside point pass to the contructor is null
2. use unique_ptr to wrap outside point assign to member point
   - question: whether unique_ptr will auto free the point which from outside
   - answer: yes, the default behaviour of unique_ptr is auto free the point when leaving scope regardless of the source of the pointer
   - so:using stack pointer pass to the constructor of unique_ptr will raise runtime error!
  ```c++
  //
   FFmpegH264Decode(VideoDecodeCallback* decode_callback, const std::string& video_path)
        : is_outside_callback_(true), decode_callback_(decode_callback), decode_listener_(nullptr), video_path_(video_path), video_info_("") , is_out_decode_(false){
        if (decode_callback == nullptr) {
            decode_callback_ = new VideoDecodeCallback();
            is_outside_callback_ = false;
        }
    };
    FFmpegH264Decode(VideoDecodeListener* decode_listener, std::string& video_path)
        : is_outside_callback_(true), decode_listener_(decode_listener), decode_callback_(nullptr), video_path_(video_path), video_info_("") , is_out_decode_(false){
        if (decode_listener == nullptr) {
            decode_listener_ = new VideoDecodeListener();
            is_outside_callback_ = false;
        }
    };
    virtual ~FFmpegH264Decode() {
        decode_callback_ == nullptr ? delete decode_listener_ : delete decode_callback_;
        decode_callback_ = nullptr;
        decode_listener_ = nullptr;
    };
  ```
  ```c++
  #include <memory>
#include <stdio.h>
struct var1 {
    int a = 0;
  var1(int aa){ a = aa;printf("var1 construct:%d\n", a);}
  ~var1(){printf("var1 destruct:%d\n", a);};
};
class MyClass {
private:
    std::unique_ptr<var1> myPtr;
    std::unique_ptr<var1> myPtr2;

public:
    MyClass(var1* externalPtr) {
        myPtr = std::unique_ptr<var1>(externalPtr); // 使用外部指针
        myPtr2 = std::unique_ptr<var1>(new var1(2));
    }
};

int main() {
    // int* externalPtr = new int; // 外部分配内存
    var1* var = new var1(1);
    var1 var2(3);
    MyClass obj(&var2); 
    return 0;
} 
  ```
**how to optimize this scene**
- redecalre function argument, instead of unique_ptr
- noting to do, because it's  compatible using stack variable pass to constructor