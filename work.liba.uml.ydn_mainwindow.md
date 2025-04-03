---
id: 8qskxrgq61dgcmyu70gjaq5
title: Ydn_mainwindow
desc: ''
updated: 1743659294245
created: 1743577114601
---

@startuml
skinparam sequence {
    ArrowColor DeepSkyBlue
    ActorBorderColor DeepSkyBlue
    LifeLineBorderColor blue
    LifeLineBackgroundColor #A9DCDF

    ParticipantBorderColor DeepSkyBlue
    ParticipantBackgroundColor DodgerBlue
    ParticipantFontName Impact
    ParticipantFontSize 17
    ParticipantFontColor #A9DCDF

    ActorBackgroundColor aqua
    ActorFontColor DeepSkyBlue
    ActorFontSize 17
    ActorFontName Aapex
}

title ydn_mainwindow_uml_sequence
footer Author: 徐佳飞 | Date: 2025-04-02

participant ui_event_loop_thread as ui_ev_thread
participant pollevent_thread as p_thread
participant upload_work_event_loop_thread as up_ev_thread
participant finger_thread as f_thread
participant infrared_thread as red_thread
participant motor_thread as m_thread
participant node_thread as n_thread
participant recordvideo_event_loop_thread as rv_ev_thread
participant takephoto_event_loop_thread as tp_ev_thread
participant capture_work_event_loop_thread as cpwk_ev_thread
participant convert_work_event_loop_thread as cnvwk_ev_thread
participant filetravel_work_event_loop_thread as ft_ev_thread
participant fourg_event_loop_thread as 4g_ev_thread
participant mqtt_operation_event_loop_thread as mqtt_ev_thread
participant wlan_event_loop_thread as wl_ev_thread

'主界面ui初始化
ui_ev_thread->ui_ev_thread: init_home_ui

'配置文件初始化和同步
[->ui_ev_thread: start initSystemAndSyncSystemConfigInfo
activate ui_ev_thread
[<-ui_ev_thread: end initSystemAndSyncSystemConfigInfo
deactivate ui_ev_thread

'构造工具对象:
'new http_opt()
'new MqttOperationWorker()
'new rk_pollevent()
'new MediaModule()
'new NetworkModule()
'new motorswitch()
'new rk_nodethread()
ui_ev_thread->ui_ev_thread: construct rely \n\
new object() \n\
new http_opt() \n\
new MqttOperationWorker() \n\
new rk_pollevent()

'启动pollevent线程
ui_ev_thread-->p_thread: start thread
activate p_thread
'执行run函数
'flow control flag reset, and timestamp var reset
'uart open, and port init, qst device init set
'new Infrared()
p_thread->p_thread: flow_control_flag reset
p_thread->p_thread: timestamp var reset
p_thread->p_thread: new Infrared()
'开始循环
loop break condition:触发超时并且未在操作中
'检查并更新蓝牙状态
'并行消息
!pragma teoz true
p_thread->p_thread: check_bluetooth_status
& p_thread-->ui_ev_thread: emit ui_update_opt
'开机30秒后上传日志, 触发信号
alt runtime=30s && mqtt_network ready && flag_uploaded==false
    p_thread->p_thread: flag_uploaded = true
    p_thread-->up_ev_thread: emit canUploadLogRdy 触发更新
'在upload_ev_thread中，执行upload操作
    activate up_ev_thread
    & up_ev_thread->up_ev_thread: uploadLogFile()
    deactivate
end
'start 日志上传滚动
alt is_sentlog (cur_log_file > 2100kb 触发 is_sentlog 标志为true \n即文件超出阈值一次触发上传一次)
    p_thread->p_thread: is_sentlog=false
    p_thread->p_thread: sort by timeline and remove old logs after 99 copies
    p_thread-->up_ev_thread: emit canUploadLogRdy 触发更新
'在upload_ev_thread中，执行upload操作
    activate up_ev_thread
    & up_ev_thread->up_ev_thread: uploadLogFile()
    deactivate
end
'end 日志上传滚动
'状态机流程: 

'循环结束
end
'finish
[<--p_thread: finished thread
deactivate p_thread



@enduml

















' rk poll 用印状态机图
' enum OPERATION
' {
'     UILOGHIDE = -1,              // 隐藏 UI 日志
'     IDLE = 0,                    // 空闲状态
'     ADDINKING = 1,               // 正在加墨
'     FINGERENTER = 2,             // 手指进入识别区域
'     FINGERREFUSE = 3,            // 手指识别拒绝
'     FINGEREWAIT = 4,             // 等待手指识别
'     STAMPSWTICH = 5,             // 印章切换中
'     STAMPSWTICH_FINISH = 6,      // 印章切换完成
'     STAMPING_REMOTE = 7,         // 远程盖章中
'     STAMP_RDY = 8,               // 盖章准备完成
'     STAMPING = 9,                // 盖章中
'     STAMP_START = 10,            // 盖章开始
'     STAMP_DISPOSE = 11,          // 盖章处理（结束后处理资源）
'     STAMP_COMPELETE = 12,        // 盖章完成
'     WAITEXAMINE = 13,            // 等待审核
'     PASSEXAMINE = 14,            // 审核通过
'     LOCKING = 15,                // 正在上锁
'     UNLOCKING = 16,              // 正在解锁
'     ADDINK_FINISH = 17,          // 加墨完成
'     MOVE_WARN = 18,              // 设备移动警告
'     STAMP_REMOTE_ALLOW,          // 远程盖章允许
'     STAMP_REMOTE_RDY,            // 远程盖章准备就绪
'     STAMP_WAITTING_COMMAND,      // 盖章等待指令
'     FINGERIMPORT,                // 指纹录入
'     WITH_INK,                    // 盖章时带墨
'     LOCKED,                      // 设备已上锁
'     UNLOCKED,                    // 设备已解锁
'     SETLOCK,                     // 设置锁定
'     REALSELOCK,                  // 解除锁定
'     STAMP_PRERUN,                // 盖章预执行（预备状态）
'     STAMP_WILL,                  // 即将盖章
'     STAMP_CONSECUTIVE,           // 连续盖章
'     STAMP_CONSECUTIVE_RESET,     // 连续盖章重置
'     STAMP_CONSECUTIVE_WITH_INK,  // 连续盖章且带墨
'     STAMP_CONSECUTIVERDY,        // 连续盖章准备完成
'     STAMP_CONSECUTIVE_PAUSE,     // 连续盖章暂停
'     STAMPSWTICH_CHECK,           // 盖章切换检查
'     VERIFY_FINGER,               // 指纹验证
'     UPGRADING,                   // 固件升级中
'     CONFIG_WIFI,                 // 配置 WiFi
' };

@startuml
'风格
skinparam backgroundColor #F9F9F9
skinparam state {
    BackgroundColor<<UPGRADING>> #FFDD57
    BackgroundColor<<IDLE>> #57D9FF
    FontColor black
    BorderColor black
    RoundCorner 15
    Shadowing true
}
' 

@enduml


@startuml
skinparam backgroundColor #F9F9F9
skinparam state {
    BackgroundColor<<UPGRADING>> #FFDD57
    BackgroundColor<<IDLE>> #57D9FF
    FontColor black
    BorderColor black
    RoundCorner 15
    Shadowing true
}
[*] --> IDLE : 空闲状态

IDLE --> ADDINKING : 开始加墨
IDLE --> FINGERENTER : 手指进入识别区域
IDLE --> STAMPSWTICH : 切换印章
IDLE --> STAMP_START : 开始盖章
IDLE --> LOCKING : 正在上锁
IDLE --> UPGRADING : 固件升级中

ADDINKING --> ADDINK_FINISH : 加墨完成
ADDINKING --> IDLE : 取消加墨

FINGERENTER --> FINGERREFUSE : 手指识别拒绝
FINGERENTER --> FINGEREWAIT : 等待手指识别

STAMPSWTICH --> STAMPSWTICH_FINISH : 切换完成
STAMPSWTICH_FINISH --> IDLE : 切换完成

STAMP_START --> STAMPING : 盖章中
STAMPING --> STAMP_COMPELETE : 盖章完成
STAMP_COMPELETE --> IDLE : 结束盖章

LOCKING --> UNLOCKING : 解锁设备
UNLOCKING --> IDLE : 解锁完成

WAITEXAMINE --> PASSEXAMINE : 审核通过
WAITEXAMINE --> IDLE : 审核不通过

STAMPING --> STAMP_REMOTE_ALLOW : 远程盖章允许
STAMPING --> STAMP_REMOTE_RDY : 远程盖章准备就绪

STAMP_CONSECUTIVE --> STAMP_CONSECUTIVE_RESET : 连续盖章重置
STAMP_CONSECUTIVE_RESET --> IDLE : 重置完成

STAMPSWTICH_CHECK --> IDLE : 切换检查完成

VERIFY_FINGER --> IDLE : 指纹验证完成
UPGRADING --> IDLE : 升级完成

@enduml
