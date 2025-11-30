---
id: v91h1lqw7lshhgiequrtztt
title: Uid
desc: ''
updated: 1762181713044
created: 1762181185021
---

## linux uid分配规则

1. linux所有的文件用户权限都是基于uid，而不是username，也就意味着在一些虚拟化的场景下，比如dev container中，只要保持uid一致，就能拥有相同的访问权限，而不必用户名一致，比如dev container通过使用非root用户，来挂载共享卷访问宿主文件
2. 0 root，超级用户
3. 1-99 系统保留账户（如 daemon, bin, syslog）
4. 100-999 服务账户（如mysql, www-data）
5. 1000~++ 普通用户，通常注册的第一个非root用户