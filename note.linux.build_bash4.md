---
id: zrhllx4zbjt61it7al314ep
title: Build_bash4
desc: ''
updated: 1778570501758
created: 1778570499370
---

# C++ 多工程脚手架与编译构建系统指南

本文档介绍一套跨平台 C++ 多工程脚手架方案。该方案基于**“约定优于配置” (Convention over Configuration)** 的原则，将环境配置、调度逻辑与业务执行解耦，实现工程源码、编译缓存、最终产物的三方物理隔离，并规范了多任务调度的日志输出标准。

---

## 一、 工作区目录结构约定

在工作区根目录（如 `/my_workspace`）下，遵循以下结构规范：

```text
/my_workspace                       # 工作区根目录，所有命令的执行入口
├── env.sh                          # 【数据配置层】存放工具链路径映射与平台别名
├── ws.sh                           # 【调度引擎层】负责参数解析、环境变量注入与任务分发
├── common_env/                     # 【公共资产区】
│   ├── modules/                    # 自定义的可复用模块 (如 ocr_algo, sys_utils)
│   └── toolchains/                 # 存放交叉编译工具链 (如 aarch64_linux.cmake)
├── templates/                      # 【模板仓库区】
│   └── tpl_1_basic/                # 基础工程模板
│       ├── CMakeLists.txt          # 包含占位符的构建脚本
│       └── scripts/                # 二级脚本模板 (build.sh, pushexe.sh, clean.sh)
│
├── prj/                            # 【工程源码区】存放具体业务代码
│   ├── helloworld/                 # (动态生成) 业务工程 A
│   └── vision_nvs/                 # (动态生成) 业务工程 B
│
├── build/                          # (动态生成) 【集中编译缓存区】
│   └── helloworld/                 # └─ 按项目名隔离
│       └── arm_3519/               #    └─ 按目标平台隔离
│           └── Debug/              #       └─ 按构建类型隔离
│
└── out/                            # (动态生成) 【集中终极产物区】
    └── helloworld/
        └── arm_3519/
            └── Debug/
                └── helloworld      # 最终可执行二进制文件
```

---

## 二、 系统架构与处理规则

系统由三部分构成：`env.sh`（配置）、`ws.sh`（调度分发）、`prj/.../scripts/*.sh`（业务执行）。其中包含以下核心处理规则：

### 1. 目录名即主键 (Dirname-Driven)
**规则**：`prj/` 下的工程目录名称（如 `helloworld`）即为主键。
`ws.sh` 自动提取该名称并导出为环境变量 `${WS_PROJ_NAME}` 与 `${WS_TARGET_EXE}`。项目级构建脚本（CMake）直接读取该变量作为 target 名称，无需硬编码。

### 2. 路径自动对齐 (Path Alignment)
系统根据当前任务的 `项目名` + `平台名` + `构建类型`，自动拼接生成绝对路径，并导出 `${WS_BUILD_DIR}` 和 `${WS_OUT_DIR}`。二级业务脚本在执行 `cmake` 时直接引用这些变量，确保中间文件和最终产物准确归档到根目录下的 `build/` 和 `out/` 对应子目录中。

### 3. 平台感知与过滤 (Platform Awareness)
系统支持多动作与多平台的组合调用。当任务分发至二级脚本（如 `pushexe.sh`）时，脚本内部基于传入的 `-p` 参数进行平台类型校验。对于不符合条件的平台（如向 x86 本地环境推送固件），脚本会触发跳过（Skip）逻辑并正常退出。

---

## 三、 日志规范与输出风格

系统统一管控制台输出，通过特定的前缀和颜色区分日志层级，降低信息干扰：

| 视觉元素 | 表现形式 | 核心作用 |
| :--- | :--- | :--- |
| **任务清单** | 紫色边框 + 黄色标签 | **全局预览**：任务启动前，打印当前将要执行的动作矩阵与平台组合。 |
| **动作分界** | 青色框线 (Box-drawing) | **模块划分**：标识当前正在执行的具体子任务（如 `[BUILD] ON [ARM]`）。 |
| **常规日志** | 灰色字体 (`0;90m`) | **视觉降噪**：时间戳、`[INFO]` 标签及内部命令输出默认采用灰色，作为背景信息。 |
| **状态标识** | 绿色 `[SUCC]` / 红色 `[ERR]` | **结果聚焦**：高亮标识单步任务的成功或失败状态。 |

---

## 四、 核心脚本代码参考

### 1. 配置中心：`env.sh`
```bash
#!/bin/bash
# 仅存放业务资产路径和平台映射数据

export WS_TC_SEARCH_PATH="${WORKSPACE_DIR}/common_env/toolchains"

declare -A WS_PLATFORM_MAP=(
    ["arm_3519"]="aarch64-v01c01-linux-musl.toolchain.cmake"
    ["x86"]="none"
)

resolve_toolchain_path() {
    local p_in=$1
    local tc_val=${WS_PLATFORM_MAP[$p_in]}
    if [ -n "$tc_val" ] && [ "$tc_val" != "none" ]; then
        echo "${WS_TC_SEARCH_PATH}/${tc_val}"
    fi
}
```

### 2. 二级编译脚本示例：`prj/helloworld/scripts/build.sh`
```bash
#!/bin/bash
set -e
# 日志函数 (log_info等) 与 WS_ 变量由 ws.sh 父进程提供

BUILD_TYPE="Debug"
while getopts "p:b:c:h" opt; do
  case ${opt} in
    b) BUILD_TYPE=$OPTARG ;;
  esac
done

log_info "Preparing CMake configuration..."

CM_CMD="cmake -S . -B \"${WS_BUILD_DIR}\" -DCMAKE_BUILD_TYPE=${BUILD_TYPE} -DCMAKE_EXPORT_COMPILE_COMMANDS=ON"
if [ -n "$WS_TOOLCHAIN_FILE" ]; then
    CM_CMD="${CM_CMD} -DCMAKE_TOOLCHAIN_FILE=${WS_TOOLCHAIN_FILE}"
fi

eval ${CM_CMD} > /dev/null
log_info "Invoking build system..."
cmake --build "${WS_BUILD_DIR}" -j$(nproc)

log_succ "Build completed: ${WS_ARTIFACT_PATH}"
```

### 3. CMakeLists.txt 基础配置示例
```cmake
cmake_minimum_required(VERSION 3.15)

set(TARGET_NAME $ENV{WS_TARGET_EXE})
if(NOT TARGET_NAME)
    set(TARGET_NAME ${PROJECT_NAME})
endif()

project(${TARGET_NAME} LANGUAGES CXX)

file(GLOB_RECURSE SOURCES "src/*.cpp")
add_executable(${TARGET_NAME} ${SOURCES})

if(DEFINED ENV{WS_OUT_DIR})
    set_target_properties(${TARGET_NAME} PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY "$ENV{WS_OUT_DIR}"
    )
endif()
```

---

## 五、 命令行工作流示例

命令需在工作区根目录执行，支持动作与平台的组合调用。

### 1. 标准单任务编译
```bash
./ws.sh build helloworld -p arm_3519 -b Release
```

### 2. 多任务组合流水线
```bash
./ws.sh clean,build,pushexe helloworld -p x86,arm_3519 -b Debug
```
*执行流程：*
1. 打印包含 6 个子任务（2 平台 x 3 动作）的清单。
2. 遍历执行 `clean` (x86) 和 `clean` (arm)。
3. 执行 `build` (x86)，生成本地可执行文件。
4. 执行 `build` (arm)，加载交叉编译工具链并生成对应平台文件。
5. 执行 `pushexe` (x86)，脚本内部匹配到本地平台，跳过推送。
6. 执行 `pushexe` (arm)，通过 SFTP 将产物传输至下位机并执行 MD5 校验。

---

## 六、 VS Code 联动配置 (.vscode/)

工作区结合 VS Code 的 task 和 launch 配置，可实现代码编辑与脚本系统的对接。

### 1. tasks.json (自动识别当前工程并编译)
利用 `${relativeFile}` 传递当前活动文件路径，交由 `ws.sh` 解析提取工程名。
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Build Active Project",
            "type": "shell",
            "command": "./ws.sh",
            "args": [
                "build",
                "${relativeFile}", 
                "-p", "${input:platform}",
                "-b", "${input:buildType}"
            ],
            "group": {"kind": "build", "isDefault": true},
            "presentation": { "echo": true, "reveal": "always", "panel": "shared" }
        }
    ],
    "inputs": [
        { "id": "platform", "type": "pickString", "options": ["x86", "arm_3519"], "default": "x86" },
        { "id": "buildType", "type": "pickString", "options": ["Debug", "Release"], "default": "Debug" }
    ]
}
```

### 2. launch.json (本地调试配置)
指向 `out/` 目录中按规则生成的产物路径进行调试挂载。
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug x86 Project",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/out/${input:projectName}/x86/Debug/${input:projectName}",
            "cwd": "${workspaceFolder}/prj/${input:projectName}",
            "MIMode": "gdb",
            "preLaunchTask": "Build Active Project"
        }
    ],
    "inputs": [
        { "id": "projectName", "type": "promptString", "description": "输入工程名 (如 helloworld)" }
    ]
}
```

> 说明：该方案规范了多工程环境下的路径与跨平台编译行为，使开发者能够将重心保留在 `prj/` 目录下的具体业务开发中。