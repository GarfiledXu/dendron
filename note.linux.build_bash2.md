---
id: iapvtwvu9m7hmpd9r6cijf8
title: Build_bash2
desc: ''
updated: 1778491627005
created: 1778491625637
---

# C++ 多工程脚手架与编译构建系统指南

本文档包含了一套完整的跨平台 C++ 多工程脚手架（Workspace Scaffolding & Task Runner）落地代码与配置。该方案将工作区路由控制（一级大脑）与具体的编译部署执行（二级肌肉）解耦，并将中间编译缓存与最终产物彻底物理隔离。

---

## 一、 工作区目录结构约定

在工作区根目录（如 `/my_workspace`）下，遵循以下结构：

```text
/my_workspace                       # 终端始终停留在此根目录进行所有操作
├── ws.sh                           # 【一级核心脚本】脚手架生成与命令路由总管
├── common_env/                     # 【公共资产区】
│   ├── modules/                    # 自定义的可复用模块 (如 ocr_algo, sys_utils)
│   └── toolchains/                 # 存放交叉编译工具链 (如 aarch64_linux.cmake)
├── templates/                      # 【模板仓库区】
│   └── tpl_1_basic/                # 基础工程模板 (由此生成新工程)
│       ├── CMakeLists.txt          # 包含 __PROJECT_NAME__ 占位符
│       ├── src/
│       │   └── main.cpp
│       └── scripts/                # 二级脚本模板
│           ├── build.sh            # 负责配置和编译
│           └── pushexe.sh          # 负责 SSH 传输部署
│
├── vision_nvs/                     # (动态生成) 实际业务工程示例
├── out/                            # (动态生成) 【集中终极产物区】绝对干净
└── build/                          # (动态生成) 【集中编译缓存区】存放 .o, CMakeCache 等
```

---

## 二、 核心脚本代码

### 1. 一级路由总管：`ws.sh` (存放于根目录)
**赋予执行权限：** `chmod +x ws.sh`

```bash
#!/bin/bash
set -e

WORKSPACE_DIR="$(pwd)"
TEMPLATE_DIR="${WORKSPACE_DIR}/templates"

# ==========================================
# 1. 业务逻辑：创建新项目
# ==========================================
create_project() {
    local proj_name=$1
    local tpl_id=$2

    if [ -z "$proj_name" ] || [ -z "$tpl_id" ]; then
        echo "用法: ./ws.sh create <project_name> <template_id>"
        exit 1
    fi

    local tpl_path="${TEMPLATE_DIR}/tpl_${tpl_id}"
    local target_path="${WORKSPACE_DIR}/${proj_name}"

    if [ ! -d "$tpl_path" ]; then
        echo "❌ 错误: 找不到模板 'tpl_${tpl_id}'"
        exit 1
    fi
    if [ -d "$target_path" ]; then
        echo "❌ 错误: 项目 '${proj_name}' 已存在！"
        exit 1
    fi

    echo "📦 正在基于模板 ${tpl_id} 创建项目: ${proj_name}..."
    cp -r "${tpl_path}" "${target_path}"

    # 替换模板中的 __PROJECT_NAME__ 占位符 (兼容 Linux 和 macOS)
    find "${target_path}" -type f -exec sed -i'' -e "s/__PROJECT_NAME__/${proj_name}/g" {} + 2>/dev/null || \
    find "${target_path}" -type f -exec sed -i '' -e "s/__PROJECT_NAME__/${proj_name}/g" {} +

    chmod +x "${target_path}/scripts/"*.sh
    echo "✅ 项目 ${proj_name} 创建成功！"
}

# ==========================================
# 2. 业务逻辑：路由命令并注入上下文
# ==========================================
dispatch_task() {
    local action=$1      
    local raw_proj_name=$2   
    shift 2              

    if [ -z "$raw_proj_name" ]; then
        echo "❌ 错误: 必须指定项目名。用法: ./ws.sh $action <project_name> [args...]"
        exit 1
    fi

    # 智能截取项目名 (兼容 VSCode 传入的长路径，如 vision_nvs/src/main.cpp)
    local proj_name=$(echo "$raw_proj_name" | cut -d'/' -f1)

    local proj_path="${WORKSPACE_DIR}/${proj_name}"
    local script_path="${proj_path}/scripts/${action}.sh"

    if [ ! -f "$script_path" ]; then
        echo "❌ 错误: 找不到二级脚本 '${action}.sh' 于 ${proj_path}/scripts/"
        exit 1
    fi

    # 预扫描参数 (嗅探平台和类型，不吃掉参数)
    local target_platform="x86_64"
    local build_type="Debug"
    local OPTIND_OLD=$OPTIND
    OPTIND=1
    while getopts "p:b:i:c:h" opt "$@"; do
        case $opt in
            p) target_platform=$OPTARG ;;
            b) build_type=$OPTARG ;;
        esac
    done
    OPTIND=$OPTIND_OLD

    # 隐式注入环境变量 (上下文计算)
    export WS_COMMON_ROOT="${WORKSPACE_DIR}/common_env"
    export WS_PROJ_NAME="${proj_name}"
    export WS_TARGET_EXE="${proj_name}_bin"
    
    export WS_BUILD_DIR="${WORKSPACE_DIR}/build/${target_platform}/${build_type}/${proj_name}"
    export WS_OUT_DIR="${WORKSPACE_DIR}/out/${target_platform}/${build_type}/${proj_name}"
    export WS_ARTIFACT_PATH="${WS_OUT_DIR}/${WS_TARGET_EXE}" 

    echo "🚀 [Router] 项目: ${proj_name} | 动作: ${action} | 平台: ${target_platform}"

    # 跳转并透传所有剩余参数执行
    cd "${proj_path}"
    bash "scripts/${action}.sh" "$@"
}

# ==========================================
# 入口解析
# ==========================================
COMMAND=$1
case $COMMAND in
    create) shift; create_project "$1" "$2" ;;
    build|pushexe|clean) dispatch_task "$@" ;;
    *)
        echo "一级总管脚本说明:"
        echo "  创建: ./ws.sh create <proj_name> <tpl_id>"
        echo "  编译: ./ws.sh build <proj_name> [-p platform] [-b build_type] [-c 'cmake_args']"
        echo "  推送: ./ws.sh pushexe <proj_name> [-p platform] [-b build_type] [-i ip_address]"
        exit 1
        ;;
esac
```

---

### 3. 模板文件：`templates/tpl_1_basic/CMakeLists.txt`

```cmake
cmake_minimum_required(VERSION 3.15)
project(__PROJECT_NAME__)

# 1. 接收从 ws.sh 注入的环境变量
set(COMMON_ROOT $ENV{WS_COMMON_ROOT})
set(TARGET_EXE_NAME $ENV{WS_TARGET_EXE})
set(FINAL_OUT_DIR $ENV{WS_OUT_DIR})

# 2. 核心魔法：重定向所有最终产物到工作区的 out 目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${FINAL_OUT_DIR}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${FINAL_OUT_DIR}")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${FINAL_OUT_DIR}")

# 3. 源码与目标编译
file(GLOB SOURCES "src/*.cpp" "src/*.c")
add_executable(${TARGET_EXE_NAME} ${SOURCES})

# 示例：引入公共模块
# include_directories(${COMMON_ROOT}/modules/sys_utils/include)
# target_link_libraries(${TARGET_EXE_NAME} PRIVATE sys_utils)
```

### 4. 二级编译脚本：`templates/tpl_1_basic/scripts/build.sh`

```bash
#!/bin/bash
set -e

# 接收一级脚本计算好的环境变量，并提供本地执行时的回退默认值
PROJ_NAME=${WS_PROJ_NAME:-"Unknown"}
BUILD_DIR=${WS_BUILD_DIR:-"$(pwd)/../build"}
OUT_DIR=${WS_OUT_DIR:-"$(pwd)/../out"}

TARGET_PLATFORM="x86_64"
BUILD_TYPE="Debug"
EXTRA_ARGS=""

while getopts "p:b:c:h" opt; do
  case ${opt} in
    p) TARGET_PLATFORM=$OPTARG ;;
    b) BUILD_TYPE=$OPTARG ;;
    c) EXTRA_ARGS=$OPTARG ;;
    h) echo "用法: -p <平台> -b <类型> -c <CMake参数>"; exit 0 ;;
    \?) exit 1 ;;
  esac
done

echo "🔧 配置目录: ${BUILD_DIR}"
echo "📦 产物重定向: ${OUT_DIR}"

# 执行 CMake，导出 compile_commands.json 供 VSCode IntelliSense 使用
CMAKE_CMD="cmake -S . -B \"${BUILD_DIR}\" -DCMAKE_BUILD_TYPE=${BUILD_TYPE} -DCMAKE_EXPORT_COMPILE_COMMANDS=ON ${EXTRA_ARGS}"

# 若交叉编译，追加工具链参数
if [ "$TARGET_PLATFORM" == "aarch64" ]; then
    TOOLCHAIN="${WS_COMMON_ROOT}/toolchains/aarch64_linux.cmake"
    CMAKE_CMD="${CMAKE_CMD} -DCMAKE_TOOLCHAIN_FILE=${TOOLCHAIN}"
fi

eval ${CMAKE_CMD}

echo "🔨 开始编译..."
cmake --build "${BUILD_DIR}" -j$(nproc)

echo "✅ 编译完成！产物已存放至: ${WS_ARTIFACT_PATH:-"自定义产物路径"}"
```

### 5. 二级部署脚本：`templates/tpl_1_basic/scripts/pushexe.sh`

```bash
#!/bin/bash
set -e

TARGET_IP="192.168.1.100"

while getopts "p:b:i:h" opt; do
  case ${opt} in
    i) TARGET_IP=$OPTARG ;;
    h) echo "用法: ./ws.sh pushexe <proj> [-i 目标IP]"; exit 0 ;;
  esac
done

if [ ! -f "$WS_ARTIFACT_PATH" ]; then
    echo "❌ 错误: 找不到产物文件: ${WS_ARTIFACT_PATH}"
    echo "💡 请先执行 build 操作."
    exit 1
fi

echo "🚀 准备推送到下位机 root@${TARGET_IP}:/tmp/ ..."

scp "$WS_ARTIFACT_PATH" "root@${TARGET_IP}:/tmp/${WS_TARGET_EXE}"
ssh "root@${TARGET_IP}" "chmod +x /tmp/${WS_TARGET_EXE}"

echo "✅ 推送成功！"
```

---

## 三、 VS Code 联动配置文件

在根目录下创建 `.vscode/` 文件夹，并添加入下文件。

### 1. `tasks.json` (交互式任务驱动)

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "🔨 Build Active Project (当前聚焦的工程)",
            "type": "shell",
            "command": "./ws.sh",
            "args": [
                "build",
                "${relativeFile}",
                "-p", "${input:platform}",
                "-b", "${input:buildType}"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": false,
                "panel": "shared",
                "clear": true
            },
            "problemMatcher": ["$gcc"]
        },
        {
            "label": "🚀 Push Executable to Device (部署当前工程)",
            "type": "shell",
            "command": "./ws.sh",
            "args": [
                "pushexe",
                "${relativeFile}",
                "-p", "${input:platform}",
                "-b", "${input:buildType}",
                "-i", "${input:targetIp}"
            ],
            "group": "build",
            "problemMatcher": []
        }
    ],
    "inputs": [
        {
            "id": "platform",
            "type": "pickString",
            "description": "选择目标编译平台",
            "options": ["x86_64", "aarch64"],
            "default": "x86_64"
        },
        {
            "id": "buildType",
            "type": "pickString",
            "description": "选择编译类型",
            "options": ["Debug", "Release"],
            "default": "Debug"
        },
        {
            "id": "targetIp",
            "type": "promptString",
            "description": "输入下位机目标 IP 地址",
            "default": "192.168.1.100"
        }
    ]
}
```

### 2. `launch.json` (一键本地断点调试)

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "🐞 Debug Active Project (本地调试)",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/out/x86_64/Debug/${fileDirnameBasename}/${fileDirnameBasename}_bin",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}/${fileDirnameBasename}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "🔨 Build Active Project (当前聚焦的工程)" 
        }
    ]
}
```

### 3. `c_cpp_properties.json` (精准代码跳转与补全)

```json
{
    "configurations": [
        {
            "name": "Linux Workspace",
            "includePath": [
                "${workspaceFolder}/**",
                "${workspaceFolder}/common_env/modules/**"
            ],
            "defines": [
                "DEBUG",
                "_DEBUG"
            ],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "linux-gcc-x64",
            "compileCommands": "${workspaceFolder}/build/x86_64/Debug/${fileDirnameBasename}/compile_commands.json"
        }
    ],
    "version": 4
}
```

### 4. `settings.json` (工作区视图优化)

```json
{
    "C_Cpp.formatting": "clangFormat",
    "editor.formatOnSave": true,
    "files.exclude": {
        "**/.git": true,
        "**/build/**": true
    },
    "search.exclude": {
        "**/build/**": true,
        "**/out/**": true
    },
    "terminal.integrated.cwd": "${workspaceFolder}"
}
```

````</CMake参数>