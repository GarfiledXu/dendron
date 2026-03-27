---
id: jllcjc5ant1fpbbiattuium
title: Repo
desc: ''
updated: 1774427114396
created: 1774418589169
---

### 常用命令

`repo list` 查看所有项目

```bash
. : products/reader_product_base
app/nvs/cmake : common/cmake_base
app/nvs/third_party : app/third_party
app/nvs/third_party/libjpeg-turbo/libjpeg-turbo : common/libjpeg-turbo
app/nvs_recovery/cmake : common/cmake_base
app/nvs_recovery/third_party : app/third_party

```

`jfxu@xssw-ubuntu:~/test$ repo sync app/nvs` sync模块 

### `git repo sync` 执行成功，但进度为0

输入账户密码成功，但就是进度为0
问题原因：中途曾经输入密码错误，有概率导致后续输入成功，但无法正常进行sync

解决方案：git 账户密码免密设置，规避密码错误情况，通过设置`ssh`或者`git config --global credential.helper store`