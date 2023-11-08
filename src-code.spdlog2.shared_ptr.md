---
id: 83z5pwbb51dby6roiambxwv
title: Shared_ptr
desc: ''
updated: 1696564585852
created: 1695870644197
---

#### constructor
- [cpp-reference-constructor](https://en.cppreference.com/w/cpp/memory/shared_ptr)
- [online compile editor test](https://onlinegdb.com/SQltTkTkS)
- [shared_from_this](http://blog.guorongfei.com/2017/01/25/enbale-shared-from-this-implementaion/)
#### operator bool
- check shared_ptr status by condition expressioni
- when shared_ptr not contain avalid pointer or have a nullptr, using condition expressin for shared_ptr obj will trigger `operator bool` that convert object to a bool value
```c++
#include <iostream>
#include <memory>
void report(std::shared_ptr<int> ptr) 
{
    if (ptr)
        std::cout << "*ptr=" << *ptr << "\n";
    else
        std::cout << "ptr is not a valid pointer.\n";
}
int main()
{
    std::shared_ptr<int> ptr;
    report(ptr);
 
    ptr = std::make_shared<int>(7);
    report(ptr);
}
//output
ptr is not a valid pointer.
*ptr=7
```
#### reset 
- undefined behavior
    - transfer origin ptr which held by current shared_ptr to reset, will destruct the obj first, and then manager the dangling pointer

#### reference count how to be stored by shared_ptr
- guess: allocate one dynamic memory for counter when count equal to 1
- guess: update the counter reference and modify num when operated between shared_ptr  
- the reference count of shared_ptr doesn't trace unique physical pointer, just trace on the pointer which enter the sharedptr

#### copy and move behavior
- operator = copy
    1. copy the origin pointer 
    2. copy reference counter reference
    3. reference count increase +1
- operator = move
    1. move the origin pointer from right value
    2. clear the reference count of right vaue, 