---
id: 40bk8k29plsbuwn8fdpjavq
title: 编译缓存系统分析
desc: ''
updated: 1772943947450
created: 1772941329023
---

## 分析cmake在进行编译时内部的缓存系统如何工作的，各文件作用顺序以及依赖关系

1. 基本工作流: 整个缓存文件夹包含编译中间文件以及cmake自身缓存文件提供给外层的最终makefile引用

```bash
xjf1127@MARK-I:~/wsl_workspace/prj/probes/exp_section$ tree -L 3 /home/xjf1127/wsl_workspace/prj/probes/build/exp_section
/home/xjf1127/wsl_workspace/prj/probes/build/exp_section
├── CMakeCache.txt
├── CMakeFiles
│   ├── 3.31.2
│   │   ├── CMakeCCompiler.cmake
│   │   ├── CMakeCXXCompiler.cmake
│   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   ├── CMakeSystem.cmake
│   │   ├── CompilerIdC
│   │   └── CompilerIdCXX
│   ├── CMakeConfigureLog.yaml
│   ├── CMakeDirectoryInformation.cmake
│   ├── Makefile.cmake
│   ├── Makefile2
│   ├── TargetDirectories.txt
│   ├── cmake.check_cache
│   ├── exp_section.dir
│   │   ├── DependInfo.cmake
│   │   ├── build.make
│   │   ├── cmake_clean.cmake
│   │   ├── compiler_depend.internal
│   │   ├── compiler_depend.make
│   │   ├── compiler_depend.ts
│   │   ├── depend.make
│   │   ├── flags.make
│   │   ├── link.txt
│   │   ├── main.c.i
│   │   ├── main.c.o
│   │   ├── main.c.o.d
│   │   ├── main.c.s
│   │   └── progress.make
│   ├── pkgRedirects
│   └── progress.marks
├── Makefile
└── cmake_install.cmake

6 directories, 29 files
```

`CMakeCache.txt`配置缓存，记录cmake参数（可以以用于判断是否参数生效被解析），记录检测结果
`Makefile` 顶层构建入口，会引用缓冲中的cmake文件
`CMakeFiles/3.22.1` 编译器和系统检测（通过这个查看编译触发的编译器和系统环境是否正确）
`<target.dir>/*` 下的文件主要是缓冲中间产物，导出build.make用于顶层引用

```bash
# cmake基于target作为构建逻辑单元进行构建，makefile是具体的语法实体，所以第一层作为入口，第二层则是target的关系依赖组织，第三层就是具体的target编译内容，其中makefile2中的数字即关系层次，当然如此命名也有一定的历史因素，所以保留
Makefile
   ↓
CMakeFiles/Makefile2
   ↓
CMakeFiles/<target>.dir/build.make
   ↓
gcc / linker
```
