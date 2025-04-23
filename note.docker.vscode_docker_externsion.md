---
id: km23abkmtjg8kfxj55sso0f
title: Vscode_docker_externsion
desc: ''
updated: 1745421808959
created: 1745414548916
---

## refe

- [visual studio offical: vscode dev container](https://learn.microsoft.com/zh-cn/training/modules/use-docker-container-dev-env-vs-code/3-use-as-development-environment)
- [vscode offical: devcontainer](https://code.visualstudio.com/docs/devcontainers/containers) : 位于overview DEV CONTAINERS 一栏

## vscode 扩展工作原理

1. 首先vscode可以将容器作为完全的开发环境，并在容器上使用vscode完全的功能，通过扩展连接容器后，装载或者复制方式将本地文件集成到容器中，根据项目中 `.devcontainer` 文件夹，作为配置文件，来处理容器相关配置和默认行为
2. `.devcontainer/devcontainer.json`存放由`dev container`规范编写的元数据，会影响vscode对开发容器的创建开发行为，如vscode插件注入，在镜像基础上额外的环境依赖注入等
3. 官方文档结构:
   1. overview
      1. docker
      2. dev containers
   2. dev container
      1. quick create container by shortcut way
      2. quick create container by diff opt 
      3. simple dev container standard breif
      4. vscode attch to a exist container
      5. manual create a dev container
      6. Dev Container CLI
      7. dev container standard configure file `devcontainer.json` linker
      8. some tips and tricks and FAQ
4. vscode提供的基本操作方式:
   1. 命令面板
   2. 插件面板 按钮 + 上下文菜单
   3. 文件浏览面板 上下文菜单
   4. 左下角的 remote status bar + command select
5. 基本工作流:
   1. remote status bar quick start:
      1. 点击 remote status bar
      2. 选择 new contaner
      3. 选择 对应与构建的 image
      4. 连接容器成功，进入容器
      5. ![alt text](image-23.png)