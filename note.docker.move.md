---
id: u9pk3hyh2xahu6uhr62lgch
title: Move
desc: ''
updated: 1761372607010
created: 1761312786898
---

## 快速回顾: 镜像和容器的区别

相同点，本质上都是文件系统，并且容器基于镜像的引用
差异，容器可以看作是镜像运行后的文件系统
container = image + container layer
容器可以看作是在只读镜像的引用基础上(共享副本)，加入了可写的文件层，进行运行状态的管理

即镜像本质是多层叠加的只读文件系统，想要运行镜像，就需要引入一层可写层，用于承担镜像运行后的文件读写，
此时，镜像基础上引入了可写层 有运行状态的文件系统，称为容器
本质上都是文件系统，故容器可以将可写层转化为制度层，以此生成新镜像

## docker 对镜像的存储管理

1. 镜像本身就是一系列结构化数据（分层文件系统和元数据）的组合.
2. .tar镜像文件是一个压缩文件，需要经过解压，提取，注册到docker服务本地的存储结构中，用于运行管理

## 将已有镜像迁移到另一台电脑上运行

1. 镜像迁移

    ```bash
    docker save -o my_image.tar myapp:latest
    docker load -i my_image.tar
    完整的镜像分层信息；
    层间依赖、历史信息；
    镜像元数据（CMD、ENTRYPOINT、ENV等）。
    my_image.tar
    ├── manifest.json
    ├── repositories
    ├── layer1.tar
    ├── layer2.tar
    └── ...
    ```

2. 容器快照迁移

    ```bash
    docker export -o my_container.tar my_container
    docker import my_container.tar myapp_with_data:latest
    容器运行时文件系统的“合并快照”；
    包括镜像层和可写层；
    不含镜像层的历史结构与元数据。
    my_container.tar
    ├── bin/
    ├── etc/
    ├── usr/
    └── ...
    ```

3. 完整容器状态迁移

    ```bash
    docker commit my_container myapp_backup:latest
    docker save -o myapp_backup.tar myapp_backup:latest

    docker load -i myapp_backup.tar
    docker run -it myapp_backup bash
    # 本质上还是镜像打包

    ```

4. 系级别备份

    ```bash
    sudo tar czf docker_data.tar.gz /var/lib/docker
    在新电脑同样版本的 Docker 下恢复：
    sudo systemctl stop docker
    sudo tar xzf docker_data.tar.gz -C /
    sudo systemctl start docker
    ```

5. dev container 的迁移

    ```bash
    # dev container是一套解析创建容器的标准，用于将container作为开发环境
    所以，与普通contianer的区别，在于他先由dev container驱动，来创建容器
    # 1. 导出已经创建可运行的容器
    # 2. dev container配置文件+镜像
    但是核心的容器内vscode插件，数据并非存储在container文件系统，而是
    由docker server本地进行管理，并且用于多个dev container共享
    所以，迁移核心还是镜像+dev container配置文件，以及模板项目
    ```

## 当前使用的开发需求和迁移方案

0. 打包当前所有可用镜像
1. 以及镜像构建文本
2. 打包当前所有可用dev container配置和项目模板以及对应镜像
3. 保证可用的基础的x64 c++开发编译环境
4. 设计docker下交叉编译方式，是将sdk打包到镜像内部生成新镜像，还是以挂载方式并入
5. 如何在有层层依赖关系的镜像上，安全替换低层镜像

   ```bash
    ┌──────────────────────────────┐
    │ xjf/sdk-rv1106               │
    │ xjf/sdk-rv1126               │
    │ xjf/sdk-xxxx                 │  ← 针对不同平台的SDK镜像
    ├──────────────────────────────┤
    │ xjf/dev-base                 │  ← 公共开发基础镜像（通用层）
    └──────────────────────────────┘
    由于docker镜像制作的基础唯一性就是确定唯一的底层镜像，所以是不能替换，
    只能进行批量快速重构，并且做好版本维护才是正解

   ```

6. 制定镜像命名规则
   1. xjf/dev-base:v.1.1
   2. xjf/dev-cc_sdk_rv1106:v1.0-base1.1

7. 设计一套 可更新，通用的开发镜像制作和重构方案
   1. 镜像分类
      1. base-image
      2. sdk-image
      3. project-image
   2. 命名规范: 单靠镜像名称，无法完全表达当前镜像包含的具体软件功能列表，更多只能依靠名称标签+说明文档体现，同时构建镜像时，要将有关信息注入元数据
      1. 前缀-核心结构（冒号之前）：`<归属组织/个人>/<功能职责>-<具体分类: base/dev/project>`
         1. `xjf/dev/base:`
         2. `xjf/dev/sdk`
         3. `xjf/dev/project`
      2. 基础镜像后缀
         1. `<os>-<os_version>-<base_image_version_tag, eg: base-v1.101.1>`
      3. sdk镜像后缀
         1. `<base_image_version_tag>-<sdk_num>-<core_list_name太多就罗列核心>-<sdk-v1.01.1>`
      4. project镜像后缀
         1. `<image_tag>-<project_num>-<core_project_list_name>-<prj-v1.01.1>`
      5. 当前设计版本号数字只能标识镜像唯一性以及发布顺序，不能标识镜像内容特性，如何让编号附带一些直观的特性信息
         1. 需要标识组合内容还是单一内容，以及内容类型是否变化
         2. `v<type_seq>.<change_seq>.<type>`
            1. <type_seq> 保证新增类型/组合时序号递增
            2. <change_seq> 保证同一类型迭代更新唯一
            3. </type> 明确单/组合
      6. eg
         1. `xjf/dev/base:ubunut_22.04-base_v1.0.1.s`
         2. `xjf/dev/sdk:base_v1.0.1.s-sdk_v.101.1.m`
         3. `xjf/dev/project:sdk_v1.0.1.s-prj_v.101.1.s`

## record

```bash
## 查看当前镜像
xjf1127@MARK-I:~/wsl_workspace$ docker images
## 查看实际docker文件的空间占用
xjf1127@MARK-I:~/wsl_workspace$ docker system df

TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          56        6         34.06GB   32.2GB (94%)
Containers      7         0         14.08GB   14.08GB (100%)
Local Volumes   4         3         599.1MB   0B (0%)
Build Cache     146       0         1.973MB   1.973MB

```
