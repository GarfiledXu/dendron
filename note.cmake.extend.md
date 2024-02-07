---
id: 3jzs9mvrlligrbphpzc4plf
title: Extend
desc: ''
updated: 1706607711965
created: 1699862132285
---

### demand
****
**sub module declare and refe**
    sub cmakelist macro should provide multi version for gen module for each target to add
    and each module corresponds to a dedicated macro
```cmake
#pseudo code 1, diff module name
generate_module("m_spdlog_lib" 
    SP_INC 
        ${CURRENT_PROJECT_PATH}/inc
        ${CURRENT_PROJECT_PATH}/impl
    SP_LIB 
        # ${CURRENT_PROJECT_PATH}/lib/libspdlog.a
        "NON"
    SP_SRC 
        # ${CURRENT_PROJECT_PATH}/src/fmt.cc
        # "NON"
        ${src_list}
    MACRO
        "NON"
) 
generate_module("m_spdlog_src" 
    SP_INC 
        ${CURRENT_PROJECT_PATH}/inc
        ${CURRENT_PROJECT_PATH}/impl
    SP_LIB 
        "NON"
    SP_SRC 
        ${src_list}
    MACRO
        -DSPDLOG_COMPILED_LIB
) 
```

```cmake
#pseudo code 2, only one name with diff modifiers for reference using
#declare
generate_module("m_spdlog" 
    SP_INC 
        ${CURRENT_PROJECT_PATH}/inc
        ${CURRENT_PROJECT_PATH}/impl
    SP_LIB 
        # ${CURRENT_PROJECT_PATH}/lib/libspdlog.a
        "NON"
    SP_SRC 
        # ${CURRENT_PROJECT_PATH}/src/fmt.cc
        # "NON"
        ${src_list}
    MACRO
        "NON"
    MODIFIER
        "lib_version"
) 
generate_module("m_spdlog" 
    SP_INC 
        ${CURRENT_PROJECT_PATH}/inc
        ${CURRENT_PROJECT_PATH}/impl
    SP_LIB 
        # ${CURRENT_PROJECT_PATH}/lib/libspdlog.a
        "NON"
    SP_SRC 
        # ${CURRENT_PROJECT_PATH}/src/fmt.cc
        # "NON
        ${src_list}
    MACRO
        "NON"
    MODIFIER
        "src_version"
) 
#using
add_single_module("m_spdlog" "src_version")
```

#### plan feature
1. module name optimize of reference and defined and using
引用key(target cmake list) + 定义时修饰(module cmakelist) + 引入时修饰(prj root cmake list)
针对同一个module(名), 要求能引用不同的来源，项目自带/本地公共/远程url,以及本地的其他路径，以命名空间 或者 作用域的方式进行名称修饰, 不同作用域使用不同的根目录解析方式
当前: 引入时，本质上不通过key，而是直接引入文件夹，避免了key关联的问题，

实现思路：无? 
2. 接口名称优化
refe module
using module
define module
3. module check
每次新增一个module都需要检查现有module，如果已存在，则冲突，则报错
4. 在每一个module定义时 允许添加对应的macro


##### plan 
###### 2024
1. 每一个子target cmakelist中很可能定义多个exe，那么需要设置公共的module add，而不是针对一个target