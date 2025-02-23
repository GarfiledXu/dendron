---
id: ys7yy32asosk11dkodjk73g
title: Macro
desc: ''
updated: 1724405467282
created: 1724405361013
---

#### 宏定义的嵌套和展开
```c++
#pragma once
#include <string>
#include <stdexcept>

#define GF_EXPAND2(x) x
#define GF_GET_MACRO2(_1, _2, _3, _4, _5, _6, _7, _8, _9, _10, _11, _12, _13, _14, _15, _16, _17, _18, _19, _20, _21, _22, _23, _24, _25, _26, _27, _28, _29, _30, _31, _32, _33, _34, _35, _36, _37, _38, _39, _40, _41, _42, _43, _44, _45, _46, _47, _48, _49, _50, _51, _52, _53, _54, _55, _56, _57, _58, _59, _60,_61, NAME, ...) NAME

#define GF_PASTEE(...) GF_EXPAND2(GF_GET_MACRO2(__VA_ARGS__, \
        GF_PASTEE61, \
        GF_PASTEE60, \
        GF_PASTEE59, \
        GF_PASTEE58, \
        GF_PASTEE57, \
        GF_PASTEE56, \
        GF_PASTEE55, \
        GF_PASTEE54, \
        GF_PASTEE53, \
        GF_PASTEE52, \
        GF_PASTEE51, \
        GF_PASTEE50, \
        GF_PASTEE49, \
        GF_PASTEE48, \
        GF_PASTEE47, \
        GF_PASTEE46, \
        GF_PASTEE45, \
        GF_PASTEE44, \
        GF_PASTEE43, \
        GF_PASTEE42, \
        GF_PASTEE41, \
        GF_PASTEE40, \
        GF_PASTEE39, \
        GF_PASTEE38, \
        GF_PASTEE37, \
        GF_PASTEE36, \
        GF_PASTEE35, \
        GF_PASTEE34, \
        GF_PASTEE33, \
        GF_PASTEE32, \
        GF_PASTEE31, \
        GF_PASTEE30, \
        GF_PASTEE29, \
        GF_PASTEE28, \
        GF_PASTEE27, \
        GF_PASTEE26, \
        GF_PASTEE25, \
        GF_PASTEE24, \
        GF_PASTEE23, \
        GF_PASTEE22, \
        GF_PASTEE21, \
        GF_PASTEE20, \
        GF_PASTEE19, \
        GF_PASTEE18, \
        GF_PASTEE17, \
        GF_PASTEE16, \
        GF_PASTEE15, \
        GF_PASTEE14, \
        GF_PASTEE13, \
        GF_PASTEE12, \
        GF_PASTEE11, \
        GF_PASTEE10, \
        GF_PASTEE9, \
        GF_PASTEE8, \
        GF_PASTEE7, \
        GF_PASTEE6, \
        GF_PASTEE5, \
        GF_PASTEE4, \
        GF_PASTEE3, \
        GF_PASTEE2, \
        GF_PASTEE1)(__VA_ARGS__))

#define GF_PASTEE3(func, v1, v2) func(v1, v2)
#define GF_PASTEE5(func, v1, v2, v3, v4) GF_PASTEE3(func, v1, v2) GF_PASTEE3(func, v3, v4)
#define GF_PASTEE7(func, v1, v2, v3, v4, v5, v6) GF_PASTEE3(func, v1, v2) GF_PASTEE5(func, v3, v4, v5, v6)
#define GF_PASTEE9(func, v1, v2, v3, v4, v5, v6, v7, v8) GF_PASTEE3(func, v1, v2) GF_PASTEE7(func, v3, v4, v5, v6, v7, v8)
#define GF_PASTEE11(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10) GF_PASTEE3(func, v1, v2) GF_PASTEE9(func, v3, v4, v5, v6, v7, v8, v9, v10)
#define GF_PASTEE13(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12) GF_PASTEE3(func, v1, v2) GF_PASTEE11(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12)
#define GF_PASTEE15(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14) GF_PASTEE3(func, v1, v2) GF_PASTEE13(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14)
#define GF_PASTEE17(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16) GF_PASTEE3(func, v1, v2) GF_PASTEE15(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16)
#define GF_PASTEE19(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18) GF_PASTEE3(func, v1, v2) GF_PASTEE17(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18)
#define GF_PASTEE21(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20) GF_PASTEE3(func, v1, v2) GF_PASTEE19(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20)
#define GF_PASTEE23(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22) GF_PASTEE3(func, v1, v2) GF_PASTEE21(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22)
#define GF_PASTEE25(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24) GF_PASTEE3(func, v1, v2) GF_PASTEE23(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24)
#define GF_PASTEE27(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26) GF_PASTEE3(func, v1, v2) GF_PASTEE25(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26)
#define GF_PASTEE29(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28) GF_PASTEE3(func, v1, v2) GF_PASTEE27(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28)
#define GF_PASTEE31(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30) GF_PASTEE3(func, v1, v2) GF_PASTEE29(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30)
#define GF_PASTEE33(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32) GF_PASTEE3(func, v1, v2) GF_PASTEE31(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32)
#define GF_PASTEE35(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34) GF_PASTEE3(func, v1, v2) GF_PASTEE33(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34)
#define GF_PASTEE37(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36) GF_PASTEE3(func, v1, v2) GF_PASTEE35(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36)
#define GF_PASTEE39(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38) GF_PASTEE3(func, v1, v2) GF_PASTEE37(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38)
#define GF_PASTEE41(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40) GF_PASTEE3(func, v1, v2) GF_PASTEE39(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40)
#define GF_PASTEE43(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42) GF_PASTEE3(func, v1, v2) GF_PASTEE41(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42)
#define GF_PASTEE45(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44) GF_PASTEE3(func, v1, v2) GF_PASTEE43(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44)
#define GF_PASTEE47(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46) GF_PASTEE3(func, v1, v2) GF_PASTEE45(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46)
#define GF_PASTEE49(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48) GF_PASTEE3(func, v1, v2) GF_PASTEE47(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48)
#define GF_PASTEE51(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50) GF_PASTEE3(func, v1, v2) GF_PASTEE49(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50)
#define GF_PASTEE53(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50, v51, v52) GF_PASTEE3(func, v1, v2) GF_PASTEE51(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50, v51, v52)
#define GF_PASTEE55(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50, v51, v52, v53, v54) GF_PASTEE3(func, v1, v2) GF_PASTEE53(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50, v51, v52, v53, v54)
#define GF_PASTEE57(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50, v51, v52, v53, v54, v55, v56) GF_PASTEE3(func, v1, v2) GF_PASTEE55(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50, v51, v52, v53, v54, v55, v56)
#define GF_PASTEE59(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50, v51, v52, v53, v54, v55, v56, v57, v58) GF_PASTEE3(func, v1, v2) GF_PASTEE57(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50, v51, v52, v53, v54, v55, v56, v57, v58)
#define GF_PASTEE61(func, v1, v2, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50, v51, v52, v53, v54, v55, v56, v57, v58, v59, v60) GF_PASTEE3(func, v1, v2) GF_PASTEE59(func, v3, v4, v5, v6, v7, v8, v9, v10, v11, v12, v13, v14, v15, v16, v17, v18, v19, v20, v21, v22, v23, v24, v25, v26, v27, v28, v29, v30, v31, v32, v33, v34, v35, v36, v37, v38, v39, v40, v41, v42, v43, v44, v45, v46, v47, v48, v49, v50, v51, v52, v53, v54, v55, v56, v57, v58, v59, v60)
```
在上述的不定参宏实现中，为什么要定义一个#define GF_EXPAND2(x) x，通过GF_EXPAND2将传递的参数包裹一次呢？