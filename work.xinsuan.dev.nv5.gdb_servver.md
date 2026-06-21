---
id: rl20qnmhq1gpa8bjsawuwnf
title: Gdb_servver
desc: ''
updated: 1781244175445
created: 1781244167041
---

# nv5-hi3519 VSCode 远程 GDB 调试配置记录

本文档记录如何在宿主机（PC 端）通过 VSCode 直连下位机（开发板）进行 GDB 远程可视化单步调试的配置步骤。

## 1. 提取交叉编译工具链至本地宿主机

为解决 VSCode 调试器通过 Docker 代理脚本调用 GDB 时产生的权限及数据流异常问题，需将容器内的交叉编译工具链拷贝至宿主机本地。

由于进入 Docker 时已挂载 `/home/jfxu` 目录，可直接在容器内完成拷贝：

```bash
# 1. 启动并进入容器
xs_docker_run.sh choose xs_sw_ubuntu-22.04_hisi
xs_docker_run.sh run 
source /etc/profile

# 2. 在容器内创建宿主机本地存放工具链的目录
mkdir -p /home/jfxu/codespace/my_experiment/common_env/compilers/aarch64-v01c01

# 3. 将 /opt 下的工具链完整拷贝到刚刚创建的共享目录中
cp -r /opt/buildtools/gcc-20230609-aarch64-v01c01-linux-musl/* /home/jfxu/codespace/my_experiment/common_env/compilers/aarch64-v01c01/

# 4. 退出容器
exit
```

## 2. 修改工程环境变量

在工作区根目录的 `env.sh` 中，将对应的编译器前缀修改为刚才拷贝出来的本地绝对路径，使其脱离 Docker 调用：

```bash
declare -A WS_CROSS_PREFIX_MAP=(
    ["arm_3519"]="/home/jfxu/codespace/my_experiment/common_env/compilers/aarch64-v01c01/aarch64-v01c01-linux-musl-gcc/bin/aarch64-v01c01-linux-musl-"
    ["x86"]=""
)
```

## 3. 设备端（下位机）部署与运行

推送编译产物时，需保持业务运行所需的目录结构。`.sym` 符号表仅保留在宿主机即可，无需推送到设备端。

设备端参考目录结构：

```text
/oem/app/
├── gdbserver          # 调试服务端
├── xs-nvs             # NVS 业务主程序
└── data/
    ├── starter_cfg.json
    └── zlog.conf
```

在设备端启动 gdbserver：

```bash
cd /oem/app/
chmod +x gdbserver xs-nvs

# 启动 gdbserver 挂载业务程序，监听端口设为 2345，IP 指定为 0.0.0.0
./gdbserver 0.0.0.0:2345 ./xs-nvs
```

终端打印 `Listening on port 2345` 即表示启动成功并处于监听状态。

## 4. VSCode 调试配置

在宿主机工程目录的 `.vscode/launch.json` 中添加或覆盖以下配置，将 `miDebuggerPath` 指向本地的物理 GDB 路径：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "NVS 远程单步调试",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/out/bin/xs-nvs",
            "cwd": "${workspaceFolder}",
            
            // 指向本地宿主机实体的交叉 GDB 绝对路径
            "miDebuggerPath": "/home/jfxu/codespace/my_experiment/common_env/compilers/aarch64-v01c01/aarch64-v01c01-linux-musl-gcc/bin/aarch64-v01c01-linux-musl-gdb",
            
            // 设备端 IP 与 gdbserver 监听的端口
            "miDebuggerServerAddress": "192.168.10.200:2345",
            
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                },
                {
                    "description": "忽略目标板共享库报警",
                    "text": "set auto-load safe-path /",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```

## 5. 调试操作流程

1. 确保设备端的 `gdbserver` 已启动并处于监听状态。
2. 在 VSCode 中打开需要调试的源码文件并设置断点。
3. 切换到 VSCode 运行和调试侧边栏，选中 `NVS 远程单步调试` 配置，按下 `F5` 启动调试。
