---
id: 9ossexjc90o5iblqoc7b5le
title: Camera_takephoto_jpeg_block
desc: ''
updated: 1742870273238
created: 1742527134741
---

```bash
#启动程序后
Get One Photo OK: ./save_pic/takephoto_pic_20250321_111532557_342 [size:474542]
end signal capture one:  "./save_pic/takephoto_pic_20250321_111532666_343"
will signal capture one:  "./save_pic/takephoto_pic_20250321_111532778_344"
Get One Photo OK: ./save_pic/takephoto_pic_20250321_111532666_343 [size:473323]
end signal capture one:  "./save_pic/takephoto_pic_20250321_111532778_344"
will signal capture one:  "./save_pic/takephoto_pic_20250321_111532883_345"
Get One Photo OK: ./save_pic/takephoto_pic_20250321_111532778_344 [size:473556]
end signal capture one:  "./save_pic/takephoto_pic_20250321_111532883_345"
will signal capture one:  "./save_pic/takephoto_pic_20250321_111532989_346"
takephoto模块执行getone到 346张时卡死
复现了10次左右，基本上都是300张左右就会出现卡死
从代码现象上看，是后期进入获取jpg回调时，拿到的buffer为0

```

### 需要确认问题

0. 磁盘空间

```bash
[root@RV1126_RV1109:/oem/xjf1127/save_pic]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       944M  421M  461M  48% /
devtmpfs        366M     0  366M   0% /dev
tmpfs           367M     0  367M   0% /dev/shm
tmpfs           367M  612K  366M   1% /tmp
tmpfs           367M  296K  366M   1% /run
/dev/mmcblk0p7  492M  488M     0 100% /oem
/dev/mmcblk0p8  5.6G  3.6M  5.6G   1% /userdata
```

可以发现是ome目录下空间不足，问题来了，空间不足，为什么接口会阻塞?
猜测: 首先getone阻塞是由于前一次getone互斥锁未释放，该锁由package_cb中的两个条件分支释放，一个是进入回调的jpegbuff是为空，则释放，另一个是jpegbuff非空，且判断getone标志为true时，进行文件写入，标志重置为false，结束后释放。极有可能是文件处理过程中发生了异常抛出，导致经过第二个释放分支，抛出异常退出了当前作用域，没执行到最后释放操作
验证: 对该代码端补加异常捕获，在异常处理的地方释放锁, 看看是否阻塞


1. 是在哪一环节出现问题，是VI还是VENC
   1. 增加每一阶段的状态信息打印

2. 

### testshuit 新增功能

1. 记录进入硬编码器次数


### 操作记录

```bash
## 原工程，没有修改代码，默认持续编码器处理
## 使用编码器参数:take_photo->startTakePhotosProcess(1, 3840, 2160, 20, true);
## 取土的间隔: 100ms
## 本地路径: /home/xjf1127/codespace/git_down/new_aplus/test/takephoto/data/save_pic2
[root@RV1126_RV1109:/oem/xjf1127]# chmod 777 * && ./indep_takephoto --save_pic_d
ir_path=/userdata/save_pic

df -h
剩余5G

```

```bash
## 原工程，没有修改代码，默认持续编码器处理 ,基础上对保存文件部分加了异常保护，专门测试空间不足下的阻塞原因
## 使用编码器参数:take_photo->startTakePhotosProcess(1, 3840, 2160, 20, true);
## 取土的间隔: 100ms
## 本地路径: /home/xjf1127/codespace/git_down/new_aplus/test/takephoto/data/save_pic2
[root@RV1126_RV1109:/oem/xjf1127]# chmod 777 * && ./indep_takephoto --save_pic_d
ir_path=/userdata/save_pic

[root@RV1126_RV1109:/userdata/save_pic]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       944M  421M  461M  48% /
devtmpfs        366M     0  366M   0% /dev
tmpfs           367M     0  367M   0% /dev/shm
tmpfs           367M  472K  366M   1% /tmp
tmpfs           367M  296K  366M   1% /run
/dev/mmcblk0p7  492M  339M  127M  73% /oem
/dev/mmcblk0p8  5.6G  5.6G   19M 100% /userdata

### 这部分未补加异常处理，仅补加了打印
will signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827111_0.jpg"
end signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827111_0.jpg"
will signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827206_1.jpg"
===> current photo file process begin, filename:/userdata/save_pic/takephoto_pic_20250321_154827111_0.jpg
Get One Photo OK: /userdata/save_pic/takephoto_pic_20250321_154827111_0.jpg [size:506577]
end signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827206_1.jpg"
will signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827448_2.jpg"
===> end file process and takeMutex.unlock(), correspond filename: /userdata/save_pic/takephoto_pic_20250321_154827206_1.jpg
===> current photo file process end, filename:/userdata/save_pic/takephoto_pic_20250321_154827206_1.jpg
===> current photo file process begin, filename:/userdata/save_pic/takephoto_pic_20250321_154827206_1.jpg
Get One Photo OK: /userdata/save_pic/takephoto_pic_20250321_154827206_1.jpg [size:455219]
end signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827448_2.jpg"
will signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827560_3.jpg"
===> end file process and takeMutex.unlock(), correspond filename: /userdata/save_pic/takephoto_pic_20250321_154827448_2.jpg
===> current photo file process end, filename:/userdata/save_pic/takephoto_pic_20250321_154827448_2.jpg
===> current photo file process begin, filename:/userdata/save_pic/takephoto_pic_20250321_154827448_2.jpg
Get One Photo OK: /userdata/save_pic/takephoto_pic_20250321_154827448_2.jpg [size:600676]
end signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827560_3.jpg"
will signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg"
===> end file process and takeMutex.unlock(), correspond filename: /userdata/save_pic/takephoto_pic_20250321_154827560_3.jpg
===> current photo file process end, filename:/userdata/save_pic/takephoto_pic_20250321_154827560_3.jpg
===> current photo file process begin, filename:/userdata/save_pic/takephoto_pic_20250321_154827560_3.jpg
Get One Photo OK: /userdata/save_pic/takephoto_pic_20250321_154827560_3.jpg [size:504255]
end signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg"
will signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_154827793_5.jpg"
===> end file process and takeMutex.unlock(), correspond filename: /userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg
===> current photo file process end, filename:/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg
===> current photo file process begin, filename:/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg
===> current photo file process end, filename:/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg
===> current photo file process begin, filename:/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg
===> current photo file process end, filename:/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg
===> current photo file process begin, filename:/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg
===> current photo file process end, filename:/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg
===> current photo file process begin, filename:/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg
===> current photo file process end, filename:/userdata/save_pic/takephoto_pic_20250321_154827680_4.jpg

## 关键代码段分析
void TakePhoto::photos_packet_cb(MEDIA_BUFFER mb)
{
    if (!mb) {
        usleep(50000);
        takeMutex.unlock();
        return;
    }

    if (pThis->isGetOnePo) {
        try {
            qDebug("===> current photo file process begin, filename:%s\n", pThis->catchPhotoName.toLatin1().data());
            QFile file(pThis->catchPhotoName);
            if (file.open(QIODevice::WriteOnly)) {
                file.write((char *)RK_MPI_MB_GetPtr(mb), RK_MPI_MB_GetSize(mb));
                file.flush();
                file.close();
                pThis->isGetOnePo = false;
                qDebug("Get One Photo OK: %s [size:%d]", pThis->catchPhotoName.toLatin1().data(), int(RK_MPI_MB_GetSize(mb)));
                takeMutex.unlock();
                qDebug("===> end file process and takeMutex.unlock(), correspond filename: %s", pThis->catchPhotoName.toLatin1().data());
            }   
        }
        catch (std::exception& e) {
            qDebug("===> save file error, enter exception\n");
        }
        qDebug("===> current photo file process end, filename:%s\n", pThis->catchPhotoName.toLatin1().data());
        // takeMutex.unlock();
    }

    RK_MPI_MB_ReleaseBuffer(mb);
}
### 可以确定是发生了异常，但无法被捕获，导致在获取编码数据的回调中重复进入，发生异常， flag和mutex都没有重置
排查方式1: 在疑似发生异常的前后加入打印，确定异常位置，try后也需要打印，确定回调后续的调用情况，是否会重复
排查方式2: gdb, 给代码端头尾中间打上断点，然后模拟之前的日志流程，先疯狂continue，随后一定次数后，在开头断点，进行按行 next 可以发现会直接跳过代码，发送异常，并且不执行异常代码
b local_takephonto.cpp: 31
b local_takephonto.cpp: 34
b local_takephonto.cpp: 48
b local_takephonto.cpp: 51
info threads
c
c
c
n
n

结论: 磁盘空间不足，导致的读写操作抛出异常，但异常无法捕获，无法进入catch代码段，导致 flag和mutex没有重置，导致getone接口阻塞



```


```bash
case:
else if (cmd_param.run_case_id == 2) {
    TakePhoto* take_photo = new TakePhoto{};
    return capture_suit(argc, argv, cmd_param, 0
        /**init */
        , [=]() {
        }
        /*uninit*/
        , [=]() {
        }
        /*takeone*/
        , [=](const QString& photo_name) {
            QTime time;
            time.start();
            take_photo->startTakePhotosProcess(1, 3840, 2160, 20, true);
            take_photo->setTakePhotosEnable(true);
            int elapsed = time.elapsed();  // 获取毫秒数
            qDebug("===> camera init process cost time: %d ms", elapsed);

            QTime time2;
            time2.start();
            take_photo->getOnePhoto(photo_name);
            take_photo->getOnePhoto(photo_name);/**call twice for ensure phototake one */
            elapsed = time2.elapsed();  // 获取毫秒数
            qDebug("===> getOnePhoto cost time: %d ms", elapsed);

            QTime time3;
            time3.start();
            take_photo->destroyTakePhotosProcess();//会崩溃
            take_photo->setTakePhotosEnable(false);//正常
            elapsed = time3.elapsed();  // 获取毫秒数
            qDebug("===> destroyTakePhotosProcess cost time: %d ms", elapsed);
        }
    );
}

## 原工程，没有修改代码逻辑，默认持续编码器处理 , 统计
## 使用编码器参数:take_photo->startTakePhotosProcess(1, 3840, 2160, 20, true);
## 取土的间隔: 200ms
## 拍照方式，每次拍照走初始化，反初始化一次
## 本地路径: /home/xjf1127/codespace/git_down/new_aplus/test/takephoto/data/save_pic3
## 命令行:
[root@RV1126_RV1109:/oem/xjf1127]#  chmod 777 * && ./indep_takephoto --save_pic_d
ir_path=/userdata/save_pic --run_case_id=2 --takephoto_interval_ms=200
输出图片对应pc上的存储路径/home/xjf1127/codespace/git_down/new_aplus/test/takephoto/data/save_pic3

[root@RV1126_RV1109:/userdata/save_pic]# rm -rf ./*
[root@RV1126_RV1109:/userdata/save_pic]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       944M  421M  461M  48% /
devtmpfs        366M     0  366M   0% /dev
tmpfs           367M     0  367M   0% /dev/shm
tmpfs           367M  552K  366M   1% /tmp
tmpfs           367M  296K  366M   1% /run
/dev/mmcblk0p7  492M  340M  127M  73% /oem
/dev/mmcblk0p8  5.6G  4.4M  5.6G   1% /userdata

===> camera init process cost time: 550 ms
===> getOnePhoto cost time: 420 ms

## 发现:当前的接口设计，如果只调用一次 takephoto，只修改了flag，尤其是初始化后,进入rk回调耗时较长，此时立刻进行第一次调用并不能立刻获取图片，除非连续两次调用，利用互斥锁，保证第一次获取到图片后，阻塞接口才退出
在最顶层调用的是mediamodule的 takeOnePhoto()
点击运动过程中
limitswi->MainWindow::tmpHandleMotorUpdate->MainWindow::updateMotor->add_node = p_poll->TakePhotoContinual(true);

rk_poll->run->while->



最终发现: destroyTakePhotosProcess内部会直接终止进程
### 改进思路: 
当前主进程对ipc相机的调用都是异步的，而相机初始化和资源释放，耗时基本在半秒

1. 思路1: 接口实现优化，在
```

case 3:
```bash
[root@RV1126_RV1109:/oem/xjf1127]# 
chmod 777 * && ./indep_takephoto --save_pic_d
ir_path=/userdata/save_pic --run_case_id=3 --takephoto_interval_ms=400
else if (cmd_param.run_case_id == 3) {
    return capture_suit(argc, argv, cmd_param, 0
        /**init */
        , [=]() {
        }
        /*uninit*/
        , [=]() {
        }
        /*takeone*/
        , [=](const QString& photo_name) {
            QTime time;
            takeone_all_step(photo_name.toStdString());
            int elapsed = time.elapsed();  // 获取毫秒数
            qDebug("===> process cost time: %d ms", elapsed);
        }
    );
}


### gdb无法定位到赋值的语句
(gdb) info line main.cpp:207
Line 207 of "../o_indep_call_takephoto/main.cpp"
   is at address 0x19f84 <std::_Function_handler<void(const QString&), main(int, char**)::<lambda(const QString&)> >::_M_invoke(const std::_Any_data &, const QString &)+116>
   but contains no code.


### 加入isp代码后
call case id: 3
will signal capture one:  "/userdata/save_pic/takephoto_pic_20250321_202735799_0.jpg"
ID: 0, sensor_name is m01_f_sc8238 1-0030, iqfiles is /etc/iqfiles
[20:27:36.212406][XCORE]:XCAM ERROR v4l2_device.cpp:196: open device() failed
[20:27:36.215406][CAMHW]:XCAM ERROR LensHw.cpp:199: get vcm cfg failed
[20:27:36.231480][XCORE]:XCAM ERROR CamHwIsp20.cpp:3751: |||set DC-Iris PwmDuty: 100
[20:27:36.231623][CAMHW]:XCAM ERROR LensHw.cpp:411: iris is not supported
[20:27:36.231723][XCORE]:XCAM ERROR v4l2_device.cpp:657: device(/dev/video10) start failed
[20:27:36.231783][CAMHW]:XCAM ERROR CamHwIsp20.cpp:3319: prepare isp params dev err: -9

Segmentation fault


```

```bash
#反复初始化-取土-反初始化之后，2000张左右，在后台跑时会自动关闭
[RKMEDIA][SYS][Info]:#V4L2Stream: v4l2 ctx reset to nullptr!
[RKMEDIA][VI][Info]:#SourceStreamFlow[SourceFlow:v4l2_capture_stream]: stream reset sucessfully!
[RKMEDIA][SYS][Info]:SourceFlow:v4l2_capture_stream quit
[RKMEDIA][SYS][Info]:RK_MPI_VI_DisableChn: Disable VI[0:1]:rkispp_scale0, 3840x2160 End...
rk_aiq_uapi_sysctl_stop enter
rk_aiq_uapi_sysctl_deinit enter
rk_aiq_uapi_sysctl_deinit exit
--------> after SAMPLE_COMM_ISP_Stop
===> process cost time: 903 ms
end signal capture one:  "/userdata/save_pic/takephoto_pic_20250324_183339991_2322.jpg"
mpp[3412]: mpp_buffer: ~MppBufferService cleaning misc group


takephoto_pic_20250325_100559631_2323.jpg
takephoto_pic_20250325_100600540_2324.jpg
takephoto_pic_20250325_100601449_2325.jpg
takephoto_pic_20250325_100602352_2326.jpg
takephoto_pic_20250325_100603318_2327.jpg
takephoto_pic_20250325_100604318_2328.jpg
takephoto_pic_20250325_100605317_2329.jpg
takephoto_pic_20250325_100606629_2330.jpg
takephoto_pic_20250325_100607534_2331.jpg
takephoto_pic_20250325_100608435_2332.jpg
takephoto_pic_20250325_100609338_2333.jpg
takephoto_pic_20250325_100610317_2334.jpg
takephoto_pic_20250325_100611318_2335.jpg
takephoto_pic_20250325_100612666_2336.jpg
takephoto_pic_20250325_100614374_2337.jpg
takephoto_pic_20250325_100615735_2338.jpg
takephoto_pic_20250325_100616641_2339.jpg
takephoto_pic_20250325_100618004_2340.jpg

end signal capture one:  "/userdata/save_pic/takephoto_pic_20250325_100616641_2339.jpg"
will signal capture one:  "/userdata/save_pic/takephoto_pic_20250325_100618004_2340.jpg"
--------> will SAMPLE_COMM_ISP_Init
ID: 0, sensor_name is m01_f_sc8238 1-0030, iqfiles is /etc/iqfiles
[10:06:18.048072][XCORE]:XCAM ERROR v4l2_device.cpp:196: open device() failed
[10:06:18.050612][CAMHW]:XCAM ERROR LensHw.cpp:199: get vcm cfg failed
[10:06:18.069399][XCORE]:XCAM ERROR CamHwIsp20.cpp:3751: |||set DC-Iris PwmDuty: 100
[10:06:18.069513][CAMHW]:XCAM ERROR LensHw.cpp:411: iris is not supported
[10:06:18.080694][CAMHW]:XCAM ERROR LensHw.cpp:296: focus is not supported
rk_aiq_uapi_sysctl_init/prepare succeed
rk_aiq_uapi_sysctl_start succeed
SAMPLE_COMM_ISP_SetFrameRate start 30
SAMPLE_COMM_ISP_SetFrameRate 30
[RKMEDIA][SYS][Info]:RK_MPI_VI_EnableChn: Enable VI[0:1]:rkispp_scale0, 3840x2160 Start...
[RKMEDIA][SYS][Info]:RKAIQ: parsing /dev/media0
media get entity by name: rkisp-mpfbc-subdev is null
media get entity by name: rkisp_dmapath is null
[RKMEDIA][SYS][Info]:RKAIQ: model(rkisp0): isp_info(0): isp-subdev entity name: /dev/v4l-subdev1
[RKMEDIA][SYS][Info]:RKAIQ: parsing /dev/media1
[RKMEDIA][SYS][Info]:RKAIQ: model(rkispp0): ispp_info(0): ispp-subdev entity name: /dev/v4l-subdev0
[RKMEDIA][SYS][Info]:#V4l2Stream: camraID:0, Device:rkispp_scale0
[RKMEDIA][SYS][Warn]:camera_id: 0, chn: rkispp_scale0
[RKMEDIA][SYS][Warn]:camera_id: 0, chn: rkispp_scale0, idx: 0
[RKMEDIA][SYS][Info]:#V4l2Stream: camera id:0, VideoNode:/dev/video14
Using mplane plugin for capture 
[RKMEDIA][SYS][Info]:#V4L2Ctx: open /dev/video14, fd 92
[RKMEDIA][SYS][Info]:RK_MPI_VI_EnableChn: Enable VI[0:1]:rkispp_scale0, 3840x2160 End...
[RKMEDIA][SYS][Info]:RK_MPI_VENC_CreateChn: Enable VENC[0], Type:7 Start...
[RKMEDIA][SYS][Info]:ParseMediaConfigFromMap: rect_x = 0, rect_y = 0, rect.w = 3840, rect.h = 2160 
mpp[3128]: mpp_info: mpp version: 57ff4c6b author: Herman Chen   2021-09-13 [cmake]: Enable HAVE_DRM by default
[RKMEDIA][VENC][Info]:MPP Encoder: MPPConfig: cfg init sucess!
[RKMEDIA][VENC][Info]:MPP Encoder[JPEG]: config for JPEG...
[RKMEDIA][VENC][Info]:MPP Encoder: dcf use default value:0
[RKMEDIA][VENC][Info]:MPP Encoder: mpf_cnt use default value:0
[RKMEDIA][VENC][Info]:MPP Encoder: rect_x use default value:0
[RKMEDIA][VENC][Info]:MPP Encoder: rect_y use default value:0
[RKMEDIA][VENC][Info]:MPP Encoder[JPEG]: rotaion = 0
[RKMEDIA][VENC][Info]:MPP Encoder[JPEG]: Set output block mode.
[RKMEDIA][VENC][Info]:MPP Encoder[JPEG]: Set input block mode.
mpp[3128]: mpp_enc: MPP_ENC_SET_RC_CFG bps 0 [0 : 0] fps [30:30] gop 0
mpp[3128]: jpege_api_v2: jpege_proc_jpeg_cfg qf_min out of range, default set 1
mpp[3128]: jpege_api_v2: jpege_proc_jpeg_cfg qf_max out of range, default set 99
[RKMEDIA][VENC][Info]:MPP Encoder[JPEG]: w x h(3840[3840] x 2160[2160]), qfactor:50
[RKMEDIA][SYS][Info]:RK_MPI_VENC_CreateChn: Enable VENC[0], Type:7 End...
>>>>> elapsed_init cost time: 160 ms
[RKMEDIA][SYS][Info]:RK_MPI_SYS_Bind: Bind Mode[VI]:Chn[1] to Mode[VENC]:Chn[0]...
>>>>> elapsed_register_cb_bind cost time: 0 ms
[RKMEDIA][SYS][Info]:Camera 0 stream 92 is started
===> OutCb, /userdata/save_pic/takephoto_pic_20250325_100618004_2340.jpg, cost time: 428
>>>>>> COST TIME MS: 443, wait ret: 1, taked_result: 0
[RKMEDIA][SYS][Info]:RK_MPI_SYS_UnBind: UnBind Mode[VI]:Chn[1] to Mode[VENC]:Chn[0]...
[RKMEDIA][SYS][Info]:RK_MPI_VENC_DestroyChn: Disable VENC[0] Start...
[RKMEDIA][SYS][Info]:VideoEncoderFlow quit
[RKMEDIA][VENC][Info]:MPP Encoder: MPPConfig: cfg deinit done!
[RKMEDIA][SYS][Info]:mpp destroy ctx done
[RKMEDIA][SYS][Info]:RK_MPI_VENC_DestroyChn: Disable VENC[0] End...
[RKMEDIA][SYS][Info]:RK_MPI_VI_DisableChn: Disable VI[0:1]:rkispp_scale0, 3840x2160 Start...
[RKMEDIA][VI][Info]:#SourceStreamFlow[SourceFlow:v4l2_capture_stream]: stream off....
libv4l2: error dequeuing buf: Invalid argument
[RKMEDIA][SYS][Info]:rkispp_scale0, ioctl(VIDIOC_DQBUF): Invalid argument
[RKMEDIA][VI][Info]:#SourceStreamFlow[SourceFlow:v4l2_capture_stream]: read thread exit sucessfully!
[RKMEDIA][SYS][Info]:#V4L2Ctx: close , fd 92
[RKMEDIA][SYS][Info]:#V4L2Stream: v4l2 ctx reset to nullptr!
[RKMEDIA][VI][Info]:#SourceStreamFlow[SourceFlow:v4l2_capture_stream]: stream reset sucessfully!
[RKMEDIA][SYS][Info]:SourceFlow:v4l2_capture_stream quit
[RKMEDIA][SYS][Info]:RK_MPI_VI_DisableChn: Disable VI[0:1]:rkispp_scale0, 3840x2160 End...
rk_aiq_uapi_sysctl_stop enter
rk_aiq_uapi_sysctl_deinit enter
rk_aiq_uapi_sysctl_deinit exit
--------> after SAMPLE_COMM_ISP_Stop
===> process cost time: 1375 ms
end signal capture one:  "/userdata/save_pic/takephoto_pic_20250325_100618004_2340.jpg"
mpp[3128]: mpp_buffer: ~MppBufferService cleaning misc group

# 需要验证是因为adb断开还是其他原因
从日志分析，并不是shell 断开导致的，是程序自动退出的，仅仅多出最后一句的析构日志，很有可能main退出时，触发的正常析构日志；-> 有可能是默认定时不够，刚好足够这些图片（确认验证成功）
#在终端 shell上启动的程序，如果shell进程被杀死，是否会导致该程序也被终止，是一个怎么样的过程
```

**耗时统计**
初始化部分: 160ms

```bash
SAMPLE_COMM_ISP_Init(0, hdr_mode, bMultictx, "/etc/iqfiles");
SAMPLE_COMM_ISP_Run(0);
SAMPLE_COMM_ISP_SetFrameRate(0, 30);
RK_MPI_VI_SetChnAttr(0, 1, &vi_chn_attr);
RK_MPI_VI_EnableChn(0, 1);
RK_MPI_VENC_CreateChn(0, &venc_chn_attr);
```

绑定通道后到成功进入回调: 430ms

```bash
ret = RK_MPI_SYS_Bind(&stSrcChn, &stDestChn);
-> 进入OutCb 回调
```

解除通道，反初始化资源: (900~1300)-600 = (300~)

```bash
RK_MPI_SYS_UnBind(&stSrcChn, &stDestChn);
RK_MPI_VENC_DestroyChn(0);
RK_MPI_VI_DisableChn(0, 1);
SAMPLE_COMM_ISP_Stop(0);
```
