---
id: 5dfmhga42km2jadtzh03y8y
title: Api_test_tool_readme
desc: ''
updated: 1692341483477
created: 1692340619771
---

### C281 API TEST EXE ~xjf2613~
-----------------
#### pakcage
- c281_api_test_android
    > 可执行程序
- 1280x800.NV21 && 1280x960.NV21
  > 测试用的图片
- normal_license
  > 存放正常license
- *.so
  > 使用的so库

#### usage
- 整个文件夹放到设备可运行目录
- chmod a+x *
- 输入命令执行可执行程序，运行case:
    - **./c281_api_test_android --gtest_filter=stress_init_uninit*normal1**
    > 只进行初始化-反初始化循环的case, 默认20分钟测试时间
    - **./c281_api_test_android --gtest_filter=stress_init_uninit*normal2**
    > 只进行初始化-process一次-反初始化循环的case, 默认20分钟测试时间
    - **./c281_api_test_android --gtest_filter=stress_init_uninit*normal3**
    > 只进行process循环的case, 默认20分钟测试时间
    
#### result
- **tmp**
  > 存放临时文件，不必理会
- **record 文件夹**
    > 存放抓取的内存数据，文件名与case名相同



