---
id: 9i16w7j1n7so2hbqnjg5po8
title: Array_and_reference
desc: ''
updated: 1696563002382
created: 1696561430954
---

#### reference to array
**数组的引用，对象是引用，是一个new type，是一个和数组类型相关的引用类型，该类型的声明方式与函数指针类似，即在数组声明的基础上，将原本数组标识前面加上引用符号，并用括号攘括**
```c++
int n3[3] = {2, 4, 6};
int (&rn3)[3] = n3;     
```

```c++
//glib
template<class _Tp, size_t _Nm>
    inline _Tp*
    begin(_Tp (&__arr)[_Nm])
    { return __arr; }
 
template<class _Tp, size_t _Nm>
    inline _Tp*
    end(_Tp (&__arr)[_Nm])
    { return __arr + _Nm; }
```
> 为什么是使用数组引用的形参而不是数组类型或者数组指针？
> guess: 数组引用的类型能够保留数组长度，让在编译期间模板能够自动推导获取数组长度

#### array of references
**引用的数组，对象是数组，是一个元素类型为引用类型的数组，所以声明方式与数组相同，只是声明类型变为了引用类型**
```c++
//1
int &array[]
//2
int i,j;
int *a[5] = {&i, &j};
a[0]; // point to i
a[1]; // point to j
```