---
id: t4wbmtadkchwaupzukpe629
title: Old_ydn_code_translate
desc: ''
updated: 1744796426539
created: 1744611961662
---
### note

1. not use 并没有使用
2. not trigger 不会被触发的分支

### concept

1. seal 印章
2. stamp 盖章
3. bt 蓝牙(bluetooth)
4. IDLE 状态机空闲状态
5. authorize 授权
6. fingerprint 指纹
7. upgrade 固件升级
8. 全量释放

### macro

```c++
//ydn_json.h: 60 用于发布消息时，组成目标主题
#define FIG_LOG_TOPIC "fingerprint_login" //FIG_LOG，
#define DEVICE_INFO_TOPIC "property" //DEVICE_INFO
#define CLOSE_LINK_TOPIC "CLOSE_CONNECTION"//CLOSE_LINK
#define FIG_VARID_TOPIC "fingerprint_identify_result"//FIG_VARID
#define ABNORM_WARN_TOPIC "equipment_move_event"//ABNORM_WARN
#define STAMPLE_INFO_TOPIC "document_state_uploading_event"//STAMPLE_INFO
#define FILE_UPLOAD_TOPIC "document_file_uploading_event"//FILE_UPLOAD
#define FIG_ADD_DEL_TOPIC "fingerprint_operation_state"//FIG_ADD_DEL
#define FIG_INPUT_RES_TOPIC "fingerprint_import_state"//FIG_INPUT_RES FINGER_IMPORT_RES
#define UPGRADE_STATUS_TOPIC "EQUIPMENT_MOVE_EVENT"//not use
#define INK_RES_TOPIC "with_ink"//INK_RES
#define DEVICE_RESET "reset"//DEVICE_RESET_RES

//ydn_json.h: 74 用于接收mqtt消息时判断主题，以及用于发布消息时，组合主题
#define TOPIC_USER_LOGIN "user_login"//用户登录
#define TOPIC_USER_LOGOUT "user_logout"//用户登出
#define TOPIC_STAMPCONTI_BEGIN "seal_stretch_out" //连续用印，印章伸出 以及 紧急开舱门
#define TOPIC_STAMPCONTI_PAUSE "seal_retraction"//连续用印，印章收回
#define TOPIC_SECERT_ISSUE "secret"//mqtt生成pass和clientid使用的secret
#define TOPIC_FINGER_CHANGE "fingerprint_auth"//印章更换
#define TOPIC_ADD_INK "begin_ink"//加墨
#define TOPIC_ADD_INKRES "begin_ink_result"//加墨
#define TOPIC_WITH_INK "with_ink"//蘸墨
#define TOPIC_FINGER_RESPONE "fingerprint_login" //指纹登录
#define TOPIC_FINGER_ENTERING "fingerprint_entering"//指纹录入
#define TOPIC_CHANGE_SEAL "change_seal"//换章，卸章
#define TOPIC_CHANGE_SEAL_CONFIRM "change_seal_finish"//换章，卸章结果
#define TOPIC_BEGIN_SEAL "begin_seal"//开始盖章
#define TOPIC_EQUIPMENT_LOCK "equipment_lock"//远程盖印时设备锁定
#define TOPIC_EXAMINE_RESULT "examine_result"//远程审批结果
#define TOPIC_FINGEROPT_OVER "fingerprint_type"//对应 FIG_USER_FINISH，第三次指纹录入结果
#define TOPIC_CANCEL_OPT "cancel_operation"//1. 取消盖印、换章，指纹录入、加墨等 通用指令 2. 云端向设备发送取消操作指令，设备退出当前动作，所做操作不进行保留
#define TOPIC_UPGRADE "upgrade" //云端下发ota固件升级
#define TOPIC_UPGRADE_ACTIVELY "/ota/firmware/request_reply" //开机主动发起升级后，云端下发的响应
#define TOPIC_FILEUPLOAD_BAK "document_state_uploading_event"//设备上传到云端盖印次数后，收到云端响应的主题
#define TOPIC_FINGER_IMPORT "fingerprint_import"//云端向设备下发指纹导入和删除命令对应的主题关键词
#define TOPIC_STOP_FINGERUSER "stop_privilege_seal" //协议内容: 停止特权用户用印，type为1时，判断用户id是否匹配，是则停止特权用印；type为2时，判断documentid是否匹配，有则停止普通用印 ;实际设备内容: 直接用户登出
#define TOPIC_VOICE_SWITCH "voice"//语言播报开关
#define TOPIC_REMOTE_LOCK "remoteLock"//远程对设备进行锁定和解锁
#define TOPIC_SEARCH_DEV "find"//not use
#define TOPIC_DEVICE_ERR "abnormal_warning_event"//设备上传印章预警
#define TOPIC_SEAL_PATTERN "seal_pattern"//云端下发变更特权盖印模式，特权盖印模式 1、常规模式 2、连续模式 默认常规模式
#define TOPIC_WIFI_CONFIG "network_configuration"//云端mqtt将密码账号发送到设备
#define TOPIC_WIFI_CONNECT_RESULT "network_state"//not use
#define TOPIC_WIFI_LIST "wifi_list"//云端向设备索取wifi列表,设备进行响应返回
#define TOPIC_WIFI_PRIORITY "wifi_priority"//云端发起网络优先级变更
#define TOPIC_VIDEO_UPLOAD_EVENT "video_record_upload_event"//(当移动端向云端发送取消盖印或者用户退出事件的时候，根据业务规则判断是否需要设备上传录制视频，并发送消息通知)，即云端通知设备是否进行视频上传

#define TOPIC_VOCIE_CALL "singing"//用于云端通知设备 开始播放以及播放结束
//#define TOPIC_WIFI_PRIORITY "wifi_priority"
```

### enum

```c++
//ydn_json.h: 13
typedef enum
{
    FIG_LOG,///sys/%s/%s/event/fingerprint_login/post 设备向云端发送指纹登录请求，
    DEVICE_INFO,///sys/%s/%s/event/property/post 设备向云端发送设备信息，
    CLOSE_LINK,//not use，/sys/%s/%s/event/CLOSE_CONNECTION/post
    FIG_VARID,//sys/%s/%s/event/fingerprint_identify_result/post 设备向云端发送指纹认证结果，
    ABNORM_WARN,///sys/%s/%s/event/equipment_move_event/post 设备向云端发送设备移动事件，
    STAMPLE_INFO,///sys/%s/%s/event/document_state_uploading_event/post, 设备向云端发送盖印次数，
    FILE_UPLOAD,//not trigger，/sys/%s/%s/event/document_file_uploading_event/post 设备向云端上传盖印图片的id以及文件id，
    FIG_ADD_DEL,///sys/%s/%s/event/fingerprint_operation_state/post, 设备发起指纹录入事件
    FIG_INPUT_RES,///sys/%s/%s/event/fingerprint_import_state/post，not use
    UPGRADE_STATUS,///sys/%s/%s/ota/firmware/progress，设备上传指纹ota升级进度
    UPGRADE_ACTIVELY,///sys/%s/%s/ota/firmware/request，设备主动申请ota升级
    INK_RES,///sys/%s/%s/service/with_ink/invoke_reply,设备返回沾墨结果
 REPLACE_RES,///sys/%s/%s/service/change_seal_finish/invoke_reply,设备返回卸章换章结果
 EMERG_OUT_RES,///sys/%s/%s/service/seal_stretch_out/invoke_reply,设备返回印章伸出结果
    FIG_USER_FINISH,///sys/%s/%s/event/fingerprint_type/post， 设备向云端发送第三次指纹录完
 FIG_VERIFY,///sys/%s/%s/event/fingerprint_verify/post,● 设备向云端发送指纹验证结果
    FINGER_IMPORT,///sys/%s/%s/event/fingerprint_import/post 设备响应云端的指纹导入或删除指令
    FINGER_IMPORT_RES,///sys/%s/%s/event/fingerprint_import_state/post● 设备导入完成，向APP发送指纹导入状态
    EXAMINE_RPLY,///sys/%s/%s/service/examine_result/invoke_reply 设备响应云端的审批结果消息
    DEVICE_RESET_RES,///sys/%s/%s/service/reset/invoke_reply 设备响应云端的重置消息
 FINGER_CHANGE_RES,///sys/%s/%s/service/fingerprint_auth/invoke_reply ???存疑，为什么这里是这个
    DEVICE_ERROR,///sys/%s/%s/event/abnormal_warning_event/post 设备发送印章异常预警
    WIFI_CONFIG_RES,///sys/%s/%s/service/network_configuration/invoke_reply        //WIFI配网结果反馈
    WIFI_CONNECT_RES,//not use       //WIFI配网状态结果反馈
    WIFI_LIST,///sys/%s/%s/service/wifi_list/invoke_reply              //WIFI扫描列表结果反馈
    SEAL_OPEN_POST,///sys/%s/%s/event/click/post         //仓门开启反馈
    SEAL_CLOSE_POST,///sys/%s/%s/event/action_end/post         //仓门关闭反馈
 UPGRADE_ACT,///sys/%s/%s/ota/firmware/request 设备发起ota升级事件
    VOICE_CALL,///sys/%s/%s/service/singing/invoke_reply 设备响应云端下发的播放请求
    USER_LOGOUT,///sys/%s/%s/event/user_logout/post 设备发出消息表示登出
    EXCESS_SEAL///sys/%s/%s/event/excess_seal/post
}PUB_TOPIC_ID;

//rk_pollevent.h:59 and motorswitch.cpp:9
enum RST_STEP //电机重置状态+运动状态
{
    RST_READY, //电机可重置状态
    MOTOR_IDLE, //电机空闲状态
    MOTORB_IN, //印章步进电机 收回
    MOTORB_OUT, //印章步进电机 伸出
    MOTORA_OPEN, //舱门打开
    MOTORA_OFF, //舱门关闭
    MOTOR_TEST, //not use
    MOTORB_IN_DELAY, //not trigger
    MOTORB_OUT_DELAY //not trigger
};

//rk_pollevent.h:72 
enum UI_DISPLAY
{
    UI_LOGIN = 1,//用户登录，提示欢迎使用
    UI_LOCKED,//设备锁定，强制登出锁定
    UI_TMP_LOCK,//设备锁定，屏幕锁定，会话暂停
    UI_OFF_COUNTDOWN,//计数关机
    CANCEL_TURNOFF,//取消关机
    UI_TMP_UNLOCK,//解锁会话暂停锁定，恢复用户登录状态
    UI_UNLOCKED,//解锁强制登出锁定，此时用户已登出
    UI_LOGOUT,//用户登录
    UI_TESTSTAMPCNT,//？？？
    UI_STAMPUPDATE,//显示盖印状态
    UI_REMOTEREFUSE,//盖印动作或结果状态:审核拒绝
    UI_REMOTEPASS,//盖印动作或结果状态:审核通过
    UI_STAMPING,//盖印进行中状态
    UI_STAMPRDY,//not use
    UI_STAMPRDY_REMOTE,//not use
    UI_STAMPRDY_LOCAL,//not use
    UI_BLCONNECT,//not use
    UI_WAITEXEC,//盖印动作或结果状态:等待剩余盖印次数
    UI_ADDINK,//盖印动作或结果状态:加墨
    UI_WITHINK,//盖印动作或结果状态:沾墨
    UI_REPLACE,//盖印动作或结果状态:换章
    UI_UPGRADEFAIL,//固件升级失败
    UI_MOVE_WARN,//盖印动作或结果状态:请勿移动提醒
    UI_FIGPASS,//指纹验证通过
    UI_FIGREFUSE,//指纹验证失败
    UI_DEVRESET,//设备重置
    UI_FINGERNOSTAMP,//盖印动作或结果状态:指纹验证通过，但没有印章提示
    UI_FLUSH,//not use
    UI_BLUI,//蓝牙显示控制
    UI_LOWPOWER,//低电量显示
    UI_FIGINPUT,//not use
    UI_LESSUPLOADPHOTO,//空间不足上传警告
    //SWITCH_NETWORK
};

//public.h: 300
typedef enum {
    WelcomeToIndia = 10001,//欢迎使用印得安
    SealUnlocked = 10002,//印章已解锁，请点击按钮开始用印
    UnauthorizedTOAPP = 10003,//用户未授权，请前往APP操作
    Unauthorized = 10004,//用户未授权

    StampOver = 10005,//次数用尽，本次用印结束
    VerifyFingerprint = 10006,//印章已连接，请验证指纹

    StartUse = 10007,//印章已解锁，请开始用印

    SealAudioSealUse = 10008,//用印完成
    WaitingRemote = 10009,//等待远程用印，请勿移动印章
    Approved = 10010, //审核通过，即将盖印
    RemoteRejected = 10011,//远程已拒绝，请重新发起

    SealConnected = 10012,//印章已连接

    SealAudioInputFingerprint = 10013,//开始录入指纹
    SealAudioPressAgain = 10014,//请再按一次

    SealAudioFingerprintSuccess = 10015,//指纹录入成功
    SealAudioFingerprintFail = 10016,//指纹录入失败

    SealAudioOpen = 10017,//仓门已打开
    SealAudioClose = 10018,//仓门已关闭

    FIirmUpdating = 10019, //固件升级中
    FirmUpadteFinsh = 10020, //固件升级成功
    FirmUpadteFail = 10021, //固件升级失败

    SealAudioLower = 10022, //电量低,请及时充电
    SealAudioKacha = 10023,//咔嚓声
    SealUserLogOut = 10024,//用户已退出
    DeviceLocked = 10025,//设备已锁定
    DeviceuNLocked = 10026,//设备已解锁
    DeviceuSearch = 10027,// 我再这里reportSel
    UpgradeandWel = 10028,// 升级成功+欢迎使用
    PlzMoveFinger = 10029,// 请重新按压

}SoundList;

//public.h: 221
//问题: 多模块多线程混用状态机标志，并且标志本身状态逻辑不合理，并且很多状态转化的操作不在一个代码块，经常是主状态机循环中判断进入分支以后，最终在其他线程模块中触发状态转化操作
enum OPERATION
{
    UILOGHIDE = -1,//not use
    IDLE = 0,//空闲
    ADDINKING = 1,//加墨中
    FINGERENTER = 2,//开始指纹验证
    FINGERREFUSE = 3,//not use
    FINGEREWAIT = 4,//not use
    STAMPSWTICH = 5,//开始换章
    STAMPSWTICH_FINISH = 6,//换章结束
    STAMPING_REMOTE = 7,//自动盖印中
    STAMP_RDY = 8,//盖印准备完毕（不包括指纹条件）
    STAMPING = 9,//盖印条件通过（如指纹等），即将进入盖印操作
    STAMP_START = 10,//开始盖印
    STAMP_DISPOSE = 11,//not trigger
    STAMP_COMPELETE = 12,//盖印完成
    WAITEXAMINE = 13,//非ocr认证,远程盖印，等待审批，会立刻转变为PASSEXAMINE
    PASSEXAMINE = 14,//同样是等待审批
    LOCKING = 15,//开始设备上锁
    UNLOCKING = 16,//开始设备解锁
    ADDINK_FINISH = 17,//加墨结束
    MOVE_WARN = 18,//not use
    STAMP_REMOTE_ALLOW,//由审核结果主题TOPIC_EXAMINE_RESULT消息触发，当用印类型为4ocr认证模式时的常规盖印触发
    STAMP_REMOTE_RDY,//由审核结果主题TOPIC_EXAMINE_RESULT消息触发，用印类型为4，ocr认证模式时的，一次盖印完成后的等待再次审批  （可以理解为是两次盖印之间的中间状态）
    STAMP_WAITTING_COMMAND,//开始用印主题TOPIC_BEGIN_SEAL消息触发，当用印类型为4，ocr认证模式时，第一次设置该状态，用于等待远程审核结果下发,是一个临时状态
    FINGERIMPORT,//通过蓝牙指纹导入
    WITH_INK,//开始沾墨
    LOCKED,//not use
    UNLOCKED,//not use
    SETLOCK,//not use
    REALSELOCK,//not use
    STAMP_PRERUN,//沾墨章沾墨开始
    STAMP_WILL,//盖印前沾墨完成
    STAMP_CONSECUTIVE,//连续盖印中
    STAMP_CONSECUTIVE_RESET,//连续盖印退出: 将电机印章归位,这里的reset应该指的伸出的电机印章
    STAMP_CONSECUTIVE_WITH_INK,//not use
    STAMP_CONSECUTIVERDY,//连续盖印准备完毕
    STAMP_CONSECUTIVE_PAUSE,//连续盖印暂停
    STAMPSWTICH_CHECK,// not trigger
    VERIFY_FINGER,//开始验证指纹
    UPGRADING,//开始升级固件
    CONFIG_WIFI,//not use
};



```

### struct

```c++
//ydn_json.c: 40 not use
typedef struct
{
    PUB_TOPIC_ID topic;
    char data[1500];
    bool strcopied;
}st_pubPackage;

//ydn_json.c: 111 (设备向云端发送指纹请求时)
typedef struct mt_FingerLog
{
    int position;
}mt_FingerLog;

//ydn_json.c: 117 ()
//TakePhotoContinual()拍照并且生成一个upload_node返回
//TakephotoAndReport()输入一个upload_node,生成盖印转台信息，本地保存一份，上传一份,并且触发addTakePhotoFileNodes信号，执行handleAddTakePhotoFileNodes()，将node添加到链表节点
//question: how the upload action triggered after takephone? by file work directly? 
//answer: 通过rk_pollevent中的node_queue作为关联上传线程和逻辑线程的通道，逻辑线程travel本地素材和data文件，生成node，添加到node_queue,而上传线程在后台轮询便利该列表执行上传
//question: how to handle the loss of node_queue on memory? such as shutdown 
//answer: 
typedef struct mt_UpLoadQueue
{
    int cnt;//新增node时自增，travelofflinefile时判断无效图片，则自减
    int id; //对应msgID的值，每次生成一个node时会自增，以及publishmqttmessage时自增
    bool isSync;//是否同步了盖印次数，当存在网络以及蓝牙时，赋值为true
    //add
    int type;//0-照片 1-视频 2-NULL
    int documentId;//盖印流程Id
    int sealId;//印章id
    int staffId;//用户id
    int leftTimes;//剩余盖印次数
    int documentFileId;//文档id
    int coverId;//补盖盖印Id

    char specialTag[64];//这里赋值时间戳
    char nvPath[128];//nv21图片路径，但通常和path是一个路径，只在使用时具体判断文件后缀
    char path[128] ;//素材路径，但通常和path是一个路径，只在使用时具体判断文件后缀
    char dataFile[128];//add /oem/offlineData/%1-%2@%3 图片对应的json文件
    int fileId ; //drop
    int failNums; //记录上传失败次数
    bool isInvalid;
    bool posted;
    bool changeName;
    int isOk;
    time_t timOut;// 不能以秒来计算 应为轮询的速度远远大于秒
    time_t postTime; //add
    char sealtime[28];
    struct mt_UpLoadQueue * next;
}mt_UpLoadQueue;

//ydn_json.cpp: 428 
//视频文件上传配置信息
typedef struct {
    int width;
    int height;
    int fps;
    int uploadMode;//录制h264视频时，先设置uploadmode为3，会先经历封装mp4，当判断到mp4文件和h264文件共存时，判定为转码失败，如果成功则将uploadmode升级为1进行mp4上传 upload 为2 not use
    //params
    video_upload_property video_property;//表示视频业务类型，是用印视频还是加墨视频
    int documentId;
    int sealId;
    int staffId;
    char businessKey[64];
}gt_video_record_info;

//ydn_json.cpp: 442
//视频是否上传信息
//对应TOPIC_VIDEO_UPLOAD_EVENT主题接收到的报文消息转化，通知设备是否上传录制视频，action为1表示需要上传，2表示不需要上传
typedef struct gt_video_upload_event_info
{
    int action;
    char businessKey[64];
}gt_video_upload_event_info;

//ydn_json.cpp: 451
//用来反序列化 /oem/offlineData/ 下盖印照片信息,目前似乎仅用来判断是否格式正确，错误则清理无效内容格式
typedef struct {
    int id;
    char version[32];
    int time;
    char sealTime[64];
    char specialTag[64];
    int sealId;
    int localMode;
    int position;
    mt_StationInfo station;
}media_photoInfo;

//ydn_json.cpp: 400
//用来反序列化 DEVICE_RESET 主题消息，
typedef struct {
    bool reset; //true: 需要重置 false:不需要重置
    bool reconnection;//true: 需要重新连接 false:不需要重新连接
    bool voice;//true: 开启语音 false:关闭语音
    bool remoteLock;//true: 锁定 false:解锁
    bool bell;//true:启动找设备 false :关闭找设备
    int sealId;//印章id
    int sealType;//印章类型（1.沾墨，2.非沾墨）
    int sealPattern;//特权盖印模式 1、常规模式 2、连续模式 默认常规模式
    int wifiPriority;//WIFI的优先级 1、优先WiFi连接  2、优先4G连接
    int cutState;//1、企业版 2、政务版
    int cutContext;//1、测试环境 2、演示环境 3、正式环境
    int turnoffTim;//设备关机时间，默认值5分钟   永不关机  -1
    int sleepTim;//设备休眠时间 ，默认值3分钟  永不休眠 -1
    int gyroParameter;//陀螺仪平衡参数  1-20  
    int remmoteWake;// 远程唤醒功能  1、关闭  2、开启   默认1 
    char secret[64];//秘钥
    char stampName[620];//印章名称
}gt_devReset_config;

//ydn_json.cpp: 391
//用来反序列化 TOPIC_WIFI_CONFIG 主题消息
//账号密码发送
typedef struct {
    int type;//1、配WiFi  2、删除WiFi  3、切换已保存的WiFi
    int save;//是否记住密码 1、记住密码  2、临时使用
    char remoteId[16]; //在印的安协议文档中未找到
    char devNumber[32];//在印的安协议文档中未找到
    char ssid[128];//WiFi名称
    char password[64];//WiFi密码
}gt_wifiConfig;


//ydn_json.cpp: 384
//remoteid: TOPIC_WIFI_LIST TOPIC_WIFI_CONFIG 等主题消息中附带的id
typedef struct
{
    int status;         //1：联网成功 2，账号密码错误，3、连接失败 4，连接成功但不能联网 5,忽略网络成功 6,忽略网络失败
    char remoteId[16]; //当前远程操作对应的id，由报文携带
    char message[32];   //附加值
}gt_wifiConfigReslut;

//ydn_json.cpp: 376
//RK_WifiScanResults返回结果转化而来的
//以下备注为 gpt 解释
typedef struct {
    int flags;//标记是否加密（0 表示开放网络 [ESS]，1 表示需要密码）
    int currlink;//是否是当前连接的热点（0：否，1：当前连接，2：离线时回连的）
    int save;//是否是保存过的热点（即曾经连接过，配置已保存）
    int rssi;//信号强度（单位 dBm，数值越接近 0 越强，-30 优秀，-70 勉强）
    char ssid[40];//热点名称
}gt_wifilist;

//wlanworker.h: 26
//ap? access info
//以下备注为 gpt 解释
//ap为扫描出来的环境信息列表，wpa为当前的wifi配置
typedef struct {
    int flags;//标记是否加密（0 表示开放网络 [ESS]，1 表示需要密码）
    int currlink;//是否是当前连接的热点（0：否，1：当前连接，2：离线时回连的）
    int save;//是否是保存过的热点（即曾经连接过，配置已保存）
    int rssi;//信号强度（单位 dBm，数值越接近 0 越强，-30 优秀，-70 勉强）
    QString ssid;//热点名称
} ApInfo;
typedef struct {
  char network_id[8];//热点的内部 ID，wpa_supplicant 用于区分多个配置
  char ssid[128];//热点名称
  char bssid[128];//热点的物理地址（MAC 地址）
  char state[64];//热点连接状态，如 [CURRENT], [DISABLED], [ENABLED] 等
} WpaInfo;

//ydn_json.cpp: 368 not use
typedef struct gt_changesealrespon
{
    int seal;
    int motion;
    int status;
    char message[56];
}gt_changesealrespon;

//ydn_json.cpp: 361 
//对应TOPIC_FINGER_ENTERING主题消息的反序列化，
typedef struct gt_enterInfo
{
    int type;//1、指纹录入 2、指纹删除 3、指纹录入需指纹校验
    int position;//指纹标志位
    int auth;//是否为特权用户，1是，2否
}gt_enterInfo;

//ydn_json.cpp: 350
//对应TOPIC_FINGER_IMPORT主题消息的反序列化，对应指纹导入和删除事件
typedef struct gt_fingerImport
{
int cnt ;//???协议文档中无描述
int totalCnt;//???协议文档中无描述
int recvCnt;//???协议文档中无描述
int cloudId[21];//???协议文档中无描述
int positon[21];//导入成功时，指纹存储位置1~200
int type[21];//1、指纹导入 2、指纹删除 
int auth[21];//对应指纹的特权值
}gt_fingerImport;

//ydn_json.cpp: 343
//对应TOPIC_USER_LOGIN主题消息的反序列化，对应用户登录
typedef struct gt_userlogin
{
int model;//1：蓝牙 2：MQTT
int staffId;//用户id
char staffName[128];//用户名称
}gt_userlogin;

//ydn_json.cpp: 335 not use
typedef struct gt_uploadInfo
{
    unsigned char uploadcnt;
    int  documentFileId;
    int  HttpfileId[2];
}gt_uploadInfo;

//ydn_json.cpp: 300
//对应TOPIC_BEGIN_SEAL主题消息反序列化，以及TOPIC_FINGER_RESPONE主题消息反序列化
typedef struct gt_userInfo//指纹登陆或者APP登陆共用结构体
{
int identity;   //1-特权用户 2-普通登录用户 4-蓝牙登录用户
int status ;//1: 可以用印 2: 不可用印
int documentId ;//文件id
int coverId;//补盖id
char stampName[620];//印章名称
char staffName[512];//用户名称
int sealType;//印章类型 1. 沾墨 2.非沾墨
int staffId;//用户id 
int documentRemoteId;//锁定ID
// int type ;
int sealId ;//印章id
int leftTimes ;//剩余盖印次数
int remote ;//用印类型  1、常规用印（无需OCR认证） 2、远程用印 3、连续用印 4、认证模式（设备处于等待状态） 4这个认证模式是OCR认证? 
short int position[5];//指纹位置
int stampInstall;//印章是否绑定
char fileName[128];     //盖印流程名称
char timet[24];//盖印时间
int pictureType;//图片，0、开启设备影像 1、关闭设备影像 默认为0
char businessKey[64];//如果有值，则表示此次盖印过程需要录制视频，且businessKey的值即为此次视频的唯一标识符
int infrared;//是否开启红外设备
}gt_userInfo;

//ydn_json.cpp: 292
//用于指纹导入结果的消息序列化
typedef struct gt_importRes//印章信息
{
int type;//1、指纹录入 2、指纹删除 3、重置指纹并导入新的指纹
int position;//指纹标识位
int status ;//1、核验成功  2、核验失败
char msg[24];//失败时错误信息
}gt_importRes;

//ydn_json.cpp: 261
typedef struct gt_sysInfo//印章信息
{
int stampInstall;//是否绑定印章，1:已绑定 0:未绑定
char stampName[620];//印章名称
char stampNamenoEnter[620];//not use
char userName[512];//用户名称
char mqttUser[108];//not use
char mqttpassWD[108];//not use
char mqttKey[72];//提取的报文中的secret作为mqtt key，用于生成mqtt密码和用户名
int staffId;//用户id
int wifiPriority;//1、优先WIFI连接  2、优先4G连接
int sealPattern;//特权盖印模式 1、常规模式 2、连续模式 默认常规模式
char version[12];//固件版本信息
int sealId ;//印章id
int upgradeFlg;//是否升级，true or false
int upgradeType;//升级方式0-http 1-https 2-bt
int energy;//电池电量
int voice;//是否播放声音
int  sealType;//印章类型（1.沾墨，2.非沾墨)
int phtotgraphicId;//???
unsigned char islocked;//远程锁定
unsigned char envir;//环境://1、测试环境 2、演示环境 3、正式环境
unsigned char pubEnv;//1、企业版 2、政务版
char priIp[256];//私有化环境使用ip
char priApi[256];//私有化环境使用api
int sealDistance;//印章下落距离调节
int axisOffset;//三轴灵敏度
bool remotewake;//是否远程唤醒
unsigned int SuperUserSpec[200];//数组下标对应指纹位置，具体下表对应元素的值标识该指纹是否是特权
}gt_sysInfo;

```

```c++
remote 
// 用印类型 
//1、常规用印（无需OCR认证） 2、远程用印 3、连续用印 4、认证模式（设备处于等待状态）
//4这个认证模式是OCR认证? 

pictureType
//图片，0、开启设备影像 1、关闭设备影像 默认为0

businessKey
//如果有值，则表示此次盖印过程需要录制视频，且businessKey的值即为此次视频的唯一标识符
stafffid
//用印人id，让设备在盖印前获知
infrared
//是否开启红外设备
lefttimes
//剩余次数
sealid 
//印章id
authentication
//是否开启指纹认证

mode 
// 同意之后盖印模式
// 1、常规盖印一审一印模式
// 2、常规盖印一审多印模式
// 3、连续盖印一审多印模式
// 4?全量释放

equipment
//远程盖印进行设备锁定，锁定时不能随意移动，会报警和退出用印

state
//远程用印审批结果: 1. 同意 2. 拒绝
//设备是一印一审模式下，同意后用印后可以移动；拒绝后，设备解锁，不能用印，app端让用户重新发起审批认证

盖印次数上传: 设备核对剩余盖印次数进行盖印，上传盖印的次数，云端返回更新后剩余的盖印次数
//普通用印次数同步（常规用印、连续用印、远程用印、指纹普通盖印）
//特权用印次数上传

coverid
//补盖? not use, only record info?

motion
//1、更换印章2、卸下印章3、印章类型修改
//流程：云端->设备:执行motion，云端->设备：motion finish

device_maintain
//设备唤醒消息
//但在设备端并没有实现

开始用印的指纹认证和非指纹认证的区别?

```

### usage case
