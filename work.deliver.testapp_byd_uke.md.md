---
id: qi4t5bgg3jwzy5b3bx3y477
title: Testapp_byd_uke
desc: ''
updated: 1698736369420
created: 1698736106227
---
### project
byd uke testapp

环境依赖：
	sdk对外包中的算法库及相关依赖（arc_cfg.json + license.json）
	testApp的配置文件 app_config.ini、option.json
	ArcDump库（depends下的libArcDump.so）
	字库 DejaVuSans-Bold.ttf、Microsoft-YaHei-Bold-Font.ttf

使用步骤：
	1、创建testApp运行目录，将bins下对应的可执行程序testApp放到App运行目录下
	2、将depends下的所有文件放到testApp运行目录下
	3、导入依赖库的路径 export... ；将arc_cfg.json, license.json等放到配置路径下,修改权限
	4、先运行testApp
	5、再运行testApp_hmi交互进程，同testApp进行交互，输入值见下方
		[1 str]			截图拍摄+自定义字段
		[2 str] 		定时录制+自定义字段
		[3 str] 		自定义时长录制+自定义字段
		[4 value]  		设置速度
		[5 value]		设置变道信号
		[6 value]  		设置离手信号
		[7 value]  		设置noa信号
		[8 value]  		设置方向盘转向角信号（弧度）
		[9 value]  		设置方向盘转角速度信号（弧度/s）
		[10 value]		设置转向信号
		[11]  			退出
	
配置文件格式(app_config.ini)：
[config]
license_vin_str
	license的vin码
license_file_path
	授权文件位置
video_save_path
	视频存储路径
pic_save_path：
	当前画面保存和屏幕截图存储路径
log_save_path
	log存储路径
DMS_drowsy_switch
	疲劳开关
DMS_distract_switch
	分心开关
DMS_covered_switch
	遮挡开关
result_stay_time                                     
	画面打印信息停留时间，单位s
video_record_duration                                
	默认视频保存时长，单位s
DMS_camera_input_id
	DMS镜头input ID 

=====================================================================================
V1.0.20821.30 -- 20231025
Add:
    fc95fef..d6c1fd9
	1、更新视线宏定义
	2、增加driverstatus的dms结果显示


V1.0.20821.17 -- 20230329
Add:
	1、新增对内输出，持续闭眼时长


V1.0.20821.16 -- 20230323
Add:
	1、新增对内输出，比亚迪和C3方案对内结果根据可配参数切换显示
	2、增加中文显示


V1.0.20821.15 -- 20230316
Add:
	1、取消授权文件联网校验


V1.0.20821.14 -- 20230315
Add:
	1、新增对内输出参数：手机使用分心的累积时长


V1.0.20821.13 -- 20230313
Add:
	1、新增对内输出参数：短时分心的累积时长、长时分心的累积时长


V1.0.20821.11 -- 20230309
Fix:
	1、修复读license文件的问题


V1.0.20821.9 -- 20230303
Fix:
	1、修复视线0值误输出invalid的问题


V1.0.20821.8 -- 20230228
Add:
	1、更新授权文件校验流程；
	2、更新视线区域及对应icon,增加视线值的显示；3
	3、增加外部信号传入SDK；
	4、新增外部传参，需要更新app_config.ini；
	5、新增拉流的FPS显示；
	6、其他问题修改


V1.0.20821.6 -- 20230223
Add:
	1、修复对内结果显示偶现不输出的问题（降低刷新频率）


V1.0.20821.5 -- 20230215
Add:
	1、更新对内结果显示逻辑


V1.0.20821.4 -- 20230209
Add:
	1、增加对内版本，对内版本增加中间结果输出显示
	
	
V1.0.20821.3 -- 20230203
Add:
	1、更新视线


V1.0.20821.2 -- 20230202
Add:
	1、修改参数


V1.0.20821.0 -- 20230118
Add:
	1、比亚迪STEBU车型testApp初始化版本