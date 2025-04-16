---
id: 1q52gov50bw6wwv3bazg7ai
title: Ydn_mainwindow
desc: ''
updated: 1744348651717
created: 1743578722565
---
### 关注点

1. 主体逻辑
2. 根据功能点来查找对应实现
   1. 电池电量的显示和处理逻辑
3. 各模块成功 失败条件以及现象和打印
4. 关注不同模块涉及的文件，路径，地址等
5. 废弃的代码块

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


17. 电池电量状态机
    1. 电池电量计算方式

    ```c++
        //获取原始数据
        getDevCap = readParaFromDev("/sys/class/power_supply/bq27542-0/voltage_now"); // 读取电压（单位一般是微伏 uV）
        getDecur = readParaFromDev("/sys/class/power_supply/bq27542-0/current_now"); // 电流（单位一般是微安 uA）
        //根据充放电状态进行电压补偿
        //充电时：电压因为电流输入而偏高 → 减掉一部分电流的影响
        //放电时：电压因为负载拉低 → 加上一点补偿
        //0.12f 为经验系数
        if (device_sta->isCharge) {
            getDevCap = getDevCap - 0.12f * getDecur;
        } else {
            getDevCap = getDevCap + 0.12f * abs(getDecur);
        }
        //进行电压和电量映射
        getDevCap = VolTransCapacity(getDevCap);
        //进行10次采样取平均值
        sampCapacity[10];
    ```

    2. 疑惑

    ```c++
    //在客户日志中，为什么没有qWarning("LOW POWER capcity <= 35")，noPower = true;是生效的
        static bool lowPow_warn = false;
        if (device_sta->capcity <= 40 && !lowPow_warn)
        {
            // 低电量预警阈值40%
            PlaySound(SealAudioLower);
            lowPow_warn = true;
        }
        else if (device_sta->capcity <= 35 /*|| readParaFromDev((char *)"/sys/class/power_supply/bq27542-0/voltage_now") <= 3600000) */ && device_sta->isCharge == false)
        {
            // 自动关机电量阈值35%
            noPower = true;
            qWarning("LOW POWER capcity <= 35");
        }
        else if (is_file_exist("/oem/LOWPOWER"))
        {
            qWarning("LOW POWER TEST &************************$%");
            noPower = true;
        }
    ```
18. userInfo.remote 取值 1 2 3 4 5分别是什么意思

    ```c++
        if (p_poll->userInfo.remote == 1)
        {
            p_poll->isOperation = STAMPING; // 非远程盖印
        }
        else if (p_poll->userInfo.remote == 2)
        {
            p_poll->isOperation = WAITEXAMINE; // 远程ads
        }
        else if (p_poll->userInfo.remote == 3)
        {
            qDebug("[DEBUG]:  RKMEDIA_CATCH READY  [%d]", p_poll->isOperation);
            qDebug("[DEBUG]:  RKMEDIA_CATCH READY  [%d]", p_poll->isOperation);
            qDebug("[DEBUG]:  RKMEDIA_CATCH READY  [%d]", p_poll->isOperation);
            qDebug("[DEBUG]:  RKMEDIA_CATCH READY  [%d]", p_poll->isOperation);
            qDebug("[DEBUG]:  RKMEDIA_CATCH READY  [%d]", p_poll->isOperation);
            if (p_poll->isOperation != STAMP_CONSECUTIVE)
            {
                p_poll->isOperation = STAMP_CONSECUTIVERDY; // 连续盖印远程

            }
            DisplayStamping();
        }
        else if (p_poll->userInfo.remote == 4)
        {
            qDebug("+++++++++++++++++++++++++++++++进入等待认证盖印模式(remote=4)+++++++++++++++++++++++++++++++");
            p_poll->isOperation = STAMP_WAITTING_COMMAND; // 未知盖印模式
        }

        if (userInfo.remote == 1)
        {
            isOperation = STAMPING; // 非远程盖印
            PlaySound(SealUnlocked);
        }
        else if (userInfo.remote == 2)
        {
            isOperation = PASSEXAMINE; // 远程
            PlaySound(DeviceuNLocked);
        }
        else if (userInfo.remote == 3)
        {
            PlaySound(DeviceuNLocked);
            isOperation = STAMP_CONSECUTIVERDY; // 连续
        }
        else if (userInfo.remote == 4)
        {
            // 	isOperation = STAMP_REMOTE_RDY; // 连续盖印远程
            // 	isRemote = true;
            // 	PlaySound(DeviceuNLocked);
            // }
            // else if (userInfo.remote == 5)
            //{
            isOperation = STAMP_WAITTING_COMMAND;   // 连续盖印远程
            //PlaySound(DeviceuNLocked);/*范围用印的解锁及审判指令未下发前不播放语音*/             
            //	isRemote = true;
        }
        Q_EMIT pMediaModule->setTakePhotosEnabled(true);
    ```


### deprecated code block


### 主界面元素


```c++

```

### publish_env 版本切换逻辑(company,government)

1. mqtt 密钥逻辑? mqtt connect时需要密码和clientid，根据不同的mqtt环境(company和government)会生成不同的psw和clientid，即当触发了环境（政务和企业）切换时，需要生成对应的secret，用来重新连接mqtt
   1. 为什么 mqtt`"secret"` 的主题定义为 TOPIC_SECERT_ISSUE???
   2. 关键的`secret`来源，通过`JsonParseMqttKey(data, sysInfo.mqttKey);`同步到`sysInfo.mqttKey`
   3. SwitchToCompany(DEVICE_NUMBER, sysInfo.mqttKey); || rk_pollevent::SwitchToGovement(DEVICE_NUMBER, sysInfo.mqttKey) -> CreatMqttPassword(username, key, passwd, clientId, true); 用于连接mqtt使用

   ```c++
    //最终反哺到dev_info中, 在mqttwork连接时使用
    //company
    strcpy(dev_info.device_number, username);
    strcpy(dev_info.mqtt_paswd, passwd);
    strcpy(dev_info.clenitId, clientId);
    strcat(mqttuser, "&yda");
    strcpy(dev_info.mqtt_user, mqttuser);
    SetSNandMQTTinfo(&sysInfo);//并且每次本地进行同步配置信息
    //govement
    strcpy(dev_info.device_number, username);
    strcpy(dev_info.mqtt_paswd, passwd);
    strcpy(dev_info.clenitId, clientId);
    strcat(mqttuser, "&yda_gov");
    strcpy(dev_info.mqtt_user, mqttuser);
    SetSNandMQTTinfo(&sysInfo);
   ```

2. mqtt publishenv切换的触发上游?
   1. 默认 publishEnv 为1
   2. AnalsyMqttServerMsg 接收到mqtt_msg: topic:DEVICE_RESET("reset") 此主题为设备初始化时会进行
   3. 提取`cutContext`+`secret`字段决定版本，`cutState`字段决定环境
   4. 判断到与publishEnv不同则进行切换
3. 切换后的下游 mqtt的网络重连
   1. 判断 needRecnt == true
   2. 先 MQTTClient_disconnect
   3. mqtt_sta = MQTT_RESET
   4. p_mqtt->anlsyMsgOK = true
   5. MQTT_RESET-> 在mqttwork的事件循环中存在定时的状态轮询，判断到 MQTT_RESET 会触发 20s的sleep等待解绑 -> 最终触发重连
   6. MQTT_RESET->在mainwindow中会触发 :/picture/1111bak.png + 正在连接服务器
4. mqttwork的anlsyMsgOK的逻辑?
    anlsyMsgOK 控制消息处理的状态，每次获取到消息，重置为false，表示消息未被handle，超时时间定为25秒，超过25则自动设置为true，为true表示当前已处理完一条消息，则可以从消息队列中拿取

   ```c++
   //接收消息回调，只要
    static time_t anlsy_now;
    if (!anlsyMsgOK) {
        if (!is_timeout(anlsy_now, 25)) {
            qWarning("[MQTT] wait msg dispose Timeout/Error!");
            anlsyMsgOK = true;
        }
    }

    if (recvcnt > 0 && anlsyMsgOK)
    {
        anlsyMsgOK = false;
        int mainNums = mainQueues.size();
        int otherNums = documentStateQueues.size();
        struct recvmsg_queue analsy_queue = {0};
        if (mainNums > 0) {
            queueMutex.lock();
            analsy_queue = mainQueues.dequeue();
            queueMutex.unlock();
        } else {
            if (otherNums > 0) {
                queueMutex.lock();
                analsy_queue = documentStateQueues.dequeue();
                queueMutex.unlock();
            }
            else {
                return;
            }

        }

        time(&anlsy_now);
        emit handleOneMqttMsg(analsy_queue);
        recvcnt--;
    }
    //每次处理完消息则手动置为true
    emit publish_Message_signal_1(QString("/sys/%1/%2/service/%3/invoke_reply").arg(topicType).arg(DEVICE_NUMBER).arg(TOPIC_USER_LOGIN), datas);
    p_mqtt->anlsyMsgOK = true;
    
   ```

5. mqttwork中的status: MQTT_NOEMPOW
    也是属于mqtt unready的状态
    会在mqttwork的事件循环中的定时状态轮询中触发重连

   ```c++
   // =
    int ret = connectMqttServer();
    if (ret == 4 || ret == 3) {
        mqttLink = 0;
        mqtt_sta = MQTT_NOEMPOW; // 没有绑定企业   没有权限
    }
    // == 
    //在mainwindow中会显示对应的提示: 请绑定设备，即判断到设备没有绑定到企业
    if (p_poll->device_sta->mqttsta == MQTT_NOEMPOW) // TODO
    {
        img->load(":/picture/1111bak.png");
        sprintf(text, "请绑定设备");
    }
   ```

6. `needRecnt`是什么JB命名? reconnectd???
7. 前面的mqtt设备绑定逻辑是什么?

### 环境切换逻辑(测试，演示，正式)

p_poll->environment GlobalModel::instance()->getDeviceConfigAttribute()->envir p_poll->sysInfo.envir

0. 关联模块: 设备，系统信息配置模块，
1. envir 的作用点
   1. 在rk_nodethread中，通过int envir = GlobalModel::instance()->getDeviceConfigAttribute()->envir;获取到，决定素材上传时的协议以及具体url

   ```c++
    int envir = GlobalModel::instance()->getDeviceConfigAttribute()->envir;
    QString upload_api = GlobalModel::instance()->getDeviceConfigAttribute()->api;
    QString url;
    if (envir == ENV_TEST) {
        url = (outInfo.video_property==PRINT_SEAL ? "http://ydatestvideo.libawall.com/equipment/video/upload" : "http://ydatestvideo.libawall.com/equipmentVideo/equipment/ink/video/upload");
    } else if (envir == ENV_SAAS) {
        url = (outInfo.video_property==PRINT_SEAL ? "http://video.yda.cn:19082/equipment/video/upload" : "http://video.yda.cn:19082/equipmentVideo/equipment/ink/video/upload");
    } else if (envir == ENV_DEMO) {
        url = (outInfo.video_property==PRINT_SEAL ? "http://preydaapi.libawall.com/equipmentVideo/equipment/video/upload" : "http://preydaapi.libawall.com/equipmentVideo/equipment/ink/video/upload");
    } else if (envir == ENV_PRI) {
        if (upload_api.contains("ydatestapi")) {
            if (upload_api.contains("https")) {
                url = (outInfo.video_property==PRINT_SEAL ? "https://ydatestvideo.libawall.com/equipment/video/upload" : "https://ydatestvideo.libawall.com/equipmentVideo/equipment/ink/video/upload");
            } else {
                url = (outInfo.video_property==PRINT_SEAL ? "http://ydatestvideo.libawall.com/equipment/video/upload" : "http://ydatestvideo.libawall.com/equipmentVideo/equipment/ink/video/upload");
            }
        } else {
            url = upload_api + (outInfo.video_property==PRINT_SEAL ? "/equipmentVideo/equipment/video/upload" : "/equipmentVideo/equipment/ink/video/upload");
        }
    }
   ```
   
   2. 在global_model中，进场初始化时，会进行配置同步，判断到私有化环境并且api支持https时，赋值`is_https_api`，m_sysStatusAttr.hasHttps = true,
   在mainwindow初始化的末尾会重新判断一下环境，再将该标志位置为true

   ```c++
   if (strstr(p_poll->sysInfo.priApi, "https:") && p_poll->sysInfo.envir == 3)
    {
        is_https_api = true;
    }
   ```

   ```c++
   //在mainwindows中决定 指纹相关的上传下载
    if (is_https_api)
    {
        connect(p_poll, &rk_pollevent::curlHttpsFinishFingerUpload, this, &MainWindow::handleCurlHttpsFinishFingerUpload);
    }
    //在mainwindows中决定下载固件包
    void MainWindow::downloadFile(char *url)
    {
        qDebug("[HTTP] Download App Recv URL: %s", url);

        ui->label->setPixmap(QPixmap::fromImage(QImage(":/picture/upgrade.png")));
        //pox_system("");
        if (is_https_api)
        {
            time(&p_poll->httpsDownloadTimes);
            GlobalModel::instance()->getSystemStatus()->isHttpsUpgrading = true;
            o_http->httpsCurlGetUpgradeApp(url);
        }
        else
        {
            qDebug("recv UEL: %s", url);
            ConstructGet(url);
        }
    }
    //在mainwidnow中决定finger相关的处理函数
    //保存用户位置，发布消息
    MainWindow::getFingerFeature{
        if (is_https_api)
        {
            qDebug("all finger import OK ![%d]", Info.cnt);
            p_poll->isOperation = IDLE;
            char superUserPos[512] = {0};
            SaveSuperUserPostion(p_poll->sysInfo.SuperUserSpec, superUserPos);
            printf("Save Spuer user Line : %s \n", superUserPos);
            SetLocalInfo(&p_poll->sysInfo);
            // p_poll->AskFingerFollow();
            char payloadd[120] = {0};
            JsonReqFingerList(payloadd, 0);
            qDebug("[HTTPS] %s\n\n", payloadd);
            p_poll->MqttPublish(payloadd, FINGER_IMPORT);
            // qDebug("[FINGER] finger Syci compelete![%d]",isOperation);
            qDebug("[HTTPS] ASK MESG SENT!!!");
            if (p_poll->sysInfo.stampInstall && !p_poll->isLoging)
            {
                if (!p_poll->isLoging && strlen(p_poll->sysInfo.stampName) > 0)
                {
                    P_figroll->figOption = FIG_MATCH;
                }
            }
        }
    }
    
   ```

   3.在uploadwork中影响 UploadWorker::checkWebLinkStatus(int position) 使用的url

   ```c++
    void UploadWorker::checkWebLinkStatus(int position)
    {
        cWebPosition = position;
        int envir = GlobalModel::instance()->getDeviceConfigAttribute()->envir;
        QString url;
        if (envir == ENV_SAAS) {
            url = "http://apiv2.yda.cn:18082/equipment/link";
        } else if (envir == ENV_DEMO) {
            url = "http://preydaapi.libawall.com/equipment/link";
        } else if (envir == ENV_PRI) {
            QString api = GlobalModel::instance()->getDeviceConfigAttribute()->api;
            if (api.contains("https")) { api.replace("https", "http");}
            url = QString("%1%2").arg(api).arg("/equipment/link");
        } else {
            url = "http://ydatestapi.libawall.com/equipment/link";
        }

        QNetworkRequest request;
        request.setUrl(QUrl(url));
        m_accessManager->get(request);
    }
   ```

   4. 影响mainwindwos中fingerfeature 上传和下载功能

   ```c++
   MainWindow::UploadFeaturetoCloud()
   MainWindow::getFingerFeature(gt_fingerImport *importInfo)
   ```

   5. 决定了mqttwork中server的ip_port

   ```c++
   //1. 在mainwindow中构造初始化 mqtt_work 时，会先通过sysinfo赋值私有化 ip_port
   m_ip_port = QString(sysinfo->priIp);
   //2. 在连接服务器时通过 envir 选用其他ip_port
   if (envir == 0) m_ip_port = ADDRESS_TEST;
    else if (envir == 1) m_ip_port = ADDRESS;
    else if (envir == 2) m_ip_port = ADDRESS_DEMO;
    else if (envir == 3) { }
   ```

   那么问题来了，私有化的ip_port来源是?
   1. 默认来源 由 /oem/sys.info 下的ip_port进行初始化
   2. 在void rk_pollevent::Ble_ENVCHANGE_Dispose_V1(char* msg, char* replymsg)-> 提取消息priApiser和priIpandPort，publishEnv 同步到sysinfo中
   3. Ble_ENVIRONMENT_Dispose_V1 和 Ble_ENVCHANGE_Dispose_V1 什么区别?

   ```c++
   
   ```

### 蓝牙数据流

0. 初始化

1. 资源释放

2. 在wlwork模块中定义的
   1. 发: sendBtData 依赖 RK_BleWriteData
      1. 在wlanwork中处理wifi配置时，使用sendBtData进行报文响应
      2. 同时信号转发到上层的rk_pollevent::sendBtData，但是并没有使用该函数进行蓝牙保温数据发送???
   2. 收: handleBTCommand
      1. 处理wifi配置，`WIFILIST`，`WIFISETTING`
      2. 发送信号 `recvBtData`，在rk_pollevent中定义数据处理的回调

!!! 上下游搞反了
3. 在rk_pollevent中封装逻辑
    1. 发
        1. 下游实现逻辑: 根据硬件版本的不同，`is_v2_6pcb` 使用不同的蓝牙收发方式
            1. `is_v2_6pcb`: RK_BleWriteData(channel==1?BLE_UUID_WIFI_CHAR:BLE_UUID_SEND, ...)，这里传参的uuid根据蓝牙协议版本而定
            2. `!is_v2_6pcb`: WriteComPort(ttyBl,...) 串口蓝牙
        2. 中游实现分为了
            1. RK_BTSEND bt_send_p 拥有队列
            2. rk_pollevent::sendn() 直接发送
                1. rk_pollevent::Ble_ENVIRONMENT_Dispose_V1 环境切换协议处理时，末尾直接发送 `sendn((char *)"/>|22|ENVIRONMENT|OK|#")`
                2. rk_pollevent::UpdateLimitSwitch(unsigned char sta) 在isBlueTooth的情况下，会回复"INKING"，通过`sendn()`
                3. rk_pollevent::blebuffer_reply(char* buffer) -> rk_pollevent::sendn();**该函数用于指纹数据上传，在独立线程执行，被mainwindo层引用**
            3. rk_pollevent::Ble_Reply() -> rk_pollevent::sendn() || bt_send_p; 封装了不同协议版本的指令回复，内部实现结束指令使用`sendn`回复，其他的使用`bt_send_p.Control_Of_Btsend`按序排队发送
        3. 上游发送数据的场景都是 调用Ble_Reply进行发送
    2. 收
       1. 最下游，统一的msg处理: rk_pollevent::anlasyBTmsg(char *msg)；readmsg(buff)负责处理测试指令
          1. 解析指令，调用对应的处理函数
       2. 上游，`!is_v2_6pcb` 触发 rk_pollevent的循环中，rk_pollevent::BTreadOneMsg() -> anlasyBTmsg(): 读取串口tty，最终进行 anlasyBTmsg 业务流程指令处理或者 readmsg(buff) 进行测试指令处理
       3. 中游，`is_v2_6pcb` 触发 wlwork的事件循环中，rk_pollevent::BtHandleMsg()，该数据流，通过wlwork的模块，两层信号转发，最终将模块的蓝牙数据发送到这里
          1. wlworker: emit recvBtData(); 事件循环
          2. netmodule: connect(m_wlanWorker, &WlanWorker::recvBtData, this, &NetworkModule::recvBtData); 无事件循环
          3. mainwindow: connect(m_networkModule, &NetworkModule::recvBtData, p_poll, &rk_pollevent::BtHandleMsg); ui事件循环
          4. 信号转发方向: WlanWorker::handleBTCommand-> emit WlanWorker::recvBtData(cmd, len) -> NetworkModule::recvBtData -> rk_pollevent::BtHandleMsg()
          5. BtHandleMsg 最终执行线程: wlworker的事件循环

### mqtt数据流

0. 初始化
1. 资源释放
2. 重连
3. 消息主题订阅
4. 接收消息处理
   1. 下游，mqttwork: 模块初始化时，开启事件循环，500ms定时执行状态机，当网络状态`LINED`时，触发`MqttOperationWorker::updateRecvNodeFront()`，消息处理状态条件+发送数据`emit handleOneMqttMsg(analsy_queue)`
   2. 中游，信号转发，mainwindow: connect(m_mqtt, &MqttOperationWorker::handleOneMqttMsg, p_poll, &rk_pollevent::DisposeSubcribeMsg2);  
   3. 上游，msg消息处理，rk_pollevent::DisposeSubcribeMsg2 -> `rk_pollevent::AnalsyMqttServerMsg(char *getdata, char *gettopic)`，解析主题和报文
   4. 信号转发方向: MqttOperationWorker::updateRecvNodeFront()->emit MqttOperationWorker::handleOneMqttMsg(analsy_queue)->rk_pollevent::AnalsyMqttServerMsg()
   5. rk_pollevent::AnalsyMqttServerMsg 最终执行线程: mqtt work的事件循环
5. 发送消息
   1. 下游，mqttwork，存在两个版本
      1. MqttOperationWorker::publishMessage() 没有重试，会等待消息发送的结果，成功或者超时
      2. MqttOperationWorker::publishTopicIdMessage_1() 直接发送，没有等待消息发送的结果的步骤
   2. 中游，信号转发，mainwindow
        connect(p_poll, &rk_pollevent::publish_Message_signal, m_mqtt, &MqttOperationWorker::publishTopicIdMessage); MqttOperationWorker拥有事件循环线程
        connect(p_poll, &rk_pollevent::publish_Message_signal_1, m_mqtt, &MqttOperationWorker::publishTopicIdMessage_1);
   3. 上游，rk_pollevent 核心包装为 `MqttPublish`
      1. rk_pollevent::MqttPublish(QString data, int topicNumber) -> MqttOperationWorker::publishTopicIdMessage 会等待消息发送的结果
      2. 直接调用信号 emit publish_Message_signal_1() 直接发送，仅在发送用户登录消息以及空白报文时，调用，其他均使用MqttPublish
         1. emit publish_Message_signal_1(QString("/sys/%1/%2/service/%3/invoke_reply").arg(topicType).arg(DEVICE_NUMBER).arg(TOPIC_USER_LOGIN), datas);
         2. char dataa[480] = {0}; JsonNormRespon(dataa, 0, this_msgid/*msgid*/); emit publish_Message_signal_1(ttopic, dataa);
   4. 上上游，mainwindow 会调用 rk_pollevnet::MqttPublish()
   5. 信号转发方向:  rk_pollevent::MqttPublish -> rk_pollevent::publish_Message_signal -> MqttOperationWorker::publishTopicIdMessage
   6. 最终执行线程: mqtt work的事件循环
   
6. 其他细节条件

### http 数据流

1. 相关源码文件
   1. media/uploadwork.cpp
      1. UploadWorker::uploadLogFile(QByteArray devSn) 上传日志，使用QHttpMultiPart
      2. UploadWorker::uploadPhotoFiles UploadWorker::uploadVideoFiles， 如果是https，使用curl，如果是http，使用http
      3. UploadWorker::uploadFingerDatas 同样
   3. global/QRcode/http_opt.cpp 被mainwindow引用
      1. http_opt::PostLocalLog deprecated
      2. mainwindow.cpp -> o_http->isCheckHttpsCurlExecOK() 检查curl功能
      3. mainwindow.cpp -> http_opt::httpsCurlPostFingerData 发送指纹数据，条件: 私有化环境`ENV_PRI`并且api contains`https`，否则将使用 m_mediaModule->uploadFingerDatas() http上传
      4. mainwindow.cpp -> o_http->httpsCurlGetFingerData() 下载指纹数据，缓存到临时文件`/tmp//tmp/fingerprint_%d.txt`，条件:私有化环境`ENV_PRI`并且api contains`https`，否则将使用`QNetworkRequest`，MainWindow::ConstructGet-->receiveReply(QNetworkReply *) ->MainWindow::handleHttpNetworkReply 
   4. rk_pollevent.cpp 引用
   5. rk_nodethread.cpp 引用
   6. mainwindow.cpp 封装http操作，引用https
      1. connect(manager, SIGNAL(finished(QNetworkReply *)), this, SLOT(receiveReply(QNetworkReply *))); 
   7. globalmodel.h 定义相关配置的数据结构

### 指纹数据的传递

0. 指纹模块的手册?
1. 指纹线程
   1. p_poll->sysInfo.stampInstall == 1 || strlen(p_poll->sysInfo.stampName) > 0 时，开启指纹线程
   2. 状态机

   ```plantuml
    @startuml
    [*] --> Idle

    Idle --> Register : figOption = FIG_REG
    Idle --> Match : figOption = FIG_MATCH

    Register --> Capturing : Start Finger Capture
    Capturing --> Capturing : ErollFp() Success
    Capturing --> Idle : ErollFp() Fail
    Capturing --> Finish : ErollFp() Complete

    Finish --> Idle : Emit displaySig(FIG_FINISH)

    Match --> Searching : Start Finger Match
    Searching --> Searching : FignerSeachFp() Success
    Searching --> Idle : FignerSeachFp() Fail
    Searching --> MatchSuccess : FignerSeachFp() Found

    MatchSuccess --> Idle : Emit displaySig(getId)

    Idle --> Quit : figOption = FIG_QUIT
    Quit --> [*]

    @enduml
   ```

   ```c++
   //指纹操作，启动指纹线程时带入，以及状态机自身状态驱动
   typedef enum {FIG_QUIT=-2,FIG_WAITUPLOAD=-3,FIG_PAUSE=-4,FIG_TRANSFER= -5,FIG_MATCH=0,FIG_REG=1}FINGER_OPT;
   //操作过程的状态和结果，用于指纹线程向mainwindow发送状态，用于交互
   enum {FINGER_NOIMG=-11,FINGER_REPEAT=-3,MATCH_FALSE=-2,MATCH_TRUE=-1,FRST_PUT=-4,SECOND_PUT=-5,THIRD_PUT=-6,FIG_FINISH=-7,ACT_DOING=-8,ACT_IDLE=-9,MATCH_PUT=-10};
   ```

   3. 指纹线程的控制
      1. 默认的指纹验证线程，mainwindow connect大部分信号和槽之后，会开启 指纹线程进行 FIG_MATCH相关逻辑的循环

        ```c++
        //初始化
        if (p_poll->sysInfo.stampInstall == 1 || strlen(p_poll->sysInfo.stampName) > 0)
        {
            qDebug("SEALNAME:%s sysinfo[%d]", p_poll->sysInfo.stampName, p_poll->sysInfo.stampInstall);
            P_figroll = new figThread(FIG_MATCH);
            connect(P_figroll, &figThread::displaySig, this, &MainWindow::DisplayFigerOperation);
            connect(P_figroll, &figThread::finishSig, this, &MainWindow::TerminaFigThread);
            connect(P_figroll, &figThread::timeoutResetMatchStatus, this, [&] {p_poll->fignerWait = false; });

            P_figroll->start();
            isFigThreadon = true;
        }
        //结果状态处理
        switch (type)
        { // enum {FINGER_NOIMG=-11,FINGER_REPEAT=-3,MATCH_FALSE=-2,MATCH_TRUE=-1,FRST_PUT=-4,SECOND_PUT=-5,THIRD_PUT=-6,FIG_FINISH=-7,ACT_DOING=-8,ACT_IDLE=-9,MATCH_PUT=-10};

        case MATCH_FALSE:
            ui->label_progress->hide();
            DisplayMatchFalse();
            break; // 指纹认证失败。未录入
        // case MATCH_TRUE:
        //     DisplayCheckFinger();break; //指纹认证通过
        case FRST_PUT:
            ui->label_progress->hide();
            DisplayEnteringFig(FRST_PUT);
            break;
        case SECOND_PUT:
            DisplayEnteringFig(SECOND_PUT);
            break;
        case THIRD_PUT:
            DisplayEnteringFig(THIRD_PUT);
            break;
        case FIG_FINISH:
            DisplayEnteringFig(FIG_FINISH);
            break;
        default:
            DisposeFingerResult(type);
            break;
        }
        ```

        1. 在 FIG_MATCH 循环中:
            1. 联网状态下:
                1. 匹配到指纹: FignerSeachFp(&getId) >= 1 -> EmitDisplaySignal(getId) -> emit displaySig(type) 指纹匹配成功最终会将匹配到的id发送到 MainWindow::DisplayFigerOperation 槽函数
                2. 发布mqtt指纹信息，更新pollevent状态: checkWebServerStatus(int) -> emit MainWindow::alreadyCheckWebServerStatus(int) --> MainWindow::onAlreadyCheckWebServerStatus(int) -> 条件:!p_poll->isLoging && p_poll->isOperation == IDLE -> ReportUserputFinger(int)->publishTopicIdMessage(FIG_LOG, ..)，p_poll->fignerWait = true等待服务器响应返回用户信息
                3. 接收到mqtt 主题TOPIC_FINGER_RESPONE，用户指纹登陆，返回消息(条件: 未登录 指纹已经验证并等待服务器下发指纹等级)->提取userInfo，
                   1. false: 若解析失败则或者解析成功但是没有用印权限-->触发ui提示(UI_FIGREFUSE) 用户未授权,请前往APP发起
                   2. true: 触发语音播报:SealUnlocked(印章已解锁，请点击按钮开始用印)
                4. 判断是否为特权用户以及盖印模式
                   1. 若是连续盖印则 打开舱门，并设置 盖印状态机 状态为 `isOperation = STAMP_CONSECUTIVERDY`，等待盖印状态机进行消费
                   2. 若是常规盖印则 直接设置盖印状态为`STAMPING`，等待盖印状态机消费
                   3. 使能拍照
                   4. emit mainwindow::updateLabel(UI_FIGPASS) -> MainWindow::DisposeFingerRespone(true): 触发ui提示，界面更新(解锁成功，请用印 + 印章名称)
                5. 最终执行线程: mainwindow事件循环
            2. 在断网状态下:
               1. 指纹状态机线程 匹配到指纹: FignerSeachFp(&getId) >= 1 -> EmitDisplaySignal(getId) -> emit displaySig(type) 指纹匹配成功最终会将匹配到的id发送到 MainWindow::DisplayFigerOperation 槽函数
               2. 调用方向: MainWindow::DisposeFingerResult(pos) -> MainWindow::checkWebServerStatus(int) -> emit MainWindow::alreadyCheckWebServerStatus(int) --> MainWindow::onAlreadyCheckWebServerStatus(int) ->MainWindow::ReportUserputFinger(pos) -> 判断条件(isWebOff || !mqttLink) && (p_poll->sysInfo.SuperUserSpec[pos] == 1)
                  1. 触发语音播报:SealUnlocked(印章已解锁，请点击按钮开始用印) 
                  2. RecycleFingerThread() 回收指纹线程
                  3. 判断盖印模式
                        1. 若是连续盖印则 打开舱门，并设置 盖印状态机 状态为 `isOperation = STAMP_CONSECUTIVERDY`，等待盖印状态机进行消费
                        2. 若是常规盖印则 直接设置盖印状态为`STAMPING`，等待盖印状态机消费
                  4. 使能拍照功能
                  5. emit mainwindow::updateLabel(UI_FIGPASS) -> MainWindow::DisposeFingerRespone(true): 触发ui提示，界面更新(解锁成功，请用印 + 印章名称)

        2. 什么情况下 p_poll->isLoging 为true?
        3. userinfo更新后是否会和本地同步?
        4. p_poll->userInfo.identity=1 特权用户什么时候被赋值?
        5. sysInfo.islocked 什么时候被改变?
        6. fingerWait 什么意思? 指纹验证成功后,等待服务器信息同步
        7. UI_FIGREFUSE 是什么操作?

        

      2. mainwindow封装FingerOperation(int mode)，创建指纹线程并连接相关信号，并作为槽函数提供给pollevent的信号进行触发，rk_pollevent::FingerMode(int)
      3. mainwindow封装TerminaFigThread()->RecycleFingerThread()，销毁指纹线程，并作为槽函数提供给pollevent的信号进行触发，rk_pollevent::stopFigopt(void)
      4. figThread::makeFeatureCode() 用来获取指纹特征数据，通过手动指定 `figThread::postion` 和 `figThread::feature`，在上传指纹特征时使用
      5. 核心的触发操作
         1. 为什么FIG_MATCH的结果去掉了MATCH_TRUE，而是MATCH_FALSE
         2. ErollFp的第三个参数notRealseTime，仅是外部传入的控制标志位，为1时，及时CommSingleInstruction成功也返回错误码-11，
         3. new figThread(FIG_REG)
            1. FINGER_MODE_WAKE-->mainwindow:display(FRST_PUT)[提示请入录指纹]->
         4. new figThread(FIG_MATCH)



   2. mqtt接收到指纹导入消息，转化为gt_fingerImport数据结构，
   3. 指纹标志位 position是什么
   4. 指纹录入需指纹校验
   5. 什么是指纹录入权限
   6. 突破口，查看特权用印流程

   ```c++
    int  fingertype;/*指纹录入模式*//*1：普通录入，2:指纹删除,3：录入验证*/ 指纹导入 指纹删除 指纹权限变更 指纹数据清空
    int  position;/*指纹录入位置*/
    int  auth;/*指纹录入权限*/
    typedef struct gt_fingerImport
    {
        int cnt ;
        int totalCnt;
        int recvCnt;
        int cloudId[21];
        int positon[21];
        int type[21];
        int auth[21];
    }gt_fingerImport;

    typedef struct gt_enterInfo
    {
        int type;
        int position ;
        int auth;
    }gt_enterInfo;
   ```

### motor

1. motorswitch 封装了电机状态响应操作
   1. 一个状态机后台线程，不断获取限位开关状态，判断当前开关状态组合变化，以及电机运动状态(外部输入)，来触发对应的电机动作，转化电机运动状态
   2. 上层调用: motorswitch模块创建并运行-->其他模块: 执行电机动作宏，向 motorswitch 传入电机运动状态 --> motorswitch 状态机线程根据 开关组合状态 + 电机运动状态 + 业务标志位，触发对应操作
   3. 信号方向
      1. rk_pollevent触发电机复位(远程锁定，用户登出，TOPIC_STAMPCONTI_PAUSE，用印退出，TOPIC_CANCEL_OPT，TOPIC_ADD_INKRES加墨，设备重置，焕章): emit: rk_pollevent::MotorReset() -> motorswitch::MotorPositReset() -> 设置标志位，等待motorswitch状态机线程处理
      2. rk_pollevent执行电机操作宏后，设置电机运动状态: emit: rk_pollevent::stampSport(unsigned char) -> motorswitch::GetStampSportDirect(unsigned char)
      3. rk_pollevent调整印章伸出长度: emit: rk_pollevent::stampDistanceSet(int) -> motorswitch::SetSealDisdtance(int)
      4. rk_pollevent触发电机状态机线程重新检测标志(开关状态组合发生变化 || 重检标志==true): emit: rk_pollevent::Refresh_Motor_Status(void) -> motorswitch::Refresh_Motor_Status(void)

### https 数据流

### 通讯硬件模块

### 机械结构硬件模块


### 业务逻辑流程

### 远程唤醒逻辑

### mqtt消息 收发流程



### 文件路径

"/oem/LOWPOWER" 由驱动创建? 

### 拍照录像时机

1. 拍照 2
   1. `TakePhotoContinual`拍照封装于 rk_pollevent中，进行拍照，并且返回一个node用于上传
   2. 盖印过程中，设置needphoto标志位 在rk_pollevent中
      1. TOPIC_EXAMINE_RESULT 审批结果处理
      2. userInfo.remote == 4 处于远程盖印
   3. 更新limitswitch限位开关 在mianwindows中
2. 开始录像 five
   1. `Q_EMIT pMediaModule->setRecordVidesEnabled(true, ...)`
   2. 蓝牙v1，加墨时录制  （<btl_msg>need_video && <系统镜像>2.4v ）使用gt_video_record_info.info.businessKey = time_str 作为生成视频的key
   3. 蓝牙v2，`STARTSTAMP` && isLoging -> userInfo.businessKey != "0"  ,使用userInfo.businessKey 作为生成视频名称的key
   4. mqtt_msg_anaysis -> JsonParseBeginStamp(., gt_userInfo) or JsonParseFinger(., gt_userInfo) -> update: userInfo.businessKey
   5. mqtt, 未登录 主题正确 指纹已经验证并等待服务器下发指纹等级(TOPIC_FINGER_RESPONE) -> has_videoRecord && userInfo.businessKey > 1，使用userInfo.businessKey作为生成视频名称的key
   6. mqtt, 开始盖章 必须在登陆的状态下 但不能是指纹登陆(TOPIC_BEGIN_SEAL) -> has_videoRecord && userInfo.businessKey > 1 ,使用userInfo.businessKey 作为生成视频名称的key
   7. mqtt，加墨(TOPIC_ADD_INK)，默认不录制，-> <mqtt_msg>need_video && is_can_record, 使用userInfo.businessKey 作为生成视频名称的
3. 结束录像 

### module test

   1. 基本的网络模块控制，初始化，消息收发，模块卸载
   2. 拍照 录像模块



### 心得

恍然发现，缺乏源码阅读的训练，读得没有章法，没有效率
根据日志，反向去对比源码




#### struct specification


```c++
typedef struct gt_userInfo//指纹登陆或者APP登陆共用结构体
{
    int identity;   //1-特权用户 2-普通登录用户 4-蓝牙登录用户
	int status ; //1.可以用印 2.不可用印
    int documentId ; //文件id
    int coverId;
    char stampName[620];
	char staffName[512];
	int sealType; //印章类型 : 1. 沾墨 2.非沾墨
	int staffId;
    int documentRemoteId;//锁定ID
//	int type ;
	int sealId ; //印章id
	int leftTimes ;
	int remote ;
    short int position[5]; //指纹坑位
    int stampInstall;
    char fileName[128];     //盖印流程名称
	char timet[24];
	int pictureType;
    char businessKey[64];
    int infrared;
}gt_userInfo;

//env
enum {
    ENV_TEST,
    ENV_SAAS,
    ENV_DEMO,
    ENV_PRI
};

sysInfo.sealPattern
特权盖印模式 1、常规模式 2、连续模式 默认常规模式
```


电机部分还需要独立demo测试一下