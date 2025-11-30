---
id: 4tduds1ycsepgf4n9bijc8y
title: File_migrate
desc: ''
updated: 1762778085846
created: 1762776146409
---

## 如何将一个旧镜像中的部分文件迁移到新镜像中

1. 待选方案
   1. 从旧镜像导出整个文件系统，然后导入新镜像后选择性处理.
   2. 手动在旧镜像的容器中打包，然后拷贝到新镜像中解压.
   3. 通过镜像制作的方式，引用旧镜像作为镜像来源之一，通过COPY等方式拷贝旧镜像文件.

## 方案3实践

1. 通过私有register将其他machine上的旧镜像pull到本地.
2. 进行构建

   ```bash
   FROM rk1126/old-sdk:latest AS old
   FROM ubuntu:22.04

   COPY --from=old /opt/rk1126sdk /opt/rk1126sdk
   COPY --from=old /opt/qt /opt/qt

   ENV RK1126_SDK=/opt/rk1126sdk
   ENV PATH=$PATH:/opt/qt/bin

   RUN apt update && apt install - build-essential
   ```
