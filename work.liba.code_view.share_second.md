---
id: eg5dldtgeilkyiyt1kr1xgo
title: Share_second
desc: ''
updated: 1744596712418
created: 1744342310259
---
### 蓝牙数据处理

1. 在wlwork模块中定义的
   1. 发: sendBtData 依赖 RK_BleWriteData
      1. 在wlanwork中处理wifi配置时，使用sendBtData进行报文响应
      2. 同时信号转发到上层的rk_pollevent::sendBtData，但是并没有使用该函数进行蓝牙保温数据发送???
   2. 收: handleBTCommand
      1. 处理wifi配置，`WIFILIST`，`WIFISETTING`
      2. 发送信号 `recvBtData`，在rk_pollevent中定义数据处理的回调

2. 在rk_pollevent中封装逻辑
    1. 发
        1. 底层实现逻辑: 根据硬件版本的不同，`is_v2_6pcb` 使用不同的蓝牙收发方式
            1. `is_v2_6pcb`: RK_BleWriteData(channel==1?BLE_UUID_WIFI_CHAR:BLE_UUID_SEND, ...)，这里传参的uuid根据蓝牙协议版本而定
            2. `!is_v2_6pcb`: WriteComPort(ttyBl,...) 串口蓝牙
        2. 中游实现分为了
            1. RK_BTSEND bt_send_p 拥有队列
            2. rk_pollevent::sendn() 直接发送
                1. rk_pollevent::Ble_ENVIRONMENT_Dispose_V1 环境切换协议处理时，末尾直接发送 `sendn((char *)"/>|22|ENVIRONMENT|OK|#")`
                2. rk_pollevent::UpdateLimitSwitch(unsigned char sta) 在isBlueTooth的情况下，会回复"INKING"，通过`sendn()`
                3. rk_pollevent::blebuffer_reply(char* buffer) -> rk_pollevent::sendn();**该函数用于指纹数据上传，在独立线程执行，被mainwindo层引用**
            3. rk_pollevent::Ble_Reply() -> rk_pollevent::sendn() || bt_send_p; 封装了不同协议版本的指令回复，内部实现结束指令使用`sendn`回复，其他的使用`bt_send_p.Control_Of_Btsend`按序排队发送
        3. 上层发送数据的场景都是 调用Ble_Reply进行发送
    2. 收
       1. 上层，统一的msg处理: rk_pollevent::anlasyBTmsg(char *msg)；readmsg(buff)负责处理测试指令
          1. 解析指令，调用对应的处理函数
       2. 底层，`!is_v2_6pcb` 触发 rk_pollevent的循环中，rk_pollevent::BTreadOneMsg() -> anlasyBTmsg(): 读取串口tty，最终进行 anlasyBTmsg 业务流程指令处理或者 readmsg(buff) 进行测试指令处理
       3. 底层，`is_v2_6pcb` 触发 wlwork的事件循环中，rk_pollevent::BtHandleMsg()，该数据流，通过wlwork的模块，两层信号转发，最终将模块的蓝牙数据发送到这里
          1. wlworker: emit recvBtData(); 事件循环
          2. netmodule: connect(m_wlanWorker, &WlanWorker::recvBtData, this, &NetworkModule::recvBtData); 无事件循环
          3. mainwindow: connect(m_networkModule, &NetworkModule::recvBtData, p_poll, &rk_pollevent::BtHandleMsg); ui事件循环
          4. 信号转发方向: WlanWorker::handleBTCommand-> emit WlanWorker::recvBtData(cmd, len) -> NetworkModule::recvBtData -> rk_pollevent::BtHandleMsg()
          5. BtHandleMsg 最终执行线程: wlworker的事件循环


### mqtt 数据流

1. 重连
2. 接收消息处理
   1. 底层，mqttwork: 模块初始化时，开启事件循环，500ms定时执行状态机，当网络状态`LINED`时，触发`MqttOperationWorker::updateRecvNodeFront()`，消息处理状态条件+发送数据`emit handleOneMqttMsg(analsy_queue)`
   2. 中游，信号转发，mainwindow: connect(m_mqtt, &MqttOperationWorker::handleOneMqttMsg, p_poll, &rk_pollevent::DisposeSubcribeMsg2);  
   3. 上层，msg消息处理，rk_pollevent::DisposeSubcribeMsg2 -> `rk_pollevent::AnalsyMqttServerMsg(char *getdata, char *gettopic)`，解析主题和报文
   4. 信号转发方向: MqttOperationWorker::updateRecvNodeFront()->emit MqttOperationWorker::handleOneMqttMsg(analsy_queue)->rk_pollevent::AnalsyMqttServerMsg()
   5. rk_pollevent::AnalsyMqttServerMsg 最终执行线程: mqtt work的事件循环
3. 发布消息
   1. 底层，mqttwork，存在两个版本
      1. MqttOperationWorker::publishMessage() 没有重试，会等待消息发送的结果，成功或者超时
      2. MqttOperationWorker::publishTopicIdMessage_1() 直接发送，没有等待消息发送的结果的步骤
   2. 中游，信号转发，mainwindow
        connect(p_poll, &rk_pollevent::publish_Message_signal, m_mqtt, &MqttOperationWorker::publishTopicIdMessage); MqttOperationWorker拥有事件循环线程
        connect(p_poll, &rk_pollevent::publish_Message_signal_1, m_mqtt, &MqttOperationWorker::publishTopicIdMessage_1);
   3. 上层，rk_pollevent 核心包装为 `MqttPublish`
      1. rk_pollevent::MqttPublish(QString data, int topicNumber) -> MqttOperationWorker::publishTopicIdMessage 会等待消息发送的结果
      2. 直接调用信号 emit publish_Message_signal_1() 直接发送，仅在发送用户登录消息以及空白报文时，调用，其他均使用MqttPublish
         1. emit publish_Message_signal_1(QString("/sys/%1/%2/service/%3/invoke_reply").arg(topicType).arg(DEVICE_NUMBER).arg(TOPIC_USER_LOGIN), datas);
         2. char dataa[480] = {0}; JsonNormRespon(dataa, 0, this_msgid/*msgid*/); emit publish_Message_signal_1(ttopic, dataa);
   4. 上上游，mainwindow 会调用 rk_pollevnet::MqttPublish()
   5. 信号转发方向:  rk_pollevent::MqttPublish -> rk_pollevent::publish_Message_signal -> MqttOperationWorker::publishTopicIdMessage
   6. 最终执行线程: mqtt work的事件循环



### 指纹

1. figThread 封装了进行指纹注册，识别的状态机线程
   1. 开启条件: mainwindow connect大部分信号和槽之后，开启指纹线程条件: p_poll->sysInfo.stampInstall == 1 || strlen(p_poll->sysInfo.stampName) > 0 即绑定了印章
   2. 信号方向

   ```c++
    在指纹状态机线程中: emit figThread::displaySig --> MainWindow::DisplayFigerOperation
    在指纹状态机线程中: figOption == FIG_QUIT -> figThread::stop() -> emit figThread::finishSig --> MainWindow::TerminaFigThread->MainWindow::RecycleFingerThread()   内部状态触发mainwindow释放指纹线程
    在mainwindow中: MainWindow::RecycleFingerThread()-> P_figroll->figOption = FIG_QUIT;delete ... 释放指纹线程以及资源
   ```

   3. 交互逻辑: 
      1. 默认 mainwindow初始化 开启指纹线程，状态机线程在后台进行轮询，主要响应 `FIG_REG` 和 `FIG_MATCH`的操作，而指纹线程创建时，默认进行 `FIG_MATCH`处理
      2. 在状态机中处理的结果通过 emit figThread::displaySig() 进行交互响应

        ```c++
        //mainwindow  MainWindow::DisplayFigerOperation
        switch (type)
        { 
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
            DisposeFingerResult(type); //匹配成功后，type值为指纹pos传到deault中进行处理
            break;
        }
        ```

    4. 指纹匹配的逻辑
       1. 在 FIG_MATCH 循环中:
            1. 联网状态下:
                1. 匹配到指纹: FignerSeachFp(&getId) >= 1 -> EmitDisplaySignal(getId) -> emit displaySig(type) 指纹匹配成功最终会将匹配到的id发送到 MainWindow::DisplayFigerOperation 槽函数
                2. 发布mqtt指纹信息，更新pollevent状态: MainWindow::DisposeFingerResult(pos)-> checkWebServerStatus(int) -> emit MainWindow::alreadyCheckWebServerStatus(int) --> MainWindow::onAlreadyCheckWebServerStatus(int) -> 条件:!p_poll->isLoging && p_poll->isOperation == IDLE -> ReportUserputFinger(int)->publishTopicIdMessage(FIG_LOG, ..)，p_poll->fignerWait = true等待服务器响应返回用户信息
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


### 电机

1. motorswitch 封装了电机状态响应操作
   1. 一个状态机后台线程，不断获取限位开关状态，判断当前开关状态组合变化，以及电机运动状态(外部输入)，来触发对应的电机动作，转化电机运动状态
   2. 上层调用: motorswitch模块创建并运行-->其他模块: 执行电机动作宏，向 motorswitch 传入电机运动状态 --> motorswitch 状态机线程根据 开关组合状态 + 电机运动状态 + 业务标志位，触发对应操作
   3. 信号方向
      1. rk_pollevent触发电机复位(远程锁定，用户登出，TOPIC_STAMPCONTI_PAUSE，用印退出，TOPIC_CANCEL_OPT，TOPIC_ADD_INKRES加墨，设备重置，焕章): emit: rk_pollevent::MotorReset() -> motorswitch::MotorPositReset() -> 设置标志位，等待motorswitch状态机线程处理
      2. rk_pollevent执行电机操作宏后，设置电机运动状态: emit: rk_pollevent::stampSport(unsigned char) -> motorswitch::GetStampSportDirect(unsigned char)
      3. rk_pollevent调整印章伸出长度: emit: rk_pollevent::stampDistanceSet(int) -> motorswitch::SetSealDisdtance(int)
      4. rk_pollevent触发电机状态机线程重新检测标志(开关状态组合发生变化 || 重检标志==true): emit: rk_pollevent::Refresh_Motor_Status(void) -> motorswitch::Refresh_Motor_Status(void)
      5. motorswitch 状态机处理一次状态变化后将当前状态组合值抛出: emit motorswitch::limitswi(unsigned char)-->mainwindow::tmpHandleMotorUpdate(unsigned char)->
        p_poll->UpdateLimitSwitch(sta) 触发盖印相关的逻辑判断，触发对应电机动作 + MainWindow::updateMotor(sta)核减盖印次数;




### 拍照录像时机

1. 拍照 2
   1. `TakePhotoContinual`拍照封装于 rk_pollevent中，进行拍照，并且返回一个node用于上传
   2. 盖印过程中，设置needphoto标志位 在rk_pollevent中
      1. TOPIC_EXAMINE_RESULT 审批结果处理
      2. userInfo.remote == 4 处于远程盖印
   3. 在mianwindows中 MainWindow::tmpHandleMotorUpdate()  核减盖印次数 
2. 开始录像 five
   1. `Q_EMIT pMediaModule->setRecordVidesEnabled(true, ...)`
   2. 蓝牙v1，加墨时录制  （<btl_msg>need_video && <系统镜像>2.4v ）使用gt_video_record_info.info.businessKey = time_str 作为生成视频的key
   3. 蓝牙v2，`STARTSTAMP` && isLoging -> userInfo.businessKey != "0"  ,使用userInfo.businessKey 作为生成视频名称的key
   4. mqtt_msg_anaysis -> JsonParseBeginStamp(., gt_userInfo) or JsonParseFinger(., gt_userInfo) -> update: userInfo.businessKey
   5. mqtt, 未登录 主题正确 指纹已经验证并等待服务器下发指纹等级(TOPIC_FINGER_RESPONE) -> has_videoRecord && userInfo.businessKey > 1，使用userInfo.businessKey作为生成视频名称的key
   6. mqtt, 开始盖章 必须在登陆的状态下 但不能是指纹登陆(TOPIC_BEGIN_SEAL) -> has_videoRecord && userInfo.businessKey > 1 ,使用userInfo.businessKey 作为生成视频名称的key
   7. mqtt，加墨(TOPIC_ADD_INK)，默认不录制，-> <mqtt_msg>need_video && is_can_record, 使用userInfo.businessKey 作为生成视频名称的

### 拍照 录像数据流

### 电池电量处理

1. 在rk_pollevent中定时触发 rk_pollevent::getDeviceStatus() -> 获取voltage_now，current_now
   1. 检测充电状态
   2. 过程输出每次cat到的电压电流数据
   3. 根据充放电状态对数据进行补偿计算
   4. 通过VolTransCapacity进行电量和电量转换
   5. 进行数据采样，十次记录，进行取平均值
   6. 检查电量值，低于阈值则电池电量警报
   7. 结尾触发信号 mainwindow::updateSta(st_devStatus *) 响应ui状态变化


### http 
1. media/uploadwork.cpp
      1. UploadWorker::uploadLogFile(QByteArray devSn) 上传日志，使用QHttpMultiPart
      2. UploadWorker::uploadPhotoFiles UploadWorker::uploadVideoFiles， 如果是https，使用curl，如果是http，使用http
      3. UploadWorker::uploadFingerDatas 同样
   3. global/QRcode/http_opt.cpp 被mainwindow引用
      1. http_opt::PostLocalLog deprecated
      2. mainwindow.cpp -> o_http->isCheckHttpsCurlExecOK() 检查curl功能
      3. mainwindow.cpp -> http_opt::httpsCurlPostFingerData 发送指纹数据，条件: 私有化环境`ENV_PRI`并且api contains`https`，否则将使用 m_mediaModule->uploadFingerDatas() http上传
      4. mainwindow.cpp -> o_http->httpsCurlGetFingerData() 下载指纹数据，缓存到临时文件`/tmp//tmp/fingerprint_%d.txt`，条件:私有化环境`ENV_PRI`并且api contains`https`，否则将使用`QNetworkRequest`，MainWindow::ConstructGet-->receiveReply(QNetworkReply *) ->MainWindow::handleHttpNetworkReply 

### 环境切换

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

   那么问题来了，私有化的ip_port来源
   1. 默认来源 由 /oem/sys.info 下的ip_port进行初始化
   2. 在void rk_pollevent::Ble_ENVCHANGE_Dispose_V1(char* msg, char* replymsg)-> 提取消息priApiser和priIpandPort，publishEnv 同步到sysinfo中
   3. Ble_ENVIRONMENT_Dispose_V1 和 Ble_ENVCHANGE_Dispose_V1 什么区别?

### 版本切换逻辑

1. mqtt 密钥逻辑? mqtt connect时需要密码和clientid，根据不同的mqtt环境(company和government)会生成不同的psw和clientid，即当触发了环境（政务和企业）切换时，需要生成对应的secret，用来重新连接mqtt
   1. 为什么 mqtt`"secret"` 的主题定义为 TOPIC_SECERT_ISSUE???
   2. 关键的`secret`来源，通过`TOPIC_SECERT_ISSUE` 主题消息 同步到`sysInfo.mqttKey`
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

2. mqtt publishenv切换的触发?
   1. 默认 publishEnv 为1
   2. AnalsyMqttServerMsg 接收到mqtt_msg: topic:DEVICE_RESET("reset") 此主题为设备初始化时会进行
   3. 提取`cutContext`+`secret`字段决定版本，`cutState`字段决定环境
   4. 判断到与publishEnv不同则进行切换
3. 切换触发后的 mqtt的网络重连
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

### 网络切换

### 所有盖印流程

### 蓝牙下载指纹和上传指纹数据

### 为什么蓝牙固件不能传?

### 指纹录制数量 99,100 问题

### nopower对用印状态的影响?

### superuserpos 的存取和配置文件中特权用户的记录

### 蓝牙的服务通道uuid

### 指纹的核心板以及触摸板分离供电？低功耗处理?

### 不同用印方式对应的核减方式的区别：连续盖印使用o型板，普通盖印使用按键，为什么？

防止常规盖印时的漏盖和偷盖


