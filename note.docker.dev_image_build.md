---
id: wt9d1nu4fvz1v3qups64gid
title: Dev_image_build
desc: ''
updated: 1762182997118
created: 1761371967924
---

### 命名规范

[[note.docker.move]]

### 规范化的基础镜像：使用此前命名未经规范化，但经过基础构建开发环境可用的镜像

导出镜像

```bash
docker save -o /home/xjf1127/wsl_workspace/prj/docker/image/xjf_dev_v2.0.tar xjf_dev:v2.0
```

导入镜像

```bash
docker load -i xjf_dev_v2.0.tar
```

规范化重命名，打tag共享唯一imageid
`xjf/dev/base:ubunut_22.04-base_v2.0.1.m`

```bash
root@MARK-II:/home/xjf1127/codespace/docker/image# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
xjf_dev      v2.0      22c1ca1c106f   6 months ago   3.29GB
root@MARK-II:/home/xjf1127/codespace/docker/image# docker tag 22c1ca1c106f xjf/dev/base:ubunut_22.04-base_v2.0.1.m
root@MARK-II:/home/xjf1127/codespace/docker/image# docker images
REPOSITORY     TAG                          IMAGE ID       CREATED        SIZE
xjf_dev        v2.0                         22c1ca1c106f   6 months ago   3.29GB
xjf/dev/base   ubunut_22.04-base_v2.0.1.m   22c1ca1c106f   6 months ago   3.29GB
root@MARK-II:/home/xjf1127/codespace/docker/image# docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          1         0         3.291GB   3.291GB (100%)
Containers      0         0         0B        0B
Local Volumes   0         0         0B        0B
Build Cache     0         0         0B        0B
root@MARK-II:/home/xjf1127/codespace/docker/image# docker rmi xjf_dev:v2.0
Untagged: xjf_dev:v2.0
```

通过重构建方式，补充元数据说明

### xjf/dev/base:ubunut_22.04-base_v2.0.1.m

```bash
##===> dockerfile
# 基于已有镜像
FROM xjf/dev/base:ubunut_22.04-base_v2.0.1.m

# ----------------------
# 元信息标签
# ----------------------
LABEL maintainer="xjf <xjf1127@example.com>"
LABEL description="Ubuntu 22.04 基础开发镜像，包含常用开发工具和 Qt5 UI 支持"
LABEL version="v2.0.1.m"
LABEL build_date="2025-10-25"
LABEL os="ubuntu_22.04"
LABEL type="base"
LABEL usage="通用基础开发环境，可用于嵌入式及跨平台开发"
LABEL software_list="qtbase5-dev, qtchooser, qt5-qmake, qtbase5-dev-tools, gdb, strace, lsof, htop, tcpdump, net-tools, iputils-ping, vim, x11-apps, cmake_3.31.6"
LABEL qt5_ui_verified="true"

##===> build.sh
docker build -t xjf/dev/base:ubunut_22.04-base_v2.0.1.m .
## 构建命令说明
## 参数 build ：构建新镜像
## flag -t：打标签
## 最后一个参数 . ：指定构建时环境目录，如以来的dockerfile等会从该目录搜寻

##==> readme
### 构建背景

首先基于xjf/dev:v2.0版本镜像导入，后修改tag为xjf/dev/base:ubunut_22.04-base_v2.0.1.m

### 包含软件

Ubuntu 22.04 基础开发镜像，包含 Qt5、调试工具及 GUI 支持
qtbase5-dev, qtchooser, qt5-qmake, qtbase5-dev-tools, gdb, strace, lsof, htop, tcpdump, net-tools, iputils-ping, vim, x11-apps, cmake_3.31.6

### 为了将备注信息注入元数据，通过重构建，仅加入LABEL的形式活得同名镜像


```

### xjf/dev/base:ubunut_22.04-base_v2.0.2.m

dockerfile

```bash
# 使用你自己的基础镜像
FROM xjf/dev/base:ubunut_22.04-base_v2.0.1.m

# 创建具有 sudo 权限的非 root 用户
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    # 安装 sudo，并赋予该用户 sudo 权限
    && apt-get update \
    && apt-get install -y sudo \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 默认切换到非 root 用户运行
USER $USERNAME

```

bash build

```bash
docker build -t xjf/dev/base:ubunut_22.04-base_v2.0.2.m .
## 构建命令说明
## 参数 build ：构建新镜像
## flag -t：打标签
## 最后一个参数 . ：指定构建时环境目录，如以来的dockerfile等会从该目录搜寻
```

为dev container添加非root用户，构建镜像的方式2，在dev container配置文件中添加

```bash
## .devcontainer/devcontainer.json
{
  "name": "using xjf prebuild dev container",
  // "image": "xjf_dev_prebuild:v3.0",
  "build": {
    "dockerfile": "middle_dockerfile",
    "context": "."
  },

  // "postCreateCommand": [
  //   "id vscode || useradd -m -s /bin/bash vscode",
  //   "sudo -lU vscode | grep -q NOPASSWD || (echo 'vscode ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/vscode && chmod 0440 /etc/sudoers.d/vscode)"
  // ],
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-vscode.cpptools",
        "ms-vscode.cmake-tools",
        "hanfengv.ddd",
        "dracula-theme.theme-dracula",
        "formulahendry.auto-close-tag",
        "-github.copilot"
      ]
      ,
      "settings": {
        "terminal.integrated.defaultProfile.linux": "bash"
      }
    }
  },
  // "features": {
  //   "ghcr.io/devcontainers/features/common-utils:2": {}
  // },

  "remoteUser": "vscode"
}

```

