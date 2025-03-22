---
id: aid8k40f20zsj3if53ecqn1
title: Rockchip_rv1126_sdk
desc: ''
updated: 1742347230022
created: 1740533448190
---

### my question

        在团队开发中，SDK 是如何在不同角色（硬件、驱动、固件、应用）之间分工使用和传递的？
        我负责的是应用开发，现在 SDK 已同步，prebuilt/ 目录里有交叉编译工具链，但没有 Qt 依赖。
        Qt 的交叉编译依赖项有哪些？为什么 prebuilt/ 里没有？
        Qt 的编译配置应该由谁负责？它涉及底层驱动（如 GPU 加速），是否应该由驱动或固件团队来编译？
        SDK 是否应按模块划分，由特定岗位（如系统集成工程师）完成部分组件的编译，然后交付给应用开发者使用？
        在一个典型的嵌入式团队中，SDK 的完整开发和使用流程是怎样的？如何避免应用开发者额外编译 Qt？

```bash

你的问题涉及SDK 的分工使用与使用流程，尤其是Qt 在整个 SDK 体系中的归属、编译依赖以及团队如何协作。你的核心疑问包括以下几点：

Qt 的编译依赖项是什么？

既然 SDK 已经同步了，但 prebuilt/ 目录中没有 Qt 相关的依赖，那 Qt 在交叉编译时具体依赖哪些库和组件？
SDK 的使用流程是怎样的？

在整个团队开发过程中，SDK 是如何被不同角色（硬件、驱动、固件、应用）使用和管理的？
各个模块（例如交叉工具链、Qt 库、系统 rootfs 等）的编译和配置分别应该由哪个岗位负责？
应用开发者是否应该编译 Qt？

prebuilt/ 目录下的内容是否应该已经包含应用开发所需的所有运行库？
Qt 相关的编译和配置是否应该由驱动或固件开发人员完成，而不是应用开发者自己去编译？
既然 Qt 涉及底层驱动（如图形渲染、窗口系统），那么它是否应该由驱动和固件团队来编译和提供？
从团队协作的角度，SDK 的正确使用流程是什么？

在一个典型的嵌入式开发团队中，SDK 的各部分应如何被组织和管理？
是否存在明确的职责划分，例如某些 SDK 组件（如工具链、Qt）只能由特定岗位编译，而应用层只能使用？

```

```bash
##原始问题
我的疑惑在于，现在sdk同步后，交叉编译工具链是有了，但是我这边只负责进行应用开发，需要用到qt，在prebuildt目录并没有qt的依赖，问题来了，qt的编译依赖项是有哪些,对我而言，我只负责应用开发交叉编译，所以整个sdk包我首先玻璃prebuild部分拷贝到我自己的环境中使用，  但是对于一个项目一个团队而言，是有硬件，驱动，固件，应用这几个分工的，那个从整个团队和项目的角度，对该sdk的使用，应该是有流程顺序，分模块独立关联的，即sdk中部分模块的编译 配置只能由某个岗位进行，成果物再转交给下个流程岗位使用，而不是由下个岗位进行配置编译，我的猜测就是 prebuild里的内容是给应用层以及驱动层都使用的，但对于该平台涉及的应用层依赖，比如qt的编译配置是否是和驱动和固件岗位进行，因为他涉及到驱动之类的，不应该由应用层人操作，应用层只有使用权？   请详细解释一下我上面的疑惑，并且整理出正确的流程进行解释
##结论
那是否结论就是，我从现有的buildroot中提取出编译同步好的 qt5依赖项，进行独立管理 配合prebuild里的交叉编译工具链，就可以进行应用层qt5的开发了
##关键词
开发团队 基于该SDK的开发流程
https://doc.gsw945.com/blog-attach/122/127
使用，分工，流程，顺序，输入和产出物
和各个模块编译相关的文件
```

- 相关文档: rockchip linux软件开发指南
- buildroot maunal online: <http://buildroot.org/downloads/manual/manual.html>
- RV1126/RV1109 Linux SDK 快速⼊⻔
- ref:<https://zhuanlan.zhihu.com/p/658762874>
- ref:<https://www.jb51.net/program/303483svy.htm>
- [blog: RK1126从入门到放弃：（一）编译篇](https://cloud.tencent.com/developer/article/2205338)

### extract

**从原张佳豪虚拟机环境提取 qt5 环境**

- 计划依赖
- 目录拷贝
    1. /home/zhou/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host/bin/qmake -> mnt/hgfs/linux_win_share_path/qt5/src_file/bin

```bash
###操作记录
zhou@zhou:~/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host/bin$ ls | grep q
fixqt4headers.pl
gio-querymodules
gobject-query
mksquashfs
msguniq
qlalr
qmake
qmlcachegen
qmlimportscanner
qmllint
qmlmin
qt.conf
qvkgen
syncqt.pl
unsquashfs

zhou@zhou:~/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host/bin$ cp q* /mnt/hgfs/linux_win_share_path/qt5/src_file/bin

zhou@zhou:~/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host/lib64$ cd ../lib && ls -l | grep libQt
-rw-r--r--  1 root root  5077050 12月 30  2022 libQt5Bootstrap.a
-rw-r--r--  1 root root      647 12月 30  2022 libQt5Bootstrap.la
-rw-r--r--  1 root root     1046 12月 30  2022 libQt5Bootstrap.prl
-rw-r--r--  1 root root  4789550 12月 30  2022 libQt5QmlDevTools.a
-rw-r--r--  1 root root      780 12月 30  2022 libQt5QmlDevTools.la
-rw-r--r--  1 root root     1214 12月 30  2022 libQt5QmlDevTools.prl


cp libQt5* /mnt/hgfs/linux_win_share_path/qt5/src_file/lib

tar -czvf qt5base-1.14.2.tar.gztar -czvf ./qt5base-1.14.2

 2012  tar -czvf qt5base-5.14.2.tar.gz ./qt5base-5.14.2/
 2013  cp ./qt5base-5.14.2.tar.gz /mnt/hgfs/linux_win_share_path/qt5/src_file/bulid
 2014  tar -czvf qt5baseserialport-5.14.2.tar.gz ./qt5serialport-5.14.2/
 2015  tar -czvf qt5declarative-5.14.2.tar.gz ./qt5declarative-5.14.2/-5.14.2/
 2016  tar -czvf qt5declarative-5.14.2.tar.gz ./qt5declarative-5.14.2/
 2017  tar -czvf qt5multimedia-5.14.2.tar.gz ./qt5multimedia-5.14.2
 2018  tar -czvf qt5quickcontrols-5.14.2.tar.gz ./qt5quickcontrols-5.14.2

cp -f qt5*.tar.gz /mnt/hgfs/linux_win_share_path/qt5/src_file/bulid

```

### 初步总结

- bulidroot是一种构建框架/系统，是一个综合性的构建框架，提高系统镜像构建的效率，而package则是buldroot中的一个环节，同名目录用于存放三方软件资源

### 追问

- 如何单独构建qt package，而不是整个buildroot进行配置?或者是通过bulidroot框架，指定只进行qt的构建
**即如何基于bulidroot来进行单独的QT的环境构建**
- refe: <https://juejin.cn/post/7192572702547247161>
- refe: <https://juejin.cn/post/7286307632193159227?from=search-suggest>
- refe: <https://cloud.tencent.com/developer/article/1962247>  非常匹配
- 如何确认一个sdk的版本号????
- [qt offical: the qt command in cmake](https://doc.qt.io/qt-6/cmake-command-reference.html)

### sdk压缩包

`RV1126_RV1109_LINUX_SDK_V3.0.3_20231229.tar.bz2`

### 解压后结构

解压后是一个 .repo 文件夹，repo是google用于管理大型git项目的工具，`.repo` 则是该工具的工作区

### repo 处理

```bash
mkdir rv1126_rv1109
tar xjf rv1126_rv1109_linux_sdk_v1.0.0_20200616.tar.bz2 -C rv1126_rv1109
cd rv1126_rv1109
.repo/repo/repo sync -l
.repo/repo/repo sync -c
```

```bash
#解压后的目录结构 tree -L 2
```

### qt path set on cmake by manual

qt的prefix path中的cmake config内容，是在编译时生成的，其中很多依赖路径是直接按照安装路径指定的，比如qmake和include

1. 先服用原先cmakeconfig，拷贝qt的cmake config目录后，重新指定这些绝对路径，来进行重定向
2. 或者不依赖于cmakeconfig,直接指定
3. 修改cmakeconfig内容
4. 根据当前build出来的结果，单独重新生成cmakeconfig
5. 方案4验证失败的话，重新构建qt，重新生成install的目录

cmake变量解释
**CMAKE_PREFIX_PATH**
:CMake 查找包时的搜索路径,指定根目录即可

**Qt5_DIR**
: qt的安装路径

```bash
#replace
/home/zhou/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host/bin
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin

# /home/zhou/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host/
# /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/

/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib/cmake/install/Qt5Core/Qt5CoreConfigExtrasMkspecDir.cmake
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib/cmake/Qt5Core/Qt5CoreConfigExtrasMkspecDir.cmake
${_qt5Core_install_prefix}/../../..//mkspecs/devices/linux-buildroot-g++
${_qt5Core_install_prefix}/../../../../build/qt5base-5.14.2//mkspecs/devices/linux-buildroot-g++

-->
${_qt5Core_install_prefix}/mkspecs/devices/linux-buildroot-g++


### 修改plugin路径
${_qt5Gui_install_prefix}/lib/${LIB_LOCATION}
#### 修改
在lib目录下新建qt子目录，将build目录下的plugins目录移动到lib的qt子目录下
```

#### 注意

cmake中find_package的行为必须在project之后，否则可能会触发目标系统不支持动态链接的错误，而toolchain的解析在进入主cmakelist解析之前，即在project之前，所以，find_package不能在toolchain文件中设置，必须在主cmakelist中的project设置之后;

#### 如何根据qt build包来配置qt qmake环境

1. 搭建纯命令行编译环境
2. 根据qtcreator的图形界面进行配置

#### concept of qmake

- qt.conf
- mkspecs
  - qmake.conf
- qmake build-in variable (qmake property)
- qmake cache file `.qmake.cache`

#### question

- qt.conf and qmake -set and qmake.conf and qtchooser?
        - qt.conf中的配置项与 qmake中的property为同一个概念?
        - qt.conf 是给 `host` 编译方解析提供依赖地址的 还是给 `target` 运行方qt程序运行时解析来查找库，组件，插件路径的?还是说提供给 `host`方的依赖管理或者切换工具的???
        - qmake `property`属性作用是否是提供给`host`放进行编译依赖引用，持久话数据存储在什么位置?
        - 所谓qmake的`property`硬编码植入是指什么形式?
        - `QSettings`是什么?
- .qmake.cache and QMake.conf? 没有关系
- QMake.conf的路径只和平台系统相关? 无法指定? 是的
- 除了qmake内置的这几种property 变量，还有其他和qmake qt相关的环境变量吗，存储在哪里，被谁解析?是否有命令可以进行查询? `QT_PLUGIN_PATH`这个shell环境变量被哪些工具依赖?
- 本地`/usr/bin/qmake`查看属性是指向 `/usr/bin/`
- 在已构建的qt build包中的lib目录，`.pr*`等文件中路径是绝对路径，但在一个tmp-target-install目录下，有一个完整的host目录，里面的lib下的`.pr*`文件中使用的是qmake变量，比如QT_INSTALL_LIBS, 这是为什么?

#### 实验

- 根据 stack overflow 的提问进行问题复现, 首先在`QSettings`相关目录`~/.config/QtProject`下存在，几个默认文件`QtCreator.db  QtCreator.ini  qtcreator`，通过 `qmake -set <PROPERTY> <VALUE>`后再进行`qmake -query <PROPERTY>`查询，反馈的还是默认值，通过`qmake -query`查询所有属性，会发现刚才设置属性出现，即与默认值共存，再回到`QSettings`目录，发现已生成了`QMake.conf`文件，并且内容即为`qmake -set`设置的属性键值对, 通过`qmake -unset`指令可以进行删除
- 验`qt.conf`会被哪些工具解析? 到底qt程序在运行时是否会读取`qt*.conf`文件

#### refe of qt qmake

- [qt offical: Configuring qmake](https://doc.qt.io/qt-6/qmake-environment-reference.html)
- [qt offical: Using qt.conf](https://doc.qt.io/qt-6/qt-conf.html)
- [csdn: qmake运行时依赖的配置文件集](https://blog.csdn.net/qiushangren/article/details/127027545)
- [csdn: Qt--qt.conf](https://blog.csdn.net/GG_SiMiDa/article/details/78528193)
- [stack overflow: how to modify qmake build-in configure](https://stackoverflow.com/questions/7748424/qmake-query-internal-settings-in-linux-where-are-they) :这里提出了属性如何修改的疑惑, 默认值是在qmake编译时就在`src/corelib/global/qconfig.cpp`中硬编码植入，如果通过`-set PROPERTY`命令进行具体属性修改，查询到的变量值还是默认值，罗列全部属性会存在两个键值
- [qt offical: Configuring qmake](https://doc.qt.io/qt-6/qmake-environment-reference.html): 该文档介绍了qmake的built-in variable,即所谓的`PROPERTY`, 如何进行设置和查询，关键一点提到**This information will be saved into a QSettings object (meaning it will be stored in different places for different platforms).**，提及了一个 `QSettings`的机制用于qmake属性持久化存储

#### qt 配置文件集说明

- `.pri, .prl, .pro`文件中依赖变量

        ```bash

        $$[QT_INSTALL_LIBS]
        ```

#### mkspaecs改造

```bash
当前未改造
```

#### qtchooser 原理

为什么qmake直接软链接指向qtchooser以后，使用qmake命令就能导向正确的qmake，这是什么原理
这是软链接直接提供的机制还是说还有其他原理

```bash
xjf1127@DESKTOP-E1M5RHH:/usr/bin$ qmake -query
QT_SYSROOT:
QT_INSTALL_PREFIX:/usr
QT_INSTALL_ARCHDATA:/usr/lib/x86_64-linux-gnu/qt5
QT_INSTALL_DATA:/usr/share/qt5
QT_INSTALL_DOCS:/usr/share/qt5/doc
QT_INSTALL_HEADERS:/usr/include/x86_64-linux-gnu/qt5
QT_INSTALL_LIBS:/usr/lib/x86_64-linux-gnu
QT_INSTALL_LIBEXECS:/usr/lib/x86_64-linux-gnu/qt5/libexec
QT_INSTALL_BINS:/usr/lib/qt5/bin
QT_INSTALL_TESTS:/usr/tests
QT_INSTALL_PLUGINS:/usr/lib/x86_64-linux-gnu/qt5/plugins
QT_INSTALL_IMPORTS:/usr/lib/x86_64-linux-gnu/qt5/imports
QT_INSTALL_QML:/usr/lib/x86_64-linux-gnu/qt5/qml
QT_INSTALL_TRANSLATIONS:/usr/share/qt5/translations
QT_INSTALL_CONFIGURATION:/etc/xdg
QT_INSTALL_EXAMPLES:/usr/lib/x86_64-linux-gnu/qt5/examples
QT_INSTALL_DEMOS:/usr/lib/x86_64-linux-gnu/qt5/examples
QT_HOST_PREFIX:/usr
QT_HOST_DATA:/usr/lib/x86_64-linux-gnu/qt5
QT_HOST_BINS:/usr/lib/qt5/bin
QT_HOST_LIBS:/usr/lib/x86_64-linux-gnu
QMAKE_SPEC:linux-g++
QMAKE_XSPEC:linux-g++
QMAKE_VERSION:3.1
QT_VERSION:5.15.3
xjf1127@DESKTOP-E1M5RHH:/usr/bin$ ls -l qmake
lrwxrwxrwx 1 root root 9 Mar 22  2020 qmake -> qtchooser
xjf1127@DESKTOP-E1M5RHH:/usr/bin$ qtchooser -query
qtchooser: unknown option: -query
```

```bash
为什么 qmake 软链接到 qtchooser 仍然能找到正确的 qmake？
当 qmake 是 qtchooser 的软链接时，执行 qmake 实际上是在执行 qtchooser，并且 qtchooser 解析自身的调用方式，决定调用哪个 qmake。其关键机制如下：

（1）argv[0] 解析
当 qmake 软链接到 qtchooser 后，执行 qmake 实际上运行的是 qtchooser。
qtchooser 通过 argv[0] 获取调用时的名称，即 qmake。
qtchooser 识别到 qmake，然后根据配置找到合适的 qmake 可执行文件并运行它。
（2）查找规则
qtchooser 使用以下方式查找正确的 qmake：

环境变量 QT_SELECT
如果设置了 QT_SELECT=qt5，qtchooser 就会根据 qt5 对应的 qmake 路径执行。
用户配置文件 ~/.config/qtchooser/*.conf
用户可以创建 qt5.conf，指定 qmake 的路径，例如：
sh
复制
编辑
/opt/qt5/bin/qmake
/opt/qt5/lib
系统级配置 /etc/xdg/qtchooser/*.conf
系统管理员可以配置全局 Qt 版本。
默认搜索路径
如果没有明确配置，qtchooser 会根据系统默认的 Qt 版本查找 qmake。
```

#### qtchooser的使用

1. qtchooser拥有内置变量
**该变量是shell进程的，还是qtchooser的**

```bash
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ qtchooser
Usage:
  qtchooser { -l | -list-versions | -print-env }
  qtchooser -install [-f] [-local] <name> <path-to-qmake>
  qtchooser -run-tool=<tool name> [-qt=<Qt version>] [program arguments]
  <executable name> [-qt=<Qt version>] [program arguments]

Environment variables accepted:
 QTCHOOSER_RUNTOOL  name of the tool to be run (same as the -run-tool argument)
 QT_SELECT          version of Qt to be run (same as the -qt argument)

xjf1127@DESKTOP-E1M5RHH:~/.config/QtProject$ qtchooser -print-env
QT_SELECT="default"
QTTOOLDIR="/usr/lib/qt5/bin"
QTLIBDIR="/usr/lib/x86_64-linux-gnu"
```

实验: 确定设置`QT_SELECT`环境变量会影响qmake的配置

```bash
## 验证环境变量
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ export QT_SELECT=qt5-cross
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ echo $QT_SELECT
qt5-cross
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ qmake -v
qmake: could not find a Qt installation of 'qt5-cross'

xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ unset QT_SELECT
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ qmake -v
## 确定该环境变量是生效的，qchooser进程会读取环境变量值，覆盖进程硬编码的默认值
```

2. 三个环境变量关系验证
使用-print-env命令打印后，后两个变量值是属于版本配置内容的显示还是独立设置的变量?
```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ cat ~/.config/qtchooser/qt5_rv2116.conf 
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib

xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ qtchooser -print-env
QT_SELECT="qt5_rv2116"
QTTOOLDIR="/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin"
QTLIBDIR="/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib"
### 说明，只是第一个参数配置内容的显示
```

3. 手动新增qtchooser版本配置
        1. qtchooser配置文件的存储位置?这些配置文件路径的存储位置是默认指定的？还是可以qtchooser单独注册存储的?
        ref: <https://blog.csdn.net/WMX843230304WMX/article/details/127942273>
        **可以确定，配置文件名称与qtchooser查询到的qt版本名称是对应的, 推测qtchooser通过conf文件名作为版本报名**
        2. 进行创建配置文件，放置到预定义目录, 将文件放入到`~/.config/qtchooserf`目录下，qtchooser运行时可进行便利搜索
        3. 通过qtchooser的-install命令进行版本注册, 会将生成的配置文件存储到`~/.config/qtchooserf`目录下

```bash
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ qtchooser -l
4
5
default
qt4-x86_64-linux-gnu
qt4
qt5-x86_64-linux-gnu
qt5
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ locate 4.conf  |grep qtchooser
/usr/lib/x86_64-linux-gnu/qtchooser/4.conf
/usr/lib/x86_64-linux-gnu/qtchooser/qt4.conf
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ locate 5.conf  |grep qtchooser
/usr/lib/x86_64-linux-gnu/qtchooser/5.conf
/usr/lib/x86_64-linux-gnu/qtchooser/qt5.conf
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ locate qt5-x86_64-linux-gnu.conf  |grep qtchooser
/usr/share/qtchooser/qt5-x86_64-linux-gnu.conf
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ locate default.conf  |grep qtchooser
/usr/lib/x86_64-linux-gnu/qt-default/qtchooser/default.conf
### 研究内容
xjf1127@DESKTOP-E1M5RHH:/usr/lib/qt5/bin$ cat /usr/lib/x86_64-linux-gnu/qtchooser/qt5.conf
/usr/lib/qt5/bin
/usr/lib/x86_64-linux-gnu
# <Qt 工具目录路径>
# <Qt 库目录路径>

### 通过locate命令查询qtchooer的相关路径，并且通过查看 /var/lib/dpkg/info/qtchooser.list 内容来查看所有相关的安装路径

### 调试查找 qtchooser的硬编码路径
strace -e openat qtchooser -print-env
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libstdc++.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/home/xjf1127/.config/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/xdg/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/share/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/qt-default/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/qt-default/qtchooser//default.conf", O_RDONLY) = 4
QT_SELECT="default"
QTTOOLDIR="/usr/lib/qt5/bin"
QTLIBDIR="/usr/lib/x86_64-linux-gnu"
+++ exited with 0 +++
### 可以看出 qtchooser 的库加载流程，以及目录查找流程

```

```bash
xjf1127@DESKTOP-E1M5RHH:~/.config/qtchooser$ cat qt5-cross.conf 
/xjf_qt_conf_test/path/usr/xjf_qt_conf_test/bin
/xjf_qt_conf_test/path/usr/xjf_qt_conf_test/lib
```

4. 配置版本后，启动对应版本的qmake
        1. 通过设置环境变量 `QT_SELECT` 来配置启动的version，然后直接通过qmake命令进行调用
        2. 通过`qtchooser -run-tool` 命令来调用qmake功能 `qtchooser -run-tool=qmake -qt=qt5-cross --version`
        3. 直接使用 qmake绝对路径, 可能存在问题，因为涉及到相关工具链

5. qtchoooser的qt.conf与qmake路径下的qt.conf
```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ cat ~/.config/qtchooser/qt5_rv2116.conf 
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ qmake -version
QMake version 3.1
Using Qt version 5.14.2 in /xjf_qt_conf_test/path/usr/xjf_qt_conf_test/lib


```  
可以发现，设置了qtchooser的conf以后，qmake能够正确被找到，但是在进行版本查询时，version in的路径并非该conf指定的
```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ qtchooser -print-env
QT_SELECT="qt5_rv2116"
QTTOOLDIR="/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin"
QTLIBDIR="/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib"
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ strace -f -e openat,access qmake -version
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libstdc++.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/home/xjf1127/.config/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
openat(AT_FDCWD, "/home/xjf1127/.config/qtchooser//qt5_rv2116.conf", O_RDONLY) = 4
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libstdc++.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
access("/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin/qt.conf", F_OK) = 0
access("/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin/qt.conf", F_OK) = 0
openat(AT_FDCWD, "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin/qt.conf", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/etc/localtime", O_RDONLY|O_CLOEXEC) = 4
QMake version 3.1
Using Qt version 5.14.2 in /xjf_qt_conf_test/path/usr/xjf_qt_conf_test/lib
+++ exited with 0 +++
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ mv qt.conf qt5.conf
mv: cannot stat 'qt.conf': No such file or directory
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ mv /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin/qt.conf /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin/qt5.conf
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ qmake --version
QMake version 3.1
Using Qt version 5.14.2 in 
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ qtchooser -print-env
QT_SELECT="qt5_rv2116"
QTTOOLDIR="/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin"
QTLIBDIR="/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib"
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ strace -f -e openat,access qmake -version
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libstdc++.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/home/xjf1127/.config/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
openat(AT_FDCWD, "/home/xjf1127/.config/qtchooser//qt5_rv2116.conf", O_RDONLY) = 4
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libstdc++.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
access("/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin/qt.conf", F_OK) = -1 ENOENT (No such file or directory)
QMake version 3.1
Using Qt version 5.14.2 in 
+++ exited with 0 +++
```

**可以发现，qmake会去自身所在目录去读取固定文件 `qt.conf`，这部分内容会影响 `qmake -query`的变量**

```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ qmake -version
QMake version 3.1
Using Qt version 5.14.2 in /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/aplus$ qmake -query
QT_SYSROOT:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot
QT_INSTALL_PREFIX:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_PREFIX/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_ARCHDATA:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_ARCHDATA/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_ARCHDATA/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_DATA:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_DATA/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_DATA/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_DOCS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/doc
QT_INSTALL_DOCS/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/doc
QT_INSTALL_DOCS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/doc
QT_INSTALL_HEADERS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5
QT_INSTALL_HEADERS/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5
QT_INSTALL_HEADERS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include
QT_INSTALL_LIBS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib
QT_INSTALL_LIBS/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib
QT_INSTALL_LIBS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib
QT_INSTALL_LIBEXECS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/libexec
QT_INSTALL_LIBEXECS/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/libexec
QT_INSTALL_LIBEXECS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/libexec
QT_INSTALL_BINS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin
QT_INSTALL_BINS/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin
QT_INSTALL_BINS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin
QT_INSTALL_TESTS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/tests
QT_INSTALL_TESTS/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/tests
QT_INSTALL_TESTS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/tests
QT_INSTALL_PLUGINS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/plugins
QT_INSTALL_PLUGINS/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/plugins
QT_INSTALL_PLUGINS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/plugins
QT_INSTALL_IMPORTS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/imports
QT_INSTALL_IMPORTS/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/imports
QT_INSTALL_IMPORTS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/imports
QT_INSTALL_QML:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/qml
QT_INSTALL_QML/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/qml
QT_INSTALL_QML/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/qml
QT_INSTALL_TRANSLATIONS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/translations
QT_INSTALL_TRANSLATIONS/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/translations
QT_INSTALL_TRANSLATIONS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/translations
QT_INSTALL_CONFIGURATION:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_CONFIGURATION/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_CONFIGURATION/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_EXAMPLES:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/examples
QT_INSTALL_EXAMPLES/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/examples
QT_INSTALL_EXAMPLES/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/examples
QT_INSTALL_DEMOS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/examples
QT_INSTALL_DEMOS/raw:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/examples
QT_INSTALL_DEMOS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/examples
QT_HOST_PREFIX:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host
QT_HOST_PREFIX/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_HOST_DATA:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host
QT_HOST_DATA/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_HOST_BINS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/bin
QT_HOST_BINS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin
QT_HOST_LIBS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/lib
QT_HOST_LIBS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib
QMAKE_SPEC:mkspecs/linux-g++
QMAKE_XSPEC:mkspecs/devices/linux-buildroot-g++
QMAKE_VERSION:3.1
QT_VERSION:5.14.2
```

6. 又涉及到了 mkspecs 的配置规则 和 QMAKESPEC, 以及qmake -query中的变量与 qt.conf中变量的映射关系
mkspec下面的
```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ ls -l /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/bin/arm-linux-gnueabihf-g++
lrwxrwxrwx 1 xjf1127 xjf1127 17 Dec  7  2022 /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/bin/arm-linux-gnueabihf-g++ -> toolchain-wrapper
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ strace -o qmake_trace.log -f -e trace=openat qmake root.pro
Project ERROR: Cannot run target compiler '/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/bin/arm-linux-gnueabihf-g++'. Output:
===================
===================
Maybe you forgot to setup the environment?
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ whilch^C
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ ^C
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ which toolchain-wrapper
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ locate toolchain-wrapper
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/bin/toolchain-wrapper
/home/xjf1127/codespace/toolchain/rockchip_rv1126_rv1109/build/toolchain-external-custom/toolchain-wrapper
/home/xjf1127/codespace/toolchain/rockchip_rv1126_rv1109/host/bin/toolchain-wrapper
/home/xjf1127/codespace/toolchain/rv1126_sdk_v3.0.3_20231229/buildroot/toolchain/toolchain-wrapper.c
/home/xjf1127/codespace/toolchain/rv1126_sdk_v3.0.3_20231229/buildroot/toolchain/toolchain-wrapper.mk
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ toolchain-wrapper
toolchain-wrapper: command not found
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ ^C
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ /home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/bin/toolchain-wrapper
/home/zhou/work/rv1126_SDK/1126/buildroot/../prebuilts/gcc/linux-x86/arm/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/toolchain-wrapper: No such file or directory
```

```bash
/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/**


```




关于sysroot
```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ qmake -query | grep sysroot
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ qmake -query 
QT_SYSROOT:
QT_INSTALL_PREFIX:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_ARCHDATA:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_DATA:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_DOCS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/doc
QT_INSTALL_HEADERS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5
QT_INSTALL_HEADERS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include
QT_INSTALL_LIBS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib
QT_INSTALL_LIBEXECS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/libexec
QT_INSTALL_BINS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin
QT_INSTALL_TESTS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/tests
QT_INSTALL_PLUGINS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/plugins
QT_INSTALL_IMPORTS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/imports
QT_INSTALL_QML:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/qml
QT_INSTALL_TRANSLATIONS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/translations
QT_INSTALL_CONFIGURATION:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_EXAMPLES:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/examples
QT_INSTALL_DEMOS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/examples
QT_HOST_PREFIX:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host
QT_HOST_PREFIX/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_HOST_DATA:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host
QT_HOST_DATA/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_HOST_BINS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/bin
QT_HOST_BINS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin
QT_HOST_LIBS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/lib
QT_HOST_LIBS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib
QMAKE_SPEC:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/mkspecs/linux-g++
QMAKE_XSPEC:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/mkspecs/devices/linux-buildroot-g++
QMAKE_VERSION:3.1
QT_VERSION:5.14.2
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ ^C
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/rv1126_qt_prj_template$ qmake -query 
QT_SYSROOT:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot
QT_INSTALL_PREFIX:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_ARCHDATA:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_DATA:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_DOCS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/doc
QT_INSTALL_HEADERS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include/qt5
QT_INSTALL_HEADERS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/include
QT_INSTALL_LIBS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib
QT_INSTALL_LIBEXECS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/libexec
QT_INSTALL_BINS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin
QT_INSTALL_TESTS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/tests
QT_INSTALL_PLUGINS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/plugins
QT_INSTALL_IMPORTS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/imports
QT_INSTALL_QML:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/qml
QT_INSTALL_TRANSLATIONS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/translations
QT_INSTALL_CONFIGURATION:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_INSTALL_EXAMPLES:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/examples
QT_INSTALL_DEMOS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/examples
QT_HOST_PREFIX:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host
QT_HOST_PREFIX/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_HOST_DATA:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host
QT_HOST_DATA/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
QT_HOST_BINS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/bin
QT_HOST_BINS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin
QT_HOST_LIBS:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/lib
QT_HOST_LIBS/get:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/lib
QMAKE_SPEC:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/mkspecs/linux-g++
QMAKE_XSPEC:/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/mkspecs/devices/linux-buildroot-g++
QMAKE_VERSION:3.1
QT_VERSION:5.14.2
```
将sysroot修改正确
并且将编译器修改正确未prebuilt的即可


```bash
[EffectivePaths]
Prefix=..
[DevicePaths]
Prefix=/usr
Headers=include/qt5
Plugins=lib/qt/plugins
Examples=lib/qt/examples
[Paths]
Prefix=/usr
Headers=include/qt5
Plugins=lib/qt/plugins
Examples=lib/qt/examples
HostPrefix=/home/zhou/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host
Sysroot=/home/zhou/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host/arm-buildroot-linux-gnueabihf/sysroot
SysrootifyPrefix=true
TargetSpec=devices/linux-buildroot-g++
HostSpec=linux-g++

这里的EffectivePaths DevicePaths Paths这几个table字段是什么意思，这些预定义字段哪里可以查询到
```
- refe
  - [csdn: QLibraryInfo、qt.conf用法及关系总结](https://blog.csdn.net/danshiming/article/details/127149393)
  - [csdn: qt获取qt.conf 中内容过程 QLibraryInfo](https://blog.csdn.net/qiushangren/article/details/126868864)
本质上 qmake就是一个qt程序，同样会进行qt.conf的读取, 而这些配置项同样会影响qmake本身设计的环境变量

在build的host主机上
```bash
zhou@zhou:~/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109$ cat ./host/bin/qt.conf 
[Paths]
Prefix=/usr
HostPrefix=/home/zhou/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host
Sysroot=/home/zhou/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host/arm-buildroot-linux-gnueabihf/sysroot
Headers=/usr/include/qt5
Plugins=/usr/lib/qt/plugins
Examples=/usr/lib/qt/examples
zhou@zhou:~/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109$ cat ./build/qt5base-5.14.2/bin/qt.conf 
[EffectivePaths]
Prefix=..
[DevicePaths]
Prefix=/usr
Headers=include/qt5
Plugins=lib/qt/plugins
Examples=lib/qt/examples
[Paths]
Prefix=/usr
Headers=include/qt5
Plugins=lib/qt/plugins
Examples=lib/qt/examples
HostPrefix=/home/zhou/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host
Sysroot=/home/zhou/work/rv1126_SDK/1126/buildroot/output/rockchip_rv1126_rv1109/host/arm-buildroot-linux-gnueabihf/sysroot
SysrootifyPrefix=true
TargetSpec=devices/linux-buildroot-g++
HostSpec=linux-g++

```
!!!! 如何制作qt安装包???
**实验**
1. 验证qt.conf的生效目录
   - [ ] qmake当前路径
   - [ ] qmake上层路径
```bash
xjf1127@DESKTOP-E1M5RHH:/usr/lib/x86_64-linux-gnu/qt5$ cat qt.conf 
[Paths]
#Prefix=/usr
Prefix=/xxx
#ArchData=lib/x86_64-linux-gnu/qt5
ArchData=xxxxxxxx
Binaries=lib/qt5/bin
Data=share/qt5
Documentation=share/qt5/doc
#Examples=lib/x86_64-linux-gnu/qt5/examples
Examples=xxxxxxdfksdjfkldsf
Headers=include/x86_64-linux-gnu/qt5
HostBinaries=lib/qt5/bin
HostData=lib/x86_64-linux-gnu/qt5
HostLibraries=lib/x86_64-linux-gnu
Imports=lib/x86_64-linux-gnu/qt5/imports
Libraries=lib/x86_64-linux-gnu
LibraryExecutables=lib/x86_64-linux-gnu/qt5/libexec
Plugins=lib/x86_64-linux-gnu/qt5/plugins
Qml2Imports=lib/x86_64-linux-gnu/qt5/qml
Settings=/etc/xdg
Translations=share/qt5/translations
xjf1127@DESKTOP-E1M5RHH:/usr/lib/x86_64-linux-gnu/qt5$ ./bin/qmake -query
QT_SYSROOT:
QT_INSTALL_PREFIX:/xxx
QT_INSTALL_ARCHDATA:/xxx/xxxxxxxx
QT_INSTALL_DATA:/xxx/share/qt5
QT_INSTALL_DOCS:/xxx/share/qt5/doc
QT_INSTALL_HEADERS:/xxx/include/x86_64-linux-gnu/qt5
QT_INSTALL_LIBS:/xxx/lib/x86_64-linux-gnu
QT_INSTALL_LIBEXECS:/xxx/lib/x86_64-linux-gnu/qt5/libexec
QT_INSTALL_BINS:/xxx/lib/qt5/bin
QT_INSTALL_TESTS:/xxx/tests
QT_INSTALL_PLUGINS:/xxx/lib/x86_64-linux-gnu/qt5/plugins
QT_INSTALL_IMPORTS:/xxx/lib/x86_64-linux-gnu/qt5/imports
QT_INSTALL_QML:/xxx/lib/x86_64-linux-gnu/qt5/qml
QT_INSTALL_TRANSLATIONS:/xxx/share/qt5/translations
QT_INSTALL_CONFIGURATION:/etc/xdg
QT_INSTALL_EXAMPLES:/xxx/xxxxxxdfksdjfkldsf
QT_INSTALL_DEMOS:/xxx/xxxxxxdfksdjfkldsf
QT_HOST_PREFIX:/xxx
QT_HOST_DATA:/xxx/lib/x86_64-linux-gnu/qt5
QT_HOST_BINS:/xxx/lib/qt5/bin
QT_HOST_LIBS:/xxx/lib/x86_64-linux-gnu
QMAKE_SPEC:linux-g++
QMAKE_XSPEC:linux-g++
QMAKE_VERSION:3.1
QT_VERSION:5.15.3

xjf1127@DESKTOP-E1M5RHH:/usr/lib/x86_64-linux-gnu/qt5$ strace -e trace=open,openat ./bin/qmake -query 2>&1 | grep -E '\/'
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "./bin/qmake", O_RDONLY) = 3
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libstdc++.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/qt5/qt.conf", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/etc/localtime", O_RDONLY|O_CLOEXEC) = 4
openat(AT_FDCWD, "/home/xjf1127/.config/QtProject/QMake.conf", O_RDONLY|O_CLOEXEC) = 3
openat(AT_FDCWD, "/home/xjf1127/.config/QtProject.conf", O_RDONLY|O_CLOEXEC) = 3
QT_INSTALL_PREFIX:/xxx
QT_INSTALL_ARCHDATA:/xxx/xxxxxxxx
QT_INSTALL_DATA:/xxx/share/qt5
QT_INSTALL_DOCS:/xxx/share/qt5/doc
QT_INSTALL_HEADERS:/xxx/include/x86_64-linux-gnu/qt5
QT_INSTALL_LIBS:/xxx/lib/x86_64-linux-gnu
QT_INSTALL_LIBEXECS:/xxx/lib/x86_64-linux-gnu/qt5/libexec
QT_INSTALL_BINS:/xxx/lib/qt5/bin
QT_INSTALL_TESTS:/xxx/tests
QT_INSTALL_PLUGINS:/xxx/lib/x86_64-linux-gnu/qt5/plugins
QT_INSTALL_IMPORTS:/xxx/lib/x86_64-linux-gnu/qt5/imports
QT_INSTALL_QML:/xxx/lib/x86_64-linux-gnu/qt5/qml
QT_INSTALL_TRANSLATIONS:/xxx/share/qt5/translations
QT_INSTALL_CONFIGURATION:/etc/xdg
QT_INSTALL_EXAMPLES:/xxx/xxxxxxdfksdjfkldsf
QT_INSTALL_DEMOS:/xxx/xxxxxxdfksdjfkldsf
QT_HOST_PREFIX:/xxx
QT_HOST_DATA:/xxx/lib/x86_64-linux-gnu/qt5
QT_HOST_BINS:/xxx/lib/qt5/bin
QT_HOST_LIBS:/xxx/lib/x86_64-linux-gnu
```
使用交叉编译qmake
```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2$ strace -e trace=open,openat qmake -query 2>&1 | grep "qt.conf"
openat(AT_FDCWD, "/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/bin/qt.conf", O_RDONLY|O_CLOEXEC) = 3
```
现象: 
通过移动qmake位置和qt.conf位置
在pc端安装的qt是在qmake的上层路径
在交叉编译qmake读取的是qmake当层路径
但是至少可以确定，和和编译时输出qt.conf和qmake的相对位置是对应的
```bash
xjf1127@DESKTOP-E1M5RHH:/usr/lib/x86_64-linux-gnu/qt5$ cat qt.conf 
[Paths]
Prefix=/usr
ArchData=lib/x86_64-linux-gnu/qt5
Binaries=lib/qt5/bin
Data=share/qt5
Documentation=share/qt5/doc
Examples=lib/x86_64-linux-gnu/qt5/examples
Headers=include/x86_64-linux-gnu/qt5
HostBinaries=lib/qt5/bin
HostData=lib/x86_64-linux-gnu/qt5
HostLibraries=lib/x86_64-linux-gnu
Imports=lib/x86_64-linux-gnu/qt5/imports
Libraries=lib/x86_64-linux-gnu
LibraryExecutables=lib/x86_64-linux-gnu/qt5/libexec
Plugins=lib/x86_64-linux-gnu/qt5/plugins
Qml2Imports=lib/x86_64-linux-gnu/qt5/qml
Settings=/etc/xdg
Translations=share/qt5/translations
xjf1127@DESKTOP-E1M5RHH:/usr/lib/x86_64-linux-gnu/qt5$ ls 
bin  examples  libexec  mkspecs  plugins  qml  qt.conf
xjf1127@DESKTOP-E1M5RHH:/usr/lib/x86_64-linux-gnu/qt5$ qmake -query
QT_SYSROOT:
QT_INSTALL_PREFIX:/usr
QT_INSTALL_ARCHDATA:/usr/lib/x86_64-linux-gnu/qt5
QT_INSTALL_DATA:/usr/share/qt5
QT_INSTALL_DOCS:/usr/share/qt5/doc
QT_INSTALL_HEADERS:/usr/include/x86_64-linux-gnu/qt5
QT_INSTALL_LIBS:/usr/lib/x86_64-linux-gnu
QT_INSTALL_LIBEXECS:/usr/lib/x86_64-linux-gnu/qt5/libexec
QT_INSTALL_BINS:/usr/lib/qt5/bin
QT_INSTALL_TESTS:/usr/tests
QT_INSTALL_PLUGINS:/usr/lib/x86_64-linux-gnu/qt5/plugins
QT_INSTALL_IMPORTS:/usr/lib/x86_64-linux-gnu/qt5/imports
QT_INSTALL_QML:/usr/lib/x86_64-linux-gnu/qt5/qml
QT_INSTALL_TRANSLATIONS:/usr/share/qt5/translations
QT_INSTALL_CONFIGURATION:/etc/xdg
QT_INSTALL_EXAMPLES:/usr/lib/x86_64-linux-gnu/qt5/examples
QT_INSTALL_DEMOS:/usr/lib/x86_64-linux-gnu/qt5/examples
QT_HOST_PREFIX:/usr
QT_HOST_DATA:/usr/lib/x86_64-linux-gnu/qt5
QT_HOST_BINS:/usr/lib/qt5/bin
QT_HOST_LIBS:/usr/lib/x86_64-linux-gnu
QMAKE_SPEC:linux-g++
QMAKE_XSPEC:linux-g++
QMAKE_VERSION:3.1
QT_VERSION:5.15.3
```
从上述正常pc qt5的路径反推, 各个相对路径和qmake和qt.conf的关系，
```bash
[EffectivePaths]
Prefix=/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
[DevicePaths]
Prefix=/usr
Headers=include/qt5
Plugins=lib/qt/plugins
Examples=lib/qt/examples
[Paths]
Prefix=/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2
Headers=include/qt5
Plugins=plugins
Examples=examples
HostPrefix=/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host
# Sysroot=/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/arm-buildroot-linux-gnueabihf/sysroot
Sysroot=/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/buildroot/host/arm-buildroot-linux-gnueabihf/sysroot
# SysrootifyPrefix=true
# SysrootifyPrefix=true
# QMAKE_XSPEC=mkspecs/devices/linux-buildroot-g++
# TargetSpec=/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/mkspecs/devices/linux-buildroot-g++
# HostSpec=/home/xjf1127/codespace/toolchain/rk1126_sdk_vunknow/qt5/qt5base-5.14.2/mkspecs/linux-g++
TargetSpec=devices/linux-buildroot-g++
HostSpec=linux-g++
```
上诉配置，纯命令行qmake可用,并且qtcreator也能正常识别，得出结论，qmake根据source目录，默认目录下存在mkspec的，所以填写target时默认以mkspec为相对路径根目录



#### 思路解释

有接近一半的精力在梳理整个工程的环境依赖，这一阶段，我觉得对于我，这个项目的后续主要参与的维护者，至关重要； 决定了在关键时刻，能不能快速进行项目配置的响应，以及依赖关系的分析，甚至很多诡异型的bug。
1. 为什么要整理不同的构建方式，首先最基本的思路是保证原有项目，原汁原味的构建，在此基础上进行工程代码结构优化，以及技术选型，将会受到构建方式影响，可以更显式且简约的依赖管理； 比如cmake? 但项目涉及到很多第三方库时，它的优势就会特别突出，并且基于纯命令行的方式，一份cmake可以构建不同平台的目标 同种构建工具的命令行方式与gui方式差别不大，仅仅在于qtcreator端需要进行部分信息提前配置，然后由客户端指定部分缺省行为，比如输出目录

然后使用cmake构建目前看来是最为清爽的，我相信这也是qt官方主推cmake的一个重要原因，就是关于项目的所有配置都通过cmakelist进行统一配置，不会像qmake那样分散，涉及到多环节的环境变量配置