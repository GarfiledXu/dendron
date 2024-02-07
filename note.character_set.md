---
id: f59djadu69dvfr2zg5nbv83
title: Character_set
desc: ''
updated: 1705991068904
created: 1705988901628
---

#### ref
[消失的代码](https://xyz1001.xyz/articles/58691.html)
[VS 与 UTF-8 编码: very nice](https://www.cnblogs.com/cyds/p/16226233.html)
[愿编程不再乱码(含Qt)-根因深究: very nice](https://ifmet.cn/posts/c0862e62/)
[cpp源码在跨平台场景下文件编码的思考](https://zhuanlan.zhihu.com/p/624847186)
[字符集的那些事](https://github.com/SPURSGO/CharacterSet)
[拨开字符编码的迷雾--编译器如何处理文件编码](https://www.cnblogs.com/jiangxueqiao/p/7464408.html)
[编译器对宽字符的处理](https://blog.csdn.net/flyer0704090111/article/details/119876923)
[C++ 编译器对字符编码的要求和处理方式](https://blog.csdn.net/ayang1986/article/details/119939128)
[c++ 程序的三种编码](https://blog.csdn.net/qq_16334327/article/details/87854556)

#### ASCII 与 ANSI
> ASCII 是最原始的字符编码集合，128个字符，但不要将其混淆为 ANSI ，它实际上只是一个总称，是不同国家自己所定义的字符集的总称，所以在不同地区所使用的具体种类的 ANSI 编码是不同的，并且不兼容，
所以ANSI并不指向具体编码字符集，每种具体ANSI都可以说是ASCII的变体或者扩展(兼容了ASCII但互相不兼容)
所以，编码问题主要是和中文字符相关的

#### windows 和 ANSI
> windows上默认使用ANSI编码，而具体的编码格式通过 code page 获知(cmd 命令 chcp),若查询到936则表示使用的是GBK编码，GB18030，同时使用chcp来改变默认代码页
字符编码和字符集

> 字符编码是 字符到二进制数据的直接映射关系

> 字符集即 字符编码集合，通常来说一个字符集中的每一个字符都有唯一映射关系，但unicode中存在多套映射

#### 方案
> GBK编码，每一代是兼容上一代的，即新一代扩容了汉字，所以会出现，部分汉字只在新标准的GBK中
手动判断

[字符编码查询](https://www.lddgo.net/string/char-encode) 通过提前获知字符不同编码对应的编码值来对比程序中编译运行后直接打印的内存编码值来判断编译器的编码处理

#### 字符集和字体文件
> 字体文件内包含了各种字符集与字体文件内字体的映射关系