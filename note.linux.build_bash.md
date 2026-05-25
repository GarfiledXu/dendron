---
id: ewd52o99hnywocus0j2pum9
title: Build_bash
desc: ''
updated: 1778491749549
created: 1778485435869
---

## 架构初期设想

一级路由调度 + 二级按需定制

整个脚手架体系包含以下核心机制：

单点入口 (Single Entry Point)

所有操作都在工作区根目录执行，不再需要在各个项目间来回 cd。

一级调度脚本 (Level-1 Router Script)

作为工作区总管（例如命名为 ws.sh 或 manager.sh）。

职责一：工程脚手架 (Scaffolding)。 根据指定的模板编号生成新项目，动态替换项目名，甚至根据传入的模块列表动态修改新项目的 CMakeLists.txt。

职责二：任务路由 (Task Routing)。 接收 <任务名> <工程名>（如 build proj_A），隐式注入工程环境变量（如产物默认名称），并分发给对应工程的二级脚本执行。

二级执行脚本 (Level-2 Actor Script)

存放在具体工程内部（基于模板生成），通过名称严格约定其行为（如 build.sh 负责构建，run.sh 负责执行或部署，clean.sh 负责清理）。

负责处理项目高度定制化的构建或链接逻辑。

隐式与显式约定

一级脚本通过项目名 ProjA，隐式约定最终的执行程序名为 ProjA_bin。二级脚本默认使用该变量，但允许在其内部强行覆盖。

### 目录结构

```bash
/my_workspace                       # 终端始终停留在此根目录进行所有操作
├── ws.sh                           # 【一级核心脚本】脚手架生成与命令路由总管
├── common_env/                     # 【公共资产区】
│   ├── modules/                    # 自定义的可复用 CMake 模块 / 算法接口等
│   └── toolchains/                 # 存放交叉编译工具链 (如 aarch64_linux.cmake)
├── templates/                      # 【模板仓库区】
│   └── tpl_1_basic/                # 基础工程模板 (由此生成新工程)
│       ├── CMakeLists.txt          # 包含 __PROJECT_NAME__ 和环境变量接收逻辑
│       ├── src/
│       │   └── main.cpp
│       └── scripts/                # 二级脚本模板
│           ├── build.sh            # 负责配置和编译
│           └── pushexe.sh          # 负责 SSH 传输部署
│
├── vision_nvs/                     # (动态生成) 你的实际业务工程A
├── motor_ctrl/                     # (动态生成) 你的实际业务工程B
│
├── build/                          # (动态生成) 【集中编译缓存区】存放 .o, CMakeCache 等
│   └── aarch64/                    # 按平台隔离
│       └── Release/                # 按编译类型隔离
│           └── vision_nvs/         # 具体工程的构建过程文件
│
└── out/                            # (动态生成) 【集中终极产物区】绝对干净
    └── aarch64/
        └── Release/
            └── vision_nvs/         # 只有最终的二进制文件 vision_nvs_bin
```

二、 核心脚本内容
请依次创建上述目录结构，并填入以下代码。

1. 一级路由总管：ws.sh (位于根目录)
职责： 解析参数，生成脚手架，预计算绝对路径并隐式注入给二级脚本。

### 一级脚本

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

    # ==========================================
    # ★ VS Code 联动升级：智能截取项目名
    # 如果传来的是 vision_nvs/src/main.cpp，自动截取出第一级目录 vision_nvs
    # 如果传来的是单纯的 vision_nvs，依然保持 vision_nvs 不变
    # ==========================================
    local proj_name=$(echo "$raw_proj_name" | cut -d'/' -f1)

    local proj_path="${WORKSPACE_DIR}/${proj_name}"
    local script_path="${proj_path}/scripts/${action}.sh"

    if [ ! -f "$script_path" ]; then
        echo "❌ 错误: 找不到二级脚本 '${action}.sh' 于 ${proj_path}/scripts/"
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

    # 替换模板中的 __PROJECT_NAME__ 占位符
    find "${target_path}" -type f -exec sed -i'' -e "s/__PROJECT_NAME__/${proj_name}/g" {} + 2>/dev/null || \
    find "${target_path}" -type f -exec sed -i '' -e "s/__PROJECT_NAME__/${proj_name}/g" {} +

    chmod +x "${target_path}/scripts/"*.sh
    echo "✅ 项目 ${proj_name} 创建成功！"
}

# ==========================================
# 2. 业务逻辑：路由命令并注入上下文
# ==========================================
dispatch_task() {
    local action=$1      # 行为: build, pushexe 等
    local proj_name=$2   # 工程名: vision_nvs 等
    shift 2              # 移除前两个参数，保留后面的参数给二级脚本

    if [ -z "$proj_name" ]; then
        echo "❌ 错误: 必须指定项目名。用法: ./ws.sh $action <project_name> [args...]"
        exit 1
    fi

    local proj_path="${WORKSPACE_DIR}/${proj_name}"
    local script_path="${proj_path}/scripts/${action}.sh"

    if [ ! -f "$script_path" ]; then
        echo "❌ 错误: 找不到二级脚本 '${action}.sh' 于 ${proj_path}/scripts/"
        exit 1
    fi

    # --- 预扫描参数 (嗅探平台和类型，不吃掉参数) ---
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

    # --- 隐式注入环境变量 (上下文计算) ---
    export WS_COMMON_ROOT="${WORKSPACE_DIR}/common_env"
    export WS_PROJ_NAME="${proj_name}"
    export WS_TARGET_EXE="${proj_name}_bin"
    
    # 精确组装出 Build 缓存区和 Out 产物区的绝对路径
    export WS_BUILD_DIR="${WORKSPACE_DIR}/build/${target_platform}/${build_type}/${proj_name}"
    export WS_OUT_DIR="${WORKSPACE_DIR}/out/${target_platform}/${build_type}/${proj_name}"
    
    # 终极产物文件绝对路径
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

2. CMake 模板：templates/tpl_1_basic/CMakeLists.txt
职责： 接收环境变量，挂载外部模块，重定向输出产物。

### cmake 模板

```bash
cmake_minimum_required(VERSION 3.15)
project(__PROJECT_NAME__)

# 1. 接收从 ws.sh 注入的环境变量
set(COMMON_ROOT $ENV{WS_COMMON_ROOT})
set(TARGET_EXE_NAME $ENV{WS_TARGET_EXE})
set(FINAL_OUT_DIR $ENV{WS_OUT_DIR})

# 2. ★ 核心魔法：重定向所有最终产物到工作区的 out 目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${FINAL_OUT_DIR}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${FINAL_OUT_DIR}")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${FINAL_OUT_DIR}")

# 3. 如果是交叉编译，引入对应工具链 (可根据传入的平台变量动态设置)
# 这里仅作演示，实际也可通过命令行的 -DCMAKE_TOOLCHAIN_FILE 传入

file(GLOB SOURCES "src/*.cpp" "src/*.c")
add_executable(${TARGET_EXE_NAME} ${SOURCES})
```

### 二级编译脚本

3. 二级编译脚本：templates/tpl_1_basic/scripts/build.sh
职责： 读取参数，配置 CMake，执行构建。不需要处理复杂的路径拼接。


```bash
#!/bin/bash
set -e

# 二级脚本直接享用一级脚本计算好的环境变量
PROJ_NAME=${WS_PROJ_NAME:-"Unknown"}
BUILD_DIR=${WS_BUILD_DIR:-"$(pwd)/build"}
OUT_DIR=${WS_OUT_DIR:-"$(pwd)/out"}

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

# 执行 CMake，缓存放入 BUILD_DIR
CMAKE_CMD="cmake -S . -B \"${BUILD_DIR}\" -DCMAKE_BUILD_TYPE=${BUILD_TYPE} ${EXTRA_ARGS}"

# 若交叉编译，追加工具链参数
if [ "$TARGET_PLATFORM" == "aarch64" ]; then
    TOOLCHAIN="${WS_COMMON_ROOT}/toolchains/aarch64_linux.cmake"
    CMAKE_CMD="${CMAKE_CMD} -DCMAKE_TOOLCHAIN_FILE=${TOOLCHAIN}"
fi

eval ${CMAKE_CMD}

# 充分利用多核编译
echo "🔨 开始编译..."
cmake --build "${BUILD_DIR}" -j$(nproc)

echo "✅ 编译完成！产物已存放至: ${WS_ARTIFACT_PATH}"
```

### 二级工具脚本

4. 二级推送脚本：templates/tpl_1_basic/scripts/pushexe.sh
职责： 直接拿 $WS_ARTIFACT_PATH 办事，完成部署和传输。


```bash
#!/bin/bash
set -e

TARGET_IP="192.168.1.100" # 默认IP

# 解析参数 (忽略 p 和 b，专门解析 i)
while getopts "p:b:i:h" opt; do
  case ${opt} in
    i) TARGET_IP=$OPTARG ;;
    h) echo "用法: ./ws.sh pushexe <proj> [-i 目标IP]"; exit 0 ;;
  esac
done

# 校验一级脚本注入的终极产物文件是否存在
if [ ! -f "$WS_ARTIFACT_PATH" ]; then
    echo "❌ 错误: 找不到产物文件:"
    echo "   ${WS_ARTIFACT_PATH}"
    echo "💡 请先执行 build 操作."
    exit 1
fi

echo "🚀 准备推送到下位机 root@${TARGET_IP}:/tmp/ ..."

# 实际的 scp 命令
scp "$WS_ARTIFACT_PATH" "root@${TARGET_IP}:/tmp/${WS_TARGET_EXE}"
ssh "root@${TARGET_IP}" "chmod +x /tmp/${WS_TARGET_EXE}"

echo "✅ 推送成功！可执行文件: /tmp/${WS_TARGET_EXE}"
```

### vscode 工作流配置

工作区根目录 /my_workspace 下创建 .vscode/tasks.json
你打开 VS Code 的顶级文件夹必须是 /my_workspace。接下来的体验将是“傻瓜式”的顺滑：

1. 沉浸式编码与一键编译

你在修改 vision_nvs/src/algo.cpp 的代码。

按下快捷键 Ctrl + Shift + B。

屏幕上方立刻弹出下拉菜单：选择目标编译平台 (有 x86_64 和 aarch64 可选)。你点击 aarch64。

接着弹出 选择编译类型。你点击 Release。

黑魔法发生： VS Code 组装好命令 ./ws.sh build vision_nvs/src/algo.cpp -p aarch64 -b Release 扔给终端。ws.sh 自动把路径截断为 vision_nvs，计算出正确的交叉编译环境变量，然后进入工程内调用 build.sh。

终端瞬间输出绿色的 ✅ 编译完成！产物已存放至: /my_workspace/out/aarch64/Release/vision_nvs/vision_nvs_bin。如果有语法错误，VS Code 的 problemMatcher 会直接在你的编辑器对应代码行标上红波浪线。

2. 无缝交叉部署

按下 Ctrl + P，输入 task push，选择你定义的推送任务。

选平台、选类型、弹出输入框确认 IP（默认已填好 192.168.1.100，直接回车）。

脚本立刻根据你刚才选的参数，找到对应路径下的 bin 文件，通过 SSH 发送给下位机

```bash
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "🔨 Build Active Project (当前聚焦的工程)",
            "type": "shell",
            "command": "./ws.sh",
            "args": [
                "build",
                "${relativeFile}",         // VSCode会传入类似 "vision_nvs/src/main.cpp"
                "-p", "${input:platform}", // 触发平台选择下拉菜单
                "-b", "${input:buildType}" // 触发类型选择下拉菜单
            ],
            "group": {
                "kind": "build",
                "isDefault": true          // 设为默认编译任务，Ctrl+Shift+B 直接触发
            },
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": false,
                "panel": "shared",
                "clear": true              // 每次编译前自动清空终端
            },
            "problemMatcher": ["$gcc"]     // 自动捕获 GCC/Clang 的错误并在代码中划红线
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
                "-i", "${input:targetIp}"  // 触发输入框输入IP
            ],
            "group": "build",
            "problemMatcher": []
        }
    ],
    "inputs": [
        {
            "id": "platform",
            "type": "pickString",
            "description": "选择目标编译平台 (Target Platform)",
            "options": [
                "x86_64",
                "aarch64"
            ],
            "default": "x86_64"
        },
        {
            "id": "buildType",
            "type": "pickString",
            "description": "选择编译类型 (Build Type)",
            "options": [
                "Debug",
                "Release"
            ],
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


### 调用示例

```bash
#创建工程
./ws.sh create vision_nvs 1_basic
# 默认编译（本地 X86 调试）
./ws.sh build vision_nvs
# 交叉编译 ARM Release 版本并附加特殊宏
./ws.sh build vision_nvs -p aarch64 -b Release -c "-DUSE_HW_ACCEL=ON"
# 把刚才编译好的 ARM 产物推送到下位机测试
./ws.sh pushexe vision_nvs -p aarch64 -b Release -i 192.168.5.50

```

