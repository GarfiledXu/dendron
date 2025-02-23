---
id: tyondza55dnvusg16fxln5p
title: Source_set
desc: ''
updated: 1726048476138
created: 1726040157419
---

### ref
- [](https://blog.csdn.net/weixin_42944928/article/details/126564996)


### java 编译时忽略部分java文件
```gradle
sourceSets {
    main {
        jniLibs.srcDirs = ['libs']
        java {
            exclude 'com/example/nativeprj1/AvmSdkLogic/**'
        }
    }
}
```


### jar + so 组合如何导入模块进行引用
```gradle
dependencies {

    implementation files('libs/libarcsoft_visdrive_avm_engine.jar')
}
//同时把jar文件和so文件放到libs目录下
```


### cpp + jni 组合如何进行引用编译