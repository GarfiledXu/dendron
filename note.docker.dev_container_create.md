---
id: 08655m7q1lkikia0c4b5h3z
title: Dev_container_create
desc: ''
updated: 1762440080737
created: 1761381645596
---

### dev container 的工作原理和常见工作流整理

1. 相关对象的工作关系

dev contianer 本质是一套配置标准，基于docker容器和挂载等技术，提供项目级别的开发环境自动化配置，隔离和管理
组成：vscode dev contianer配置文件/镜像dev container元信息 + command pannel解析执行

### 基于基础镜像xjf/dev/base:ubunut_22.04-base_v2.0.1.m进行开发容器的创建

1. 文件准备

    ```bash
    |---.devcontainer·
    |--------devcontainer.json
    ### devcontainer.json content
    {
    "name": "dc-xjf/dev/base:ubunut_22.04-base_v2.0.1.m",
    "image": "xjf/dev/base:ubunut_22.04-base_v2.0.1.m",
    "customizations": {
        "vscode": {
        "extensions": [
            "adpyke.codesnap",
            "danielpinto8zz6.c-cpp-compile-run",
            "github.vscode-pull-request-github",
            "mhutchie.git-graph",
            "iliazeus.vscode-ansi",
            "maziac.binary-file-viewer",
            "qiaojie.binary-viewer",
            "maziac.hex-hover-converter",
            "ms-azuretools.vscode-docker",
            "ms-python.autopep8",
            "ms-python.black-formatter",
            "ms-python.debugpy",
            "ms-python.python",
            "ms-python.vscode-pylance",
            "ms-vscode.cmake-tools",
            "ms-vscode.cpptools",
            "ms-vscode.cpptools-extension-pack",
            "ms-vscode.makefile-tools",
            "vadimcn.vscode-lldb",
            "yzhang.markdown-all-in-one"
        ],
        "settings": {
            "terminal.integrated.defaultProfile.linux": "bash"
        }
        }
    },
    "remoteUser": "vscode"
    }
    ```

2. 将文件夹添加到workspace的根目录，然后再进行reopen in contianer，会进行构建解析/或者右下角会跳出提示框
3. 需要给镜像添加第一个非root用户，通常获取到1000的uid，可以激活更多的vscode dev container内部配置(终端专属配色，哈哈)，保持更好的文件安全和兼容性
   1. [[note.linux.uid]]
   2. https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user

4. 修改`.devcontainer/devcontainer.json` 配置，后vscode会自动检测，并提示重加载，那么镜像会重新构建
5. workspace配置整备
   1. c_cpp_properties.json

        ```bash
            {
                "configurations": [
                    {
                        "name": "Linux",
                        "compilerPath": "/usr/bin/gcc",
                        "cStandard": "c11",
                        "cppStandard": "c++17",
                        "intelliSenseMode": "linux-gcc-x64",
                        "includePath": [
                            "${workspaceFolder}/**",
                            "/usr/include/**",
                            "/usr/local/include"
                        ],
                        "defines": [
                            "_DEBUG",
                            "LINUX"
                        ]
                    }
                ],
                "version": 4
            }
        ```

   2. pe-qt5_x64_demo.code-workspace

        ```bash
            {
                "folders": [
                    {
                        "path": ".."
                    }
                ]
            }
        ```

   3. settings.json

        ```bash
        {
            "cmake.sourceDirectory": "/workspaces/v2.0.2m/pe-qt_test",
            "files.associations": {
                "*.json": "json",
                "CMakeLists.txt": "cmake",
                "qapplication": "cpp"
            }
        }
        ```

   4. task.json
      1. 在当前cmake和bash驱动的项目结构中，task.json似乎有些多余，作为编译入口
      2. 统一入口，通过vscode菜单出发
      3. 调用vscode资源以及内建变量
      4. 与launch.json进行配合集成
      5. 相比bash，实用点
         1. 可以通过内建变量，比如workspace路径，来设定一些临时目录
   5. launch.json
      1. 进行vscode 可视化gdb
6. 对于一个已构建的dev container，如何正确切换挂载目录，达到快速切换同类型项目的效果，或者是如何不重复构建的情况下，多个项目共享一套dev container
   1. 直接复制`.devcontainer`，然后通过 command pallte `open folder in container`，能够立刻打开，通过docker面板查看
      1. container id 和 image 名称都不相同，但打开速度的确是秒开
      2. ![alt text](image.png)
      3. 窥探vscode的容器构建机制：
         1. 计算配置哈希：VSCode 根据 .devcontainer 目录内容（Dockerfile、devcontainer.json、mounts 等）计算出一个哈希（类似 fingerprint）
         2. 查找已存在的镜像缓存，如果匹配，直接复用
         3. 实验验证
            1. 查看 两个image是inspect发现，digest layer的确相同，仅名称不同，可以理解为的确是一个image，通过tag操作进行区分
            2. container inspect存在差异，主要集中在mount的内容不同，每一个container专属mount了.devcontainer所在目录
   2. 复制 `.devcontainer` 后增改vscode相关配置，以及`mount` `bind`
      1. 同样会重新构建容器，image的话，基本上只是换了一个hash-tag.
   3. 好奇，vscode dev container中的插件缓存是从一个所有容器的共享池里拷贝引用到当前容器呢，还是单纯的重安装，经历下载，安装，不是复用
7. 对比通过打开文件夹的形式和打开`.code-workspace`工作区文件 加载dev container的区别
   1. 两种打开方式本质上是一样的，切换时可以认为都不需要重新构建
   2. 从配置生效角度，`.vscode`下的大部分配置，两种方式都能享受到，但是`.code-workspace`内的设置，必须是`open workspace form file`方式打开的工作区才能享受到
   3. 总结：先配置.devcontainer决定系统挂载路径，再配置.vscode来切换工作区需要的文件夹，最后，通过`open workspace from file`重新打开dev container
8. 理想的项目打开流程，dev容器复用流程
   1. 当前痛点，dev container的生命周期和管理被绑定到工作区，是一种分散原始的文件夹管理方式
   2. 理想： 命令面板选择已有 devcontainer配置->选择项目文件夹 相当于虚拟拼接
   3. 云端dev容器模板仓库
   4. 本地dev容器模板仓库，暂未有该途径，本质上模板是包含了镜像+devcontainer配置以及额外配置等一些数据的集合，所以本地提供这样的路径，似乎并没有什么必要.
   5. 当前设计，从概念和文件结构上都划分为两个模块 docker 和 project
      1. docker provider: 镜像构建以及压缩包存储，以及devcontainer和vscode模板配置，进行根目录名称为docker的统一的文件管理.
      2. project referencer: 再暴露结构配置，和入口脚本，用于项目开发环境搭建时进行快速引用，结合项目本身的统一管理.

## vscode devcontainer command explanation

1. 核心手册：https://code.visualstudio.com/docs/devcontainers/create-dev-container
2. 核心插件：ms-vscode-remote.vscode-remote-extensionpack

3. `Dev Containers: Open Folder in Container`
4. `Dev Contaienrs: New Dev Container`
5. `Dev Containers: Add Dev Container Configures Files`