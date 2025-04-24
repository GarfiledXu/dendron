---
id: rna6s1mf2y52adjbhem2xnf
title: Dev_container
desc: ''
updated: 1745466994752
created: 1745411863552
---

### refe

- [github: dev container](https://github.com/devcontainers/spec)
- [offical: dev container](https://containers.dev/)
- [stack overflow: prevent vscode using pulling to build](https://stackoverflow.com/questions/78977900/prevent-vscode-from-pulling-during-devcontainer-build)

### concept

1. dev container: 是一个使用容器作为开发环境的通用标准，意味着不归属于具体的平台，工具。只要平台或者工具支持该标准，那么相关的特性和模板即可生效。
2. 官方文档结构:
   1. overview: 介绍 dev container 的简介，元数据配置文件格式，解释了 dev container 在 编译和测试环境中的意义，保持环境一致性
   2. specification
3. 支持该标准的平台和工具:
   1. vscode
   2. vscode dev container externsion：通过 dev container相关的配置文件，来定义vscode中docker相关操作的默认行为，比如通过右键上下文菜单中`build image`来执行镜像构建
   3. visual studio
   4. intelliJ IDEA
   5. Dev Container CLI
4. 和 dev container 相对的概念: production container
