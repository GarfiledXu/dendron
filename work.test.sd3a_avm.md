---
id: jn9bm2bg3317sdqhpji34fp
title: Sd3a_avm
desc: ''
updated: 1696755760333
created: 1696647772372
---

#### single
1.	avm_sdk_PDCdistance
radar_info异常值输入，返回值为0
2.	avm_sdk_steeringangle
angle_Info异常值输入，返回值为0MOK(0)；
3.	avm_sdk_transparent
AVM车模透明度不在0~1之间，返回值为0
4.	avm_sdk_wheelpulse
pulse_infor脉冲值为负数，返回值为0
5.	avm_sdk_wheelspeed
speed_info_ptr轮速值为负数，返回值为0

#### calib
##### normal1 
pass
##### normal2
pass
##### abnormal3
block in ret = avm_sdk_calib(sdk_handle, AVM_REQUEST_CALIB_ENTER, nullptr); and ret = avm_sdk_calib(sdk_handle, (AvmCalibrationRequest)-1, 0);