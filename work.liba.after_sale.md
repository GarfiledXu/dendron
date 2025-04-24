---
id: o6ufr9p3s8r110rub2bvsnv
title: After_sale
desc: ''
updated: 1745465707645
created: 1743669345264
---
### message bak

1. 已经拉你进售后群了，可以找几个问题然后带着代码一起看一看。测试平台能找到设备日志，登录密码和账号上面发给你了，测试运营平台是能看到各个环境的设备日志的。私有化部署是设备环境的配置和运营后台地址。尽量不要去调整各平台的设备绑定和固件包就好。
2. 私有化部署链接: https://alidocs.dingtalk.com/i/nodes/4lgGw3P8vPkvbXoBSPlDn5a5V5daZ90D?cid=1053694951:1219131438&corpId=dingb052090d850ff2a535c2f4657eb6378f&doc_type=wiki_doc&iframeQuery=utm_medium=im_card&utm_source=im&utm_medium=main_vertical&utm_scene=team_space&utm_source=search

### 20250403 建德-固件升级后网络问题 2.5.0 版本 

1. 推送ppp拨号配置文件

```bash
adb push ./quecteludev-ppp /etc/ppp/peers/
### 
chmod 755 /etc/ppp/peers/quecteludev-ppp
```

2. 推送curl到设备/bin

由于服务器访问改为了https，所以需要用到curl

```bash
adb push ./curl /bin
###
chmod 755 /bin/curl
```

3. 验证

```bash
killall daemon_service ydn-rk

./daemon_service 

tail -f log_*

```

### 20250417 建德-之前没有进行升级的设备现在都无法进行网络连接

旧网址失效了

### 20250417 电池电量

#### 福建省谋成水泥（SaaS）：4.10号充满电后，今天开机发现没有电了，中途一直是关机的，没有使用过，麻烦排查一下耗电情况

docid:2025041701

![alt text](0a94d66d952265b6a47beb2db46cda89.png)

1. VER: 2.6.2
2. YDA231001000263
3. seal_name: 福建省大田县新岩水泥公章
4. 反馈人: 王雪
5. 后续: 0418反馈: 昨天充满电盖印了几次今天又没有电了，你再排查看看怎么处理

#### 奥克斯集团常州中吴明州康复医院   YDA250301000169  客户8点上班 开始充电 然后设备一拔掉就低电量 和昨天设备特权死机是一台设备@周青媛

docid:2025041702

1. 反馈人: 于红娟
2. YDA250301000169


**排查过程**

```bash
rk_pollevent.cpp: 264: void rk_pollevent::getDeviceStatus()->
qDebug("correct %f %f c=%d \n ", getDevCap, getDecur, device_sta->isCharge);
qDebug("samp 100 times get capcity [%d]  ", device_sta->capcity);

eg:
2025/04/09 15:09:46 [299] correct 3927360.000000 -953000.000000 c=0 
2025/04/09 15:09:55 [299] correct 3915760.000000 -1148000.000000 c=0 

2025/04/09 15:10:16 [339] samp 100 times get capcity [80]  

log文件名称格式:
log_202504091509.txt
log_201708061123.txt
log_take_202504101418.txt

脚本生成:
现在你是一个很牛逼的脚本程序员，请帮我生成一个python脚本，用于日志信息过滤和重整，以及绘图
我先说明一下，日志的格式
1. 首先日志文件名称格式是这样的
log_202504091509.txt
log_201708061123.txt
log_take_202504101418.txt
其中log_202504091509.txt是正常日志，log_201708061123.txt是设备出异常，导致名称上的时间戳异常，log_take_202504101418.txt带log_take_前缀的是拍照时日志
2. 其次，每局日志的时间戳格式 2025/04/15 17:26:25 

我需要的脚本，进行流水线式处理，即每一个步骤对上一个步骤的文件处理，生成新的子文件夹，作为下一个步骤的输入，每一个文件夹名称格式 : <log_src_dir_name>_<seiral>

首先输入是一个文件夹目录，该目录下会放置n个上诉格式的日志文件，你需要对其进行处理，先在当前路径创建一个root目录，用于所有输出以及中间文件存储，名称格式 <log_src_dir_name>_output
第一步，预处理修复文件异常，这是一个可选项，用标志位控制，如果为true，则会对日志文件名称，进行校对，逻辑为，判断文件的第一句日志，提取时间戳，来重命名文件名称，所有输出到<log_src_dir_name>_1中，如果选项是false，则只是把原日志内容拷贝到这里

第二步，根据配置的lambda表达式，按序提取整句日志，lambda表达式可能有多个，对应不同种类的日志，要求给出一个开关，表示是否每种日志分文件输出，还是一个文件中匹配上的都按序输出到对应的一个日志文件中，如果是分文件输出，则一个原日志文件，建立对应的子文件夹，然后人类整理输出的日志都存储在里面，仅加一点后缀需要_<seiral>

这里给出两句日志示例:
2025/04/09 15:09:46 [299] correct 3927360.000000 -953000.000000 c=0 
2025/04/09 15:10:16 [339] samp 100 times get capcity [80] 
要求提取这种日志，输出到文件夹<log_src_dir_name>_2中，
请先完成这两部分

xxxxxxxxxxxxxxxxxxxxxxxxx
很好，我发现能跑起来了，你真厉害，现在我需要修改一个小需求，即输出根文件夹名称冲突处理，比如第一次输入2025041701，输出2025041701_output,然后我重新运行以后，程序需要判断是否2025041701_output存在，存在则新增一个后缀2025041701_output_<serial>, 如2025041701_output_2来递增

xxxxxxxxxxxxxxxxxxxxxxxx
我需要你进行改造，按刚才说的toml配置方式，但是配置文件的序列化反序列化部分需要优化一下，请用一个完整的class数据结构，来表示配置内容，然后代码模块使用，而不是零散的变量
xxxxxxxxxxxxxxxxxxxxxxxx
昨天提到的日志提取工具，今天要优化一下第一个功能: 日志文件名修复
目前会遇到的问题是，由于设备亏点，导致rtc重置，所以开机后一开始时间是2017年的，只有联网了一段时间后，时间戳才会正常到2024年
而日志的文件名是根据最开始的日志时间戳命名的，导致了日志名称异常

所以现在这个功能: 
开出配置项:
1.  是否修复日志名称
2. 异常时间段，要求能够，比如小于2024年的全是异常的，或者大于2026年这种的，反正可以配置多个时间段，并且区间的另一边可以无穷小或者大
处理:
进入日志文件后，判断日志时间戳是否在异常区间，在异常区间内则忽略，直到有正常时间戳，使用这个来命名日志文件，但是修复的需要在文件名加上后缀_fix
```



```bash
C:\Users\Administrator\Desktop\tmp\Minimal ADB and Fastboot>adb push C:\Users\Administrator\Desktop\tmp\0417\UPGRADE_PACKAGE_2503271600\rkmedia_catch    \bin
5405 KB/s (44268 bytes in 0.007s)

C:\Users\Administrator\Desktop\tmp\Minimal ADB and Fastboot>adb push C:\Users\Administrator\Desktop\tmp\0417\UPGRADE_PACKAGE_2503271600\rkmedia_recordvideo \bin
6472 KB/s (79320 bytes in 0.011s)

adb push C:\Users\Administrator\Desktop\tmp\0417\UPGRADE_PACKAGE_2503271600\rkmedia_recordvideo \bin

C:\Users\Administrator\Desktop\tmp\Minimal ADB and Fastboot>adb push C:\Users\Administrator\Desktop\tmp\0417\UPGRADE_PACKAGE_2503271600\rkmedia_takephoto \bin
5878 KB/s (84040 bytes in 0.013s)


```

```bash
killall daemon_service ydn-rk

cd /oem/Backup && sh ./install.sh



```

### 20250418 售后-升级固件

#### 账号

1. 向日葵远程: 1842955336 今日验证码: x4r24f
2. 设备vin: YDAA2311030000102
3. 固件version: 2.4.7
4. 界面状况: 提示"无法联网，请使用蓝牙连接设备.."
5. 待升级数量: 2台
6. 地点: 建德市梅城镇姜山村

结果: 还剩一台设备连接后adb无法识别，待处理

#### 账号

1. 建德市乾潭镇罗村村
2. 958963285 572d5u

```bash
## 传输文件

## kill 进程

## 执行sh脚本

## 修改 cfg

## 执行damon

## 查看日志
adb push ..\..\UPGRADE_PACKAGE_2503271600 /oem/Backup
adb pull /oem/sys.cfg .
adb push ./sys.cfg /oem
```


### 20250421 售后-升级固件

1. 建德市大洋镇里黄村
2. 设备: 两台
3. 向日葵: 1801859604 x2m56x
4. 完成: 一台，另一台不需要


1. @梁丙政@徐佳飞@建德市杨村桥镇龙源村：（政务网安装不了向日葵）--QQ远程404770670两台需要升级固件

### 20250422 售后-建德手动升级固件

1. 建德市三都镇三江口村
2. 161937411 576yx2


1. 建德市梅城镇千鹤村 有三台设备需要升级
2. 1525378314 f2nt16
3. 三台完成
   
1. 1477449619 9192n3
2. 建德市梅城镇龙泉村

### 20250423 售后-建德手动升级固件

1. @徐佳飞@梁丙政建德市梅城镇顾家村：政务网，只能QQ远程：376048755，需升级固件--3台；
2. 建德市乾潭镇胥江村  设备固件不是最新 无法使用  需要远程升级 客户QQ：1005083198@徐佳飞@梁丙政谁有空呀

### 20250424 售后

**documentid: 20240424_01**


1. 烟台正海磁材公司（私有化+内网）YDA231201001031连续盖印时舱门开启与关闭时，有明显异响，麻烦排查处理一下；

日志观察:

1. 使用蓝牙连接
2. sealpattern==1 常规盖印

问题:
分析视频，得出结论
客户的盖印是沾墨章+普通用印，控制印章的电机会上下来回两次（声音是嘤嘤嘤的），第一次沾墨，第二次盖印，声音是在第一次印章下落接触舱门进行沾墨，以及第二次印章下落接触桌面文件时按压发出的声响，不是舱门开关的异响