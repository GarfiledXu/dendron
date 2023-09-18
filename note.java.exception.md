---
id: tmk3cpf4j3fqwg5q8i2fhjd
title: Exception
desc: ''
updated: 1694768355556
created: 1693838414264
---

### the specifiction of catch exception
1. one throw correspond to one catch
2. catch匹配规则: 当被抛出的exception属于catch中的子类或者类型相等，则匹配并捕获，在处理域中可以抛出异常
3. 多层catch 与 exception 继承的关系，与catch表达式在代码中的顺序无关，只与catch中的类型相关，优先匹配最相近类型进行处理

### there is need to throw exception on callback mehtod
--------
- to add throw exception declare in callback, let the obj to catch toggle
- to add flow contorl logic , sucess return true, then flow break, that could to catch exception on mehtod, and then to return true when cathed
- try catch the exception directly, then throw the runtime exception