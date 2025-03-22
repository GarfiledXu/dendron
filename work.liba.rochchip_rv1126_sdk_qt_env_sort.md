---
id: u984dzawzissc4g5kyim1xy
title: Rochchip_rv1126_sdk_qt_env_sort
desc: ''
updated: 1742464866045
created: 1742439448229
---

# rv1126 sdk 依赖以及 qt5 交叉编译开发环境梳理

## 前言
该文档用于记录`qt5`在`linux pc`上的交叉编译环境配置，主要基于CMake和QMake两种方式
共分四个步骤:
1. qt5 完整依赖提取
2. 修改qt中的cmake以及qmake相关配置文件内容
3. 在纯命令行下，使用cmake或者qmake直接引用当前设备端qt进行交叉编译
4. 在qtcreator中，配置新增qt版本，并配置cmake或者qmake

## qt5 完整依赖提取
当前环境使用的qt build文件， 是跟随rv1126 SDK buildroot部分一起编译输出的，所以环境路径以`张佳豪系统镜像环境`作为举例，该环境上已经完成了buildroot的编译，包括qt5 package的编译和安装。中间文件和生成的bin文件以及库都位于`buildroot/output/rockchip_rv1126_rv1109/build/qt5base-5.14.2`下，可提取使用的内容: `/bin`，`/lib`，`/mkspecs`，`/plugins`，额外的还需要从原始的编译环境中找到安装后的`/include`目录，该目录通常伴随着make后的install步骤安装到目标路径上，这里buildroot默认的安装路径位于 `buildroot/output/rockchip_rv1126_rv1109/host/arm-buildroot-linux-gnueabihf/sysroot/usr/include/qt5`。总结: 从已经完成buildroot编译所在的开发环境上提取qt5所有相关依赖，打包以上描述的五个部分到本地开发环境目录即可。

```bash
qt5base-5.14.2
├── bin
├── include
│   └── qt5
├── lib
├── mkspecs
├── plugins
```

## 修改qt中的cmake以及qmake相关配置文件内容

将相关依赖复制到其他开发环境以后，还需要每个子目录下涉及到构建配置的内容，由于

### cmake 部分

### qmake 部分

## 在纯命令行下，使用cmake或者qmake直接引用当前设备端qt进行交叉编译

### cmake 部分

1. 基本的工程结构

```bash
├── src
│   └── main.cpp
├── build
├── build_cmake.sh
├── CMakeLists.txt
└── toolchain
    └── toolchain_linux_arm_rv1126_qt5.cmake
```
说明: 
`src`为模板工程源码存放路径
`build`为编译输出目录
`build_cmake.sh`为自定义的脚本，触发整个工程的构建执行，内容如下:
```bash
#!/bin/bash

# 定义路径变量
BUILD_DIR="build"
CMAKE_DIR="build/cmake"
BUILD_OUT_DIR="build/cmake/out"
CMAKE_BUILD_DIR="build/cmake/build"
PURE_MAKE_DIR="build/pure_make"
TOOLCHAIN_FILE="toolchain/toolchain_linux_arm_rv1126_qt5.cmake"
SOURCE_DIR="."  # 指向包含 CMakeLists.txt 的源代码目录

# Qt 交叉编译工具链相关路径
QT_ROOT="/home/xjf1127/codespace/toolchain/qt-rv1126"  # 你的 Qt 交叉编译目录
QT_QMAKE="$QT_ROOT/bin/qmake"
QT_CMAKE_PREFIX="$QT_ROOT"  # Qt 的 CMake 安装路径

# 目录列表
DIRS=("$BUILD_DIR" "$CMAKE_DIR" "$CMAKE_BUILD_DIR" "$BUILD_OUT_DIR" "$PURE_MAKE_DIR")

# 统一的目录创建函数
create_dirs() {
    for dir in "$@"; do
        if [ ! -d "$dir" ]; then
            mkdir -p "$dir"
            echo "创建 $dir 目录"
        fi
    done
}

# 调用函数创建目录
create_dirs "${DIRS[@]}"

# 运行 CMake 配置，指定源代码目录 (即包含 CMakeLists.txt 的目录)
cmake -S "$SOURCE_DIR" -B "$CMAKE_BUILD_DIR" \
      -DCMAKE_TOOLCHAIN_FILE="$TOOLCHAIN_FILE" \
      -DCMAKE_RUNTIME_OUTPUT_DIRECTORY="$(realpath "$BUILD_OUT_DIR")" \
      -DCMAKE_PREFIX_PATH="$QT_CMAKE_PREFIX" \
      -DCMAKE_BUILD_TYPE=Release \
      -DQT_QMAKE_EXECUTABLE="$QT_QMAKE"

# 构建项目
cmake --build "$CMAKE_BUILD_DIR"
```

`toolchain_linux_arm_rv1126_qt5.cmake`为自定义的，rv1126平台的交叉编译配置，在`build_cmake.sh`中被cmake启动命令所引用。(cmake官方推荐的交叉编译配置方式，相比直接在`CMakeLists.txt`中直接修改`C` `CXX`环境变量路径，通过toolchain文件配置能够确保cmake的编译输出内容与配置内容一致，否则可能存在交叉编译配置生效，但整个过程的构建输出异常现象)
内容:

```cmake
# 设置交叉编译工具链路径
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)


# 设置交叉编译工具链
set(CMAKE_C_COMPILER /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc)
set(CMAKE_CXX_COMPILER /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-g++)

# 设置交叉编译的库和头文件路径
set(CMAKE_FIND_ROOT_PATH 
    /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler
    /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
)

# 设置查找库和头文件时的路径
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

set(CMAKE_AUTOMOC ON)

set(QT_FORCE_STATIC ON)  # 强制使用静态链接库

# 添加 Qt 相关路径
set(CMAKE_PREFIX_PATH "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib/cmake")
set(CMAKE_INCLUDE_PATH "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include")

set(Qt5_DIR "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2")
set(QT_QMAKE_EXECUTABLE "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin/qmake")

```

`CMakeLists.txt` 内容:

```cmake
cmake_minimum_required(VERSION 3.20)
project(MyQtProject CXX)

# 指定 C++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 使能 CMake 自动查找 Qt5
find_package(Qt5 REQUIRED COMPONENTS Core Gui Widgets Network)

# 添加源代码
add_executable(MyQtApp src/main.cpp)

#### buildroot相关组件路径设置
# target
set(buildroot_target_lib_dir
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/target/usr/lib
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/target/lib
)

# hos
set(buildroot_host_inc_dir
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/usr/include
)

## rkmedia 中的 librtsp 需要专门指定，并没有安装到target中
set(rkmedia_rtsp_lib_dir
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/rkmedia/examples/librtsp
)
set(rkmedia_rtsp_inc_dir
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/rkmedia/examples/librtsp
)

# link qt components
target_link_libraries(MyQtApp Qt5::Core Qt5::Gui Qt5::Widgets Qt5::Network)

# link other
target_link_directories(MyQtApp PRIVATE
${buildroot_target_lib_dir}
${rkmedia_rtsp_lib_dir}
)

target_link_libraries(MyQtApp 
rga
z
png16
pcre2-16
gthread-2.0
glib-2.0
drm
pcre
)

```

src下main.cpp内容:

```cpp
#include <QApplication>
#include <QLabel>
#include <unistd.h>
#include <stdio.h>

int main(int argc, char *argv[]) {
    printf("will enter main(), %d\n", getpid());
    QApplication app(argc, argv);
    

    QLabel label("Hello, World!");
    label.setFixedSize(200, 100);
    label.setAlignment(Qt::AlignCenter);
    label.show();
    
    printf("will enter app.exec()\n");
    return app.exec();
}
```

整体编译流程测试:

```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ bash build_cmake.sh 
-- Configuring done (0.1s)
-- Generating done (0.0s)
-- Build files have been written to: /home/xjf1127/codespace/git_down/rv1126_qt_prj_template/build/cmake/build
[ 25%] Automatic MOC for target MyQtApp
[ 25%] Built target MyQtApp_autogen
[100%] Built target MyQtApp
```


### qmake 部分

1. 基本的工程结构

```bash
├── CMakeLists.txt
├── CMakeLists.txt.user
├── MyQtApp
├── build
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   ├── cmake
│   └── pure_make
├── build_cmake.sh
├── build_qmake.sh
├── root.pro
├── src
│   └── main.cpp
└── toolchain
    └── toolchain_linux_arm_rv1126_qt5.cmake
```
说明: 
`src`为模板工程源码存放路径
`build`为编译输出目录
`build_qmake.sh`为自定义的脚本，触发整个工程的构建执行，内容如下:

```bash
export QT_SELECT=qt5_rv2116 \
qmake -o build/Makefile root.pro \
"CONFIG+=debug" \
DESTDIR=./bin \
OBJECTS_DIR=./obj \
MOC_DIR=./moc \
UI_DIR=./ui \
RCC_DIR=./rcc &&  make -C ./build
```

1. 通过环境变量`QT_SELECT`来切换qt工具链(qmake以及/bin目录下的工具集):
qt存在版本管理的概念，依赖于工具`qtchooser`, 工作原理:

```bash
通常在一个linux pc上安装了x64的qt环境之后，会建立一个`qmake`的软链接，其指向`qtchooser`，在终端执行`qmake`命令时，本质上是经历了调用`qtchooser`，解析命令传参，判定到是qmake参数后会调用对应版本的`qmake`。
每一个qt版本的描述都会有版本名称，对应一个具体的`QMake.conf`配置文件，其内容指定了具体的`qmake`程序以及`相关bin路径`，该文件的生成方式有多种，可以在`qtchooser`的固定扫描路径下手动添加，或者调用`qtchooser`的`set命令`会在固定路径下生成配置文件。
而当前终端shell的环境变量 `QT_SELECT` 则会决定使用的qt配置版本，进行变量设置后，默认切换当前开发环境对应的qt版本，对应`qmake`以及系列`/bin`工具也随之切换。
```

. `qtchooser` + `QT_SELECT` 决定最终使用的 `qmake`及其工具链版本，而`qmake`本身的工作由依赖于其自身的环境变量，这些变量的来源和覆盖规则如下:

```bash
1. 默认来源为 qmake 源码编译时的硬编码路径
2. 随后读取 `~/:conf/Qtproject/QMake.conf`该文件对所有版本 qmake 生效
3. 随后读取 当前调用的qmake程序的相对路径，该路径有可能是`qmake`的当前路径，或者是上层路径，通常和qt编译时的版本和平台行为决定，在这里rv1126编译出来的qmake默认只读取当前路径下的固定qt.conf名称
```

root.pro 内容:

```bash
# Qt 模块
QT += core gui widgets network
# 项目设置
TARGET = MyQtApp
TEMPLATE = app
# 源文件
SOURCES += src/main.cpp
# 包含路径
INCLUDEPATH += \
    /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/usr/include \
    /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/rkmedia/examples/librtsp
# 库路径
LIBS += -L/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/target/usr/lib \
        -L/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/target/lib \
        -L/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/rkmedia/examples/librtsp
# 链接库
LIBS += -lrga -lz -lpng16 -lpcre2-16 -lgthread-2.0 -lglib-2.0 -ldrm -lpcre
# Qt 静态链接选项
CONFIG += static
```

整体编译流程测试:

```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ bash build_qmake.sh 
Info: creating stash file /home/xjf1127/codespace/git_down/rv1126_qt_prj_template/build/.qmake.stash
make: Entering directory '/home/xjf1127/codespace/git_down/rv1126_qt_prj_template/build'
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-g++ -c -pipe -D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64 -Os -DUSE_UPDATEENGINE=ON -DSUCCESSFUL_BOOT=ON --sysroot=/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot -Wall -Wextra -D_REENTRANT -fPIC -DQT_WIDGETS_LIB -DQT_GUI_LIB -DQT_NETWORK_LIB -DQT_CORE_LIB -I../../rv1126_qt_prj_template -I. -I../../../toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/usr/include -I../../../toolchain/rk1126_sdk_vunknow/package/rkmedia/examples/librtsp -I../../../toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5 -I../../../toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5/QtWidgets -I../../../toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5/QtGui -I../../../toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5/QtNetwork -I../../../toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5/QtCore -Imoc -I../../../toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/mkspecs/devices/linux-buildroot-g++ -o obj/main.o ../src/main.cpp
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/compiler2/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-g++ --sysroot=/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot -o bin/MyQtApp obj/main.o   -latomic -L/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/target/usr/lib -L/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/target/lib -L/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/package/rkmedia/examples/librtsp -lrga -lz -lpng16 -lpcre2-16 -lgthread-2.0 -lglib-2.0 -ldrm -lpcre /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib/libQt5Widgets.so /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib/libQt5Gui.so /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib/libQt5Network.so /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib/libQt5Core.so -lpthread  -lrt -lpthread -ldl 
make: Leaving directory '/home/xjf1127/codespace/git_down/rv1126_qt_prj_template/build'
```


### 相关概念

1. qmake
2. qmake built-in property
3. qtchooser
4. shell environment variable of qtchooser
5. qt.conf
6. QMake.conf
7. mkspaces
8. cmake find_package

### reference

