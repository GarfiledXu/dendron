---
id: 1bu3y8x100fibv6ffi32ffc
title: 脚本内容
desc: ''
updated: 1778571113694
created: 1778570592639
---

# 项目导出

**文件数量**: 7  
**总大小**: 10.9 KB  
**Token 数量**: 2.7K  
**生成时间**: 2026/5/12 15:22:49

## 文件结构

```
📁 .
  📁 templates
    📁 cpp_basic
      📁 scripts
        📄 build.sh
        📄 clean.sh
        📄 push.sh
      📁 src
        📄 main.cpp
      📄 CMakeLists.txt
  📄 env.sh
  📄 ws.sh
```

## 源文件

### templates/cpp_basic/scripts/build.sh

*大小: 805 B | Token: 214*

```bash
#!/bin/bash
set -e
# 必须再次source以识别log函数
source "${WORKSPACE_DIR}/env.sh"

BUILD_TYPE="Debug"
EXTRA_CM_ARGS=""

while getopts "p:b:c:h" opt; do
  case ${opt} in
    b) BUILD_TYPE=$OPTARG ;;
    c) EXTRA_CM_ARGS=$OPTARG ;;
  esac
done

log_info "Starting CMake Configuration..."
[ -n "$WS_TOOLCHAIN_FILE" ] && log_info "Using Toolchain: $(basename $WS_TOOLCHAIN_FILE)"

# 构造CMake命令
CM_CMD="cmake -S . -B \"${WS_BUILD_DIR}\" \
        -DCMAKE_BUILD_TYPE=${BUILD_TYPE} \
        -DCMAKE_EXPORT_COMPILE_COMMANDS=ON"

if [ -n "$WS_TOOLCHAIN_FILE" ]; then
    CM_CMD="${CM_CMD} -DCMAKE_TOOLCHAIN_FILE=${WS_TOOLCHAIN_FILE}"
fi

# 执行
eval ${CM_CMD}
log_info "Starting Compilation..."
cmake --build "${WS_BUILD_DIR}" -j$(nproc)

log_succ "Build Finished. Artifact: ${WS_ARTIFACT_PATH}"
```

### templates/cpp_basic/scripts/clean.sh

*大小: 315 B | Token: 78*

```bash
#!/bin/bash
source "${WORKSPACE_DIR}/env.sh"

log_info "Cleaning project: ${WS_PROJ_NAME}..."
log_info "Removing build dir: ${WS_BUILD_DIR}"
log_info "Removing out dir: ${WS_OUT_DIR}"

# 真正的删除操作（取消注释即可生效）
rm -rf "${WS_BUILD_DIR}"
rm -rf "${WS_OUT_DIR}"

log_succ "Clean COMPLETED."
```

### templates/cpp_basic/scripts/push.sh

*大小: 1.4 KB | Token: 395*

```bash
#!/bin/bash
source "${WORKSPACE_DIR}/env.sh"

# 配置
DEFAULT_IP="192.168.10.200"
USER="admin"
REMOTE_DIR="/oem/app"

TARGET_PLATFORM="x86"
while getopts "p:b:i:h" opt; do
  case ${opt} in
    p) TARGET_PLATFORM=$OPTARG ;;
    i) DEVICE_IP=$OPTARG ;;
  esac
done

DEVICE_IP=${DEVICE_IP:-$DEFAULT_IP}

# 平台判断
case "${TARGET_PLATFORM}" in
    "arm" | "rv" | "3519" | "a64" | "arm_3519")
        log_info "Target platform [${TARGET_PLATFORM}] recognized. Starting push..."
        ;;
    *)
        log_warn "Platform [${TARGET_PLATFORM}] is local or not pushable. Skipping."
        exit 0
        ;;
esac

# 检查文件
if [ ! -f "${WS_ARTIFACT_PATH}" ]; then
    log_error "Artifact not found: ${WS_ARTIFACT_PATH}"
    exit 1
fi

FILE_NAME=$(basename "${WS_ARTIFACT_PATH}")

log_info "Remote: ${USER}@${DEVICE_IP}:${REMOTE_DIR}"
log_info "Stopping old process and cleaning..."
ssh ${USER}@${DEVICE_IP} "killall -9 ${FILE_NAME} 2>/dev/null ; rm -f ${REMOTE_DIR}/${FILE_NAME}"

log_info "Uploading via SFTP..."
sftp -oBatchMode=no ${USER}@${DEVICE_IP} <<EOF
cd ${REMOTE_DIR}
put ${WS_ARTIFACT_PATH}
bye
EOF

log_info "Verifying MD5..."
l_md5=$(md5sum "${WS_ARTIFACT_PATH}" | awk '{print $1}')
r_md5=$(ssh ${USER}@${DEVICE_IP} "md5sum ${REMOTE_DIR}/${FILE_NAME}" | awk '{print $1}')

if [ "$l_md5" = "$r_md5" ]; then
    log_succ "MD5 matched: ${l_md5}. Push SUCCESS."
else
    log_error "MD5 MISMATCH! Local: $l_md5, Remote: $r_md5"
    exit 1
fi
```

### templates/cpp_basic/src/main.cpp

*大小: 992 B | Token: 261*

```cpp
#include <iostream>
#include <filesystem> // C++17 标准库

namespace fs = std::filesystem;

int main(int argc, char** argv) {
    std::cout << "Hello, World! Testing C++17 filesystem..." << std::endl;

    // 获取当前路径
    try {
        fs::path current_path = fs::current_path();
        std::cout << "Current path is: " << current_path << std::endl;

        // 创建一个测试目录
        fs::path test_dir = current_path / "test_cpp17_dir";
        if (fs::create_directory(test_dir)) {
            std::cout << "Successfully created directory: " << test_dir << std::endl;
            // 删除测试目录
            fs::remove(test_dir);
            std::cout << "Successfully removed directory." << std::endl;
        } else {
            std::cout << "Directory already exists or failed to create." << std::endl;
        }
    } catch (const fs::filesystem_error& e) {
        std::cerr << "Error: " << e.what() << std::endl;
        return 1;
    }

    return 0;
}
```

### templates/cpp_basic/CMakeLists.txt

*大小: 672 B | Token: 161*

```text
cmake_minimum_required(VERSION 3.15)
project(helloworld)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF) # 建议关闭扩展，使用原生标准
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

### env.sh

*大小: 662 B | Token: 162*

```bash
#!/bin/bash

# --- [1. 路径配置] ---
export WS_TC_SEARCH_PATH="${WORKSPACE_DIR}/common_env/toolchains"

# --- [2. 业务平台映射] ---
declare -A WS_PLATFORM_MAP=(
    ["arm_3519"]="aarch64-v01c01-linux-musl.toolchain.cmake"
    ["x86"]="none"
)

# --- [3. 业务相关的工具链解析逻辑 (仍放这里，因为它与映射表强绑定)] ---
resolve_toolchain_path() {
    local p_in=$1
    local tc_val=${WS_PLATFORM_MAP[$p_in]}
    if [ -n "$tc_val" ] && [ "$tc_val" != "none" ]; then
        echo "${WS_TC_SEARCH_PATH}/${tc_val}"
    elif [ -f "${WS_TC_SEARCH_PATH}/${p_in}.cmake" ]; then
        echo "${WS_TC_SEARCH_PATH}/${p_in}.cmake"
    fi
}
```

### ws.sh

*大小: 6.1 KB | Token: 1.4K*

#!/bin/bash
set -e

# --- [I. UI 视觉框架与颜色引擎] ---
# 颜色定义
export CLR_RES='\033[0m'
export CLR_BLD='\033[1m'
export CLR_MAG='\033[0;35m'
export CLR_BLD_MAG='\033[1;35m'
export CLR_BLD_YLW='\033[1;33m'
export CLR_DARK='\033[0;90m'      # 暗灰色，用于降噪
export CLR_SUCC='\033[1;32m'      # 成功高亮绿
export CLR_ERR='\033[1;31m'       # 错误高亮红
export CLR_WARN='\033[1;33m'      # 警告高亮黄
export CLR_BC_HEADER='\033[1;46;30m' 
export CLR_BANNER_TXT='\033[1;36m'   
export CLR_BANNER_BRD='\033[0;36m'   

# 统一日志函数 (带时间戳与暗色降噪逻辑)
log_msg() {
    local level=$1
    local level_color=$2
    local msg=$3
    local time=$(date "+%H:%M:%S")
    # 时间、级别标签、正文默认均采用暗色降噪，仅级别名称本身变色
    echo -e "${CLR_DARK}[${time}]${CLR_RES}${CLR_BLD}[${level_color}${level}${CLR_RES}${CLR_BLD}]${CLR_RES} ${CLR_DARK}${msg}${CLR_RES}"
}

# 导出函数供子脚本直接使用
log_info()  { log_msg "INFO" "$CLR_DARK" "$*"; }
log_succ()  { log_msg "SUCC" "$CLR_SUCC" "$*"; }
log_warn()  { log_msg "WARN" "$CLR_WARN" "$*"; }
log_error() { log_msg "ERR " "$CLR_ERR"  "$*"; }
export -f log_msg log_info log_succ log_warn log_error

# 绘制高亮动作横幅
print_action_banner() {
    local act=$(echo "$1" | tr '[:lower:]' '[:upper:]')
    local plat=$(echo "$2" | tr '[:lower:]' '[:upper:]')
    local exe=$(echo "$3" | tr '[:lower:]' '[:upper:]')
    
    echo -e "\n${CLR_BANNER_BRD}┌────────────────────────────────────────────────────────────────────────────┐${CLR_RES}"
    printf "${CLR_BANNER_BRD}│${CLR_RES}  ${CLR_BC_HEADER} STEP ${CLR_RES}  ${CLR_BANNER_TXT}%-15s ${CLR_BANNER_BRD}│${CLR_RES}  ${CLR_BANNER_TXT}%-15s ${CLR_BANNER_BRD}│${CLR_RES}  ${CLR_BANNER_TXT}%-25s ${CLR_BANNER_BRD}│${CLR_RES}\n" "$act" "$plat" "$exe"
    echo -e "${CLR_BANNER_BRD}└────────────────────────────────────────────────────────────────────────────┘${CLR_RES}"
}

# --- [II. 环境加载与核心初始化] ---
export WORKSPACE_DIR="$(pwd)"
PRJ_DIR="${WORKSPACE_DIR}/prj"

if [ -f "${WORKSPACE_DIR}/env.sh" ]; then
    source "${WORKSPACE_DIR}/env.sh"
else
    echo "❌ 致命错误: 找不到业务配置文件 env.sh"
    exit 1
fi

# --- [III. 任务分发引擎] ---
dispatch_tasks() {
    local raw_actions=$1
    local raw_proj=$2
    shift 2

    # 解析项目名称 (dirname 驱动)
    local proj_name=$(echo "$raw_proj" | sed 's|^prj/||' | cut -d'/' -f1)
    local proj_path="${PRJ_DIR}/${proj_name}"

    # 默认业务参数解析
    local raw_platforms="x86"
    local build_type="Debug"
    local original_args=("$@")
    local OPTIND=1
    while getopts "p:b:i:c:h" opt "$@"; do
        case $opt in
            p) raw_platforms=$OPTARG ;;
            b) build_type=$OPTARG ;;
        esac
    done

    IFS=',' read -ra ACT_ARR <<< "$raw_actions"
    IFS=',' read -ra PLAT_ARR <<< "$raw_platforms"

    # --- 1. 绘制任务清单展示框 ---
    echo -e "${CLR_BLD_MAG}"
    echo "    ╔════════════════════════════════════════════════════════╗"
    echo "    ║                PROJECT BUILD PIPELINE                  ║"
    echo "    ╠════════════════════════════════════════════════════════╣"
    printf "    ║  ${CLR_BLD_YLW}%-12s${CLR_DARK} : ${CLR_RES}${CLR_BLD}%-37s${CLR_BLD_MAG} ║\n" "PROJECT" "${proj_name}"
    printf "    ║  ${CLR_BLD_YLW}%-12s${CLR_DARK} : ${CLR_RES}${CLR_BLD}%-37s${CLR_BLD_MAG} ║\n" "ACTIONS" "${ACT_ARR[*]}"
    printf "    ║  ${CLR_BLD_YLW}%-12s${CLR_DARK} : ${CLR_RES}${CLR_BLD}%-37s${CLR_BLD_MAG} ║\n" "PLATFORMS" "${PLAT_ARR[*]}"
    printf "    ║  ${CLR_BLD_YLW}%-12s${CLR_DARK} : ${CLR_RES}${CLR_BLD}%-37s${CLR_BLD_MAG} ║\n" "TYPE" "${build_type}"
    echo "    ╚════════════════════════════════════════════════════════╝"
    echo -e "${CLR_RES}"

    # --- 2. 循环执行任务流 ---
    for cur_act in "${ACT_ARR[@]}"; do
        for p_alias in "${PLAT_ARR[@]}"; do
            
            # 计算路径与环境变量导出
            export WS_PROJ_NAME="${proj_name}"
            export WS_TARGET_EXE="${proj_name}"
            export WS_TOOLCHAIN_FILE=$(resolve_toolchain_path "$p_alias")
            export WS_BUILD_DIR="${WORKSPACE_DIR}/build/${proj_name}/${p_alias}/${build_type}"
            export WS_OUT_DIR="${WORKSPACE_DIR}/out/${proj_name}/${p_alias}/${build_type}"
            export WS_ARTIFACT_PATH="${WS_OUT_DIR}/${WS_TARGET_EXE}"

            # 打印动作横幅
            print_action_banner "$cur_act" "$p_alias" "$WS_TARGET_EXE"
            
            set +e
            # 进入项目脚本目录执行对应的二级脚本
            ( cd "${proj_path}" && bash "scripts/${cur_act}.sh" "${original_args[@]}" -p "$p_alias" )
            local ret=$?
            set -e

            if [ $ret -eq 0 ]; then
                log_succ "FINISHED: [${cur_act}] ON [${p_alias}]"
            else
                log_error "FAILED: [${cur_act}] ON [${p_alias}] (EXIT CODE: ${ret})"
                exit $ret
            fi
        done
    done

    echo -e "\n${CLR_GRN}${CLR_BLD}>>> 所有 Pipeline 任务处理完毕 <<<${CLR_RES}"
}

# --- [ 辅助功能：基于模板创建新工程 ] ---
create_project() {
    local proj_name=$1
    local tpl_id=$2

    if [ -z "$proj_name" ] || [ -z "$tpl_id" ]; then
        log_error "创建失败：参数缺失！"
        echo -e "${CLR_YLW}用法: ./ws.sh create <项目名> <模板ID>${CLR_RES}"
        echo -e "示例: ./ws.sh create motor_ctrl 1_basic"
        exit 1
    fi

    local tpl_path="${WORKSPACE_DIR}/templates/tpl_${tpl_id}"
    local target_path="${PRJ_DIR}/${proj_name}"

    log_info "正在验证模板: tpl_${tpl_id}"
    if [ ! -d "$tpl_path" ]; then
        log_error "模板目录不存在: ${tpl_path}"
        exit 1
    fi

    if [ -d "$target_path" ]; then
        log_error "工程目录已存在，中止操作防止覆盖: prj/${proj_name}"
        exit 1
    fi

    log_info "正在拷贝模板至: prj/${proj_name}"
    cp -r "${tpl_path}" "${target_path}"

    log_info "正在替换模板占位符 (__PROJECT_NAME__) ..."
    # 兼容 Linux (sed -i) 和 macOS (sed -i '') 的正则替换
    find "${target_path}" -type f -exec sed -i'' -e "s/__PROJECT_NAME__/${proj_name}/g" {} + 2>/dev/null || \
    find "${target_path}" -type f -exec sed -i '' -e "s/__PROJECT_NAME__/${proj_name}/g" {} +

    log_info "正在赋予二级脚本执行权限..."
    chmod +x "${target_path}/scripts/"*.sh 2>/dev/null || true

    log_succ "工程 [${proj_name}] 创建完毕！"
}

# --- [IV. 入口解析] ---
COMMAND_STR=$1
case $COMMAND_STR in
    create) 
        shift 
        create_project "$1" "$2" 
        ;;
    *build*|*pushexe*|*push*|*clean*) 
        dispatch_tasks "$@" 
        ;;
    *) 
        echo -e "${CLR_YLW}======================================================${CLR_RES}"
        echo -e "${CLR_BLD} 脚手架一级控制台使用说明 ${CLR_RES}"
        echo -e "${CLR_YLW}======================================================${CLR_RES}"
        echo -e " 1. 创建工程: ./ws.sh create <项目名> <模板ID>"
        echo -e "    示例: ./ws.sh create vision_nvs 1_basic\n"
        echo -e " 2. 执行流水: ./ws.sh {动作组合} {项目名} [-p 平台] [-b 类型]"
        echo -e "    示例: ./ws.sh build,pushexe helloworld -p x86,arm_3519 -b Release"
        echo -e "${CLR_YLW}======================================================${CLR_RES}"
        exit 1 
        ;;
esac