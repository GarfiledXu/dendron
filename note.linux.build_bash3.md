---
id: pk99f5sc6waq83q9ihggpc6
title: Build_bash3
desc: ''
updated: 1778570281350
created: 1778492259312
---

 C++ 多工程脚手架与编译构建系统指南 

本文档包含了一套完整的跨平台 C++ 多工程脚手架方案。该方案将工作区路由控制（一级大脑）与具体的编译部署执行（二级肌肉）解耦，并将**工程源码、编译缓存、最终产物进行三方物理隔离**，保证工作区根目录的绝对整洁。

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
│           ├── build.sh            
│           └── pushexe.sh          
│
├── prj/                            # ★ 【工程源码区】所有业务代码的集中收纳地
│   ├── vision_nvs/                 # (动态生成) 实际业务工程 A
│   └── motor_ctrl/                 # (动态生成) 实际业务工程 B
│
├── build/                          # (动态生成) 【集中编译缓存区】存放 .o, CMakeCache 等
│   └── aarch64/                    
│       └── Release/                
│           └── vision_nvs/         
│
└── out/                            # (动态生成) 【集中终极产物区】绝对干净
    └── aarch64/
        └── Release/
            └── vision_nvs/         # 只有最终的二进制文件 vision_nvs_bin
```

---

## 二、 核心脚本代码

### 1. 一级路由总管：`ws.sh` (存放于根目录)

**职责：** 动态创建 `prj/` 目录，精准解析带有 `prj/` 前缀的路径，计算上下文。
**赋予执行权限：** `chmod +x ws.sh`

```bash
#!/bin/bash
set -e

WORKSPACE_DIR="$(pwd)"
TEMPLATE_DIR="${WORKSPACE_DIR}/templates"
PRJ_DIR="${WORKSPACE_DIR}/prj"

# 确保工程目录存在
mkdir -p "${PRJ_DIR}"

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
    local target_path="${PRJ_DIR}/${proj_name}"

    if [ ! -d "$tpl_path" ]; then
        echo "❌ 错误: 找不到模板 'tpl_${tpl_id}'"
        exit 1
    fi
    if [ -d "$target_path" ]; then
        echo "❌ 错误: 项目 '${proj_name}' 已存在于 prj/ 目录！"
        exit 1
    fi

    echo "📦 正在基于模板 ${tpl_id} 创建项目: ${proj_name}..."
    cp -r "${tpl_path}" "${target_path}"

    # 替换模板中的 __PROJECT_NAME__ 占位符 (兼容 Linux 和 macOS)
    find "${target_path}" -type f -exec sed -i'' -e "s/__PROJECT_NAME__/${proj_name}/g" {} + 2>/dev/null || \
    find "${target_path}" -type f -exec sed -i '' -e "s/__PROJECT_NAME__/${proj_name}/g" {} +

    chmod +x "${target_path}/scripts/"*.sh
    echo "✅ 项目 ${proj_name} 创建成功！路径: prj/${proj_name}"
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

    # 智能截取项目名：处理纯名称(vision)或VSCode传来的路径(prj/vision/src/main.cpp)
    local proj_name=$(echo "$raw_proj_name" | sed 's|^prj/||' | cut -d'/' -f1)

    local proj_path="${PRJ_DIR}/${proj_name}"
    local script_path="${proj_path}/scripts/${action}.sh"

    if [ ! -f "$script_path" ]; then
        echo "❌ 错误: 找不到二级脚本 '${action}.sh' 于 ${proj_path}/scripts/"
        exit 1
    fi

    # 预扫描参数
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
    
    # 编译区与产物区保持在根目录，按项目名隔离
    export WS_BUILD_DIR="${WORKSPACE_DIR}/build/${target_platform}/${build_type}/${proj_name}"
    export WS_OUT_DIR="${WORKSPACE_DIR}/out/${target_platform}/${build_type}/${proj_name}"
    export WS_ARTIFACT_PATH="${WS_OUT_DIR}/${WS_TARGET_EXE}" 

    echo "🚀 [Router] 项目: ${proj_name} | 动作: ${action} | 平台: ${target_platform}"

    # 跳转到实际工程目录并执行二级脚本
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

# 接收从 ws.sh 注入的环境变量
set(COMMON_ROOT $ENV{WS_COMMON_ROOT})
set(TARGET_EXE_NAME $ENV{WS_TARGET_EXE})
set(FINAL_OUT_DIR $ENV{WS_OUT_DIR})

# 核心魔法：重定向所有最终产物到工作区的 out 目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${FINAL_OUT_DIR}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${FINAL_OUT_DIR}")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${FINAL_OUT_DIR}")

file(GLOB SOURCES "src/*.cpp" "src/*.c")
add_executable(${TARGET_EXE_NAME} ${SOURCES})
```

### 4. 二级编译脚本：`templates/tpl_1_basic/scripts/build.sh`

```bash
#!/bin/bash
set -e

# 接收一级脚本环境变量，本地脱离执行时回退到根目录的 build
PROJ_NAME=${WS_PROJ_NAME:-"Unknown"}
BUILD_DIR=${WS_BUILD_DIR:-"$(pwd)/../../../build"}
OUT_DIR=${WS_OUT_DIR:-"$(pwd)/../../../out"}

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

CMAKE_CMD="cmake -S . -B \"${BUILD_DIR}\" -DCMAKE_BUILD_TYPE=${BUILD_TYPE} -DCMAKE_EXPORT_COMPILE_COMMANDS=ON ${EXTRA_ARGS}"

if [ "$TARGET_PLATFORM" == "aarch64" ]; then
    TOOLCHAIN="${WS_COMMON_ROOT}/toolchains/aarch64_linux.cmake"
    CMAKE_CMD="${CMAKE_CMD} -DCMAKE_TOOLCHAIN_FILE=${TOOLCHAIN}"
fi

eval ${CMAKE_CMD}

echo "🔨 开始编译..."
cmake --build "${BUILD_DIR}" -j$(nproc)

echo "✅ 编译完成！产物已存放至: ${WS_ARTIFACT_PATH:-"目标路径"}"
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
    exit 1
fi

echo "🚀 准备推送到下位机 root@${TARGET_IP}:/tmp/ ..."

scp "$WS_ARTIFACT_PATH" "root@${TARGET_IP}:/tmp/${WS_TARGET_EXE}"
ssh "root@${TARGET_IP}" "chmod +x /tmp/${WS_TARGET_EXE}"

echo "✅ 推送成功！"
```

---

## 四、 VS Code 联动配置文件 (适配 `prj/` 目录)

在工作区根目录下创建 `.vscode/` 文件夹并添加以下配置。

### 1. `tasks.json` (交互式构建与部署)

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
            "name": "🐞 Debug Select Project (本地调试)",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/out/x86_64/Debug/${input:projectName}/${input:projectName}_bin",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}/prj/${input:projectName}",
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
    ],
    "inputs": [
        {
            "id": "projectName",
            "type": "promptString",
            "description": "请输入要调试的工程名 (位于 prj/ 目录下的名称)",
            "default": "vision_nvs"
        }
    ]
}
```

### 3. `c_cpp_properties.json` (智能提示与代码补全)

```json
{
    "configurations": [
        {
            "name": "Linux Workspace",
            "includePath": [
                "${workspaceFolder}/prj/**",
                "${workspaceFolder}/common_env/modules/**"
            ],
            "defines": [
                "DEBUG",
                "_DEBUG"
            ],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "linux-gcc-x64"
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

## 五、 命令行调用示例

所有的命令行操作**必须在工作区根目录**（如 `/my_workspace`）下执行。`ws.sh` 具有智能截取功能，项目名称参数既可以传纯名称 `vision_nvs`，也可以传带相对路径的名称 `prj/vision_nvs`。

### 1. 创建新工程

```bash
# 基于 1_basic 模板，在 prj/ 目录下创建名为 vision_nvs 的工程
./ws.sh create vision_nvs 1_basic
```

*结果：生成 `prj/vision_nvs/` 目录，并将模板内的所有占位符自动替换为 `vision_nvs`。*

### 2. 本地 X86 编译测试 (默认参数)

```bash
# 默认使用 x86_64 平台和 Debug 模式编译
./ws.sh build vision_nvs
```

*结果：*

* 编译缓存位于：`build/x86_64/Debug/vision_nvs/`
* 最终可执行程序位于：`out/x86_64/Debug/vision_nvs/vision_nvs_bin`

### 3. ARM 交叉编译并传入定制 CMake 参数

```bash
# 指定平台为 aarch64，类型为 Release，并临时开启某个宏开关
./ws.sh build prj/vision_nvs -p aarch64 -b Release -c "-DUSE_HW_ACCEL=ON"
```

*结果：使用 `common_env/toolchains/aarch64_linux.cmake` 进行交叉编译，产物纯净地输出到 `out/aarch64/Release/vision_nvs/` 目录。*

### 4. 部署可执行文件到下位机

```bash
# 参数与编译时完全对齐，确保推送的是刚刚编译的那个 ARM Release 版本
./ws.sh pushexe vision_nvs -p aarch64 -b Release -i 192.168.5.50
```

*结果：精准定位到 `out/aarch64/Release/vision_nvs/vision_nvs_bin`，并通过 SCP 传输至 `192.168.5.50` 下位机的 `/tmp/` 目录并赋予执行权限。*

---

## 六、 VS Code 工作流可视化示例

得益于 `.vscode` 目录下的精细配置，在桌面端开发时，你可以完全不碰命令行，享受顶级的图形化交互体验。

**前置要求：** 使用 VS Code 打开整个工作区根目录 `/my_workspace`。

### 场景 1：沉浸式编码与一键编译 (Ctrl+Shift+B)

1. 在侧边栏的 `prj/` 目录下，点开你正在开发的工程文件（例如 `prj/vision_nvs/src/main.cpp`）。
2. 将光标留在代码编辑区，按下快捷键 **`Ctrl + Shift + B`**。
3. 编辑器正上方会依次弹出下拉菜单：
   * **[选择目标编译平台]**: 选择 `x86_64` 或 `aarch64`。
   * **[选择编译类型]**: 选择 `Debug` 或 `Release`。
4. 选择完毕后，底部终端自动弹出并执行对应的编译任务，若代码有语法错误，会在编辑器代码行下自动画出红波浪线。

### 场景 2：一键交叉部署

1. 按下快捷键 **`Ctrl + P`** 打开命令面板。
2. 输入并选择 `task push`（对应 `🚀 Push Executable to Device` 任务）。
3. 依次在下拉菜单中选择平台 (`aarch64`) 和编译类型 (`Release`)。
4. 顶部会弹出一个输入框 `输入下位机目标 IP 地址`（默认已填好 192.168.1.100）。
5. 按下回车，底部终端即刻开始 SSH 传输。

### 场景 3：本地断点调试 (F5)

1. 确保你已经完成了一次本地的 `x86_64` + `Debug` 编译。
2. 在左侧边栏切换到 **"运行和调试"** (Run and Debug) 面板。
3. 顶部下拉框选择 **`🐞 Debug Select Project (本地调试)`**，然后点击绿色的运行按钮（或直接按 `F5`）。
4. 顶部弹框提示 `请输入要调试的工程名`，在此输入框键入 `vision_nvs` 并回车。
5. VS Code 会自动将工作目录切换到 `prj/vision_nvs/`，并挂载 `out/` 目录下的可执行文件，直接停在你打好的代码断点处。
