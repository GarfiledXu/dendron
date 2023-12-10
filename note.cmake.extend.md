---
id: 3jzs9mvrlligrbphpzc4plf
title: Extend
desc: ''
updated: 1699863369538
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
        # "NON"
        ${src_list}
    MACRO
        "NON"
    MODIFIER
        "src_version"
) 
#using
add_single_module("m_spdlog" "src_version")
```