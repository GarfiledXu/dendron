---
id: qaaj2rfhfrvxqbmkfpyybcr
title: Basic Grammer
desc: ''
updated: 1698647587250
created: 1698633612213
---

### main entry
```python
if __name__ == "__main__":
    pass
```

### class name declare
```python
class ClassName:
    def __init__(self) -> None:
        pass
    def __del__(self) -> None:
        pass
```
    python 类并不为自己作用域下定义的函数，隐式添加self对象(类比c++中this指针), 赋予访问成员能力

### class built-in function and variable
all class carried by default
**function**
1.`__init__(self, ...)`
 will be called  automatically by one class be created/initiated, and pass the initial argument to the init function as input

2.`__del__()`

3.`__str__(self)`
 this built-in function controls what should be returned when the class object is represented as a string.
```python
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age
  def __str__(self):
    return f"{self.name}({self.age})"
p1 = Person("John", 36)
print(p1)
```

4.`__repr__`(self): 返回一个包含对象信息的字符串，通常用于调试。当直接输出对象时，会调用 __repr__ 方法。

5.`__len__`(self): 返回对象的长度，通常用于实现容器类。

6.`__getitem__`(self, key): 用于获取对象的指定元素，通常用于实现容器类。

7.`__setitem__`(self, key, value): 用于设置对象的指定元素，通常用于实现容器类。

8.`__delitem__`(self, key): 用于删除对象的指定元素，通常用于实现容器类。

9.`__iter__`(self): 返回一个迭代器对象，通常用于实现可迭代的容器类。

10.`__next__`(self): 返回迭代器的下一个元素，通常用于实现可迭代的容器类。



**variable**
1.self
 self is an variable which store the reference of current object, pass to the function which belong to current class, give them the capacity to access the member variable from current object
> 类中所有的成员变量引用，必须通过 `self` 进行显式调用，可以理解为c++中的所有成员函数必须声明this参数，并且通过this来显式访问成员变量

> 由于 self 的显式传参和引用机制，成员函数之间并无任何区别，仅仅是多了一个类作用域的修饰，直接互相调用

2.`__class__`: 包含一个指向对象所属类的引用。

3.`__doc__`: 包含类的文档字符串（通常称为 docstring）。

4.`__module__`: 包含定义类的模块的名称。

5.`__dict__`: 包含了类的命名空间，其中存储了类的属性和方法。

6.`__name__`: 如果类在模块中被定义，这个变量包含了类的名称。

### class constructor and destructor

### class member access modifiers
**python 通过变量的符号命名规则，来控制成员的访问权限，而不是通过关键字控制，`__`开头的符号，表示私有成员，类的作用域下(定义)函数可以通过self进行访问, 而在类的作用之外(类对象的调用处)**

### object instantiation
just using one symbol to be assigned by =
> 实例化一个对象时，对象符号优先，即等号左边优先写对象符号名称， 然后是右边决定使用什么类型来进行构造，类名+括号+__init__的参数列表
定义形参时同样的思路，对象符号优先, 再通过冒号:, 紧跟声明的类型(非必须)
```python

```


### class properties

