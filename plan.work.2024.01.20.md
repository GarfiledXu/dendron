---
id: sak9b7oekm7wajjf2hps90x
title: '20'
desc: ''
updated: 1705746152060
created: 1705717935755
---

#### must
- [x] i30 and objective state flow draw first version : 30min
- [ ] smc offical manual read complete: 1hour
#### to do
- [x] bilibili video: 手把手实现多线程下载以及断点续传，overview： 1hour
    1. 在curl框架基础上
    2. 通过http的头，在下载前获知要下载文件的大小，来进行文件映射和seek
    3. 多线程每次操作时记录当前数据，以便后续操作中断后的重新恢复
- [x] imgui: 渲染 图片，以及框，控制大小等
- [ ] windows上virtual studio编码问题，为什么设置为utf-8后无法处理中文字符路径
    1. 程序源码编码
    2. 程序内码
    3. 运行环境编码
- [x] 垂直同步是什么？
    将显示器的刷新率作为渲染的频率上限，避免过度渲染，性能浪费