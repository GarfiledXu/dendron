---
id: kf9p08xm7v96ckhjfqwdamr
title: List Comprehension
desc: ''
updated: 1698717528493
created: 1698647943893
---

#### refe
[offical](https://docs.python.org/zh-cn/3.7/tutorial/datastructures.html)

**语法糖，简化遍历处理去生成目标列表**


#### structure
[< expression for item > for < 每次遍历时的元素 变量名称> in < 被迭代对象 > if < condition for filter item, return true to save> ]

#### execute serial and io
**foreach item ->(input item) conditon expression -> (input return true item) expression -> (input expression return value) append to result list**

```python
def libcompile_qnx(cmake_src: CmakeSrc):
    cmd_variable_str_list = [f' -D{key}={value} ' for key, value in cmake_src.cmd_var_dict.items()]
    cmd_variable_str = ''.join(cmd_variable_str_list)
```