---
id: 3jv4h8zgqm2byu79bptop4r
title: Testapp
desc: ''
updated: 1718093719268
created: 1715840296297
---

#### 重汽avm testapp

##### sdk 特性描述
- 纯算法，不包含渲染功能，输入图片原图，输出融合结果以及单路结果(分辨率降低)
- [ ] 授权模块
- [ ] 标定模块
  - [ ] 手动标定
  - [ ] 自动标定
  - [ ] 获取标定结果

##### 功能需求
  - [ ] 激活
    - [ ] 在线激活 ARC_AVM_Activate
    - [ ] 离线激活  ARC_AVM_ImportLicenseInfo
    - [ ] 获取激活结果 ARC_AVM_GetActivateStatus
    - [ ] 获取本地激活文件相关信息 ARC_AVM_GetActivateFileInfo
    - [ ] 获取离线授权文件相关信息 ARC_AVM_GetLicenseFileInfo
  - [ ] 初始化-反初始化
    - [ ] 初始化引擎 ARC_AVM_CreateHandle
    - [ ] 反初始化引擎 ARC_AVM_ReleaseHandle
  - [ ] AVM
    - [ ] avm 融合绘图 ARC_AVM_DrawImage
      - [ ] 显示
      - [ ] 保存绘图结果 ARC_AVM_GetDrawImageResult
    - [ ] avm 标定
      - [ ] 自动标定 ARC_AVM_AutoCalibrate
      - [ ] 手动标定 ARC_AVM_ManualCalibrate
      - [ ] 获取标定结果 ARC_AVM_GetCalibrateResults
    - [ ] avm 亮度 
      - [ ] 获取 ARC_AVM_GetImageBrightness
      - [ ] 设置 ARC_AVM_SetImageBrightness
    - [ ] avm 倒车线
      - [ ] 获取 ARC_AVM_GetAuxLineInfo
      - [ ] 设置 ARC_AVM_SetAuxLineInfo
    - [ ] 像素对应尺寸
      - [ ] 获取一个像素对应的尺寸 ARC_AVM_GetLengthPerPixel
    - [ ] 倒车线点位信息
      - [ ] AVM获取倒车线点位信息 ARC_AVM_GetReversingLinePoints
    - [ ] 车模框
      - [ ] 获取车模狂信息 ARC_AVM_GetCarModelRect
    
  - [ ] BSD
    - [ ] bsd 检测 ARC_AVM_BSD_Detect
    - [ ] bsd 标定 
      - [ ] 获取摄像头标定信息 ARC_AVM_BSD_GetCalibInfo
      - [ ] 设置摄像头标定信息 ARC_AVM_BSD_SetCalibInfo
    - [ ] bsd 报警
      - [ ] 获取摄像头报警参数 ARC_AVM_BSD_GetAlarmParams
      - [ ] 设置摄像头报警参数 ARC_AVM_BSD_SetAlarmParams
    - [ ] 版本信息 ARC_AVM_GetVersionInfo
    - [ ] 设置驾驶状态 ARC_AVM_BSD_SetDrivingStatus

##### jni 封装
- [ ] 接口封装策略
  - [ ] 简单的一层结构体，直接封装java对应对象
  - [ ] 复杂结构体并且调用频率低，如配置对象，直接传配置文件路径和读配置的方式
    - [ ] 将ARC_AVM_CreateHandle接口封装为 native层读配置文件进行初始化，java层传入配置文件路径
  - [ ] 调用频率高的结构体，细节封装
    - [x] ARC_AVM_ActiveEnvParam_t
    - [x] ARC_ImageInfo_t
    - [x] ARC_AVM_CalibData_t 
    - [x] ARC_AVM_ChessInfo_t
    - [x] ARC_2DSize_t
    - [x] ARC_AVM_CarInfo_t
    - [x] ARC_AVM_CalibInfo_t
    - [x] ARC_AVM_AuxLineInfo_t
    - [x] ARC_PointF_t
    - [x] ARC_Rect_t
    - [x] ARC_AVM_BSD_DrivingStatus_t
    - [x] ARC_BSD_AlarmParam_t
    - [x] ARC_BSD_MultiFrame_AlarmParam_t
    - [x] ARC_AVM_BSD_AlarmParam_t
    - [x] ARC_AVM_CameraType_t 枚举
    - [x] ARC_AVM_BSD_Alarm_t 枚举
    - [x] ARC_AVM_BSD_DetectResult_t
    - [x] ARC_AVM_BSD_AlarmDetail_t
    - [x] ARC_AVM_BSD_ObjectInfo_t
    - [x] ARC_AVM_BSD_CalibInfo_t
    - [x] ARC_AVM_InitParam_t
      - [x] ARC_AVM_CameraInfo_t
      - [x] ARC_AVM_IntrinsicParams_t
      - [x] ARC_BSD_CameraInfo_t
      - [x] ARC_AVM_CalibInfo_t
      - [x] ARC_AVM_CalibData_t
##### 接口功能描述
##### app sdk logic
- [x] 版本信息打印
- [ ] 空库开关
- [x] 激活
- [x] 初始化开关
- [x] avm
- [ ] bsd
- [ ] avm 标定
- [ ] bsd 标定


##### YaXunZhongQi
