---
id: 1q52gov50bw6wwv3bazg7ai
title: Ydn_mainwindow
desc: ''
updated: 1743668729772
created: 1743578722565
---

### ?

1. 镜像版本和功能支持对照关系
2. flash写入配置文件?
3. env值含义?

    ```c
    enum {
        ENV_TEST,
        ENV_SAAS,
        ENV_DEMO,
        ENV_PRI
    };
    ```

[rk_pollevent.cpp]

1. enterAgemode 企业模式是什么?
2. fakeoff 伪关机模式是什么?
3. seal 是印章?
4. stampDistanceSet() 这个距离是什么意思?
5. nextStamp tmpTim autoStampStart 
6. qst_interrupt_handler 中 qst 是什么意思? 陀螺仪?
7. actStart 是什么意思? 盖章超时时间?
8. addinkStart 什么?加墨超时时间?
9. device_get 是什么玩意儿啊!!! 用于定时触发获取设备状态，并且进行更新?

10. is_v2_6pcb 是什么？v2.6版本pcb
11. 这里非 2.6pcb 以及 非企业模式下的执行条件是什么原因背景?

    ```bash
    if (userInfo.identity != 1)
        {
            if (!is_v2_6pcb)
            {
                BTreadOneMsg();
                if (!enterAgemode)
                {
                    CheckBlueTooth();
                }
            }
        }
    ```

12. rk_pollevent::checkIsPhotosSync中的逻辑状态值，是否高拍仪同步影像状态 什么意思? 默认是指无效初始值? /oem/gpy_sync 中gpy是什么含义? rk_pollevent::checkIsPhotosSync逻辑是否已经删除?

```bash
{
int isSyncMFile = 2;                    //是否高拍仪同步影像状态 0:同步完成 1:同步中/开始同步 2:默认
}_SystemStatusAttr;
```

13. 全局状态标志位
    1. mqttLink 成功连接到mqttserver，置为true
    2. isTravelEnd : TravelOfflineFile()执行完毕
14. 局部状态标志位: rk_event_poll
    1. isTravelEnd
    2. deviceOff 
15. 分析

    ```c++
    if (mqttLink && !deviceOff && isOperation == IDLE && isTravelEnd == false) {
        static int delays = 0;
        //3秒更新一次
        if ((uploadVideoNums != 0 || listNumber != 0) && delays++ > 30) {
            delays = 0;
            uploadtime = time(NULL);
        } else if (uploadVideoNums == 0 && listNumber == 0 && !is_timeout(uploadtime, 1800)) {
            delays = 0;
            uploadtime = time(NULL);
            emit stopTraversalPhotosListNodes();
        }
    }
    //isTravelEnd 是表示
    ```

16. 自动关机逻辑?