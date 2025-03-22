---
id: td1puzwq37oaiiqs2io5qlo
title: System_sequence
desc: ''
updated: 1742281264625
created: 1742281187768
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

title 系统交互时序图

footer Author: 徐佳飞 | Date: 2025-03-17



actor User as A_U
participant Devices as P_D
participant App_Frontend as P_A
participant Cloud_Backend as P_C


activate P_D

autonumber 1.0
alt 设备状态已开机
A_U -> P_D: 长按关机
activate P_D #33aa77
P_D->P_D: 进入设备即将关闭页面\n显示倒计时3秒图标
  alt 长按超2秒
  P_D->P_D: 关机息屏
  else 长按不足2秒
    P_D->P_D: 退出设备即将关闭页面
    P_D->P_D: 进入用户请登录页面
  end
deactivate P_D #33aa77

else 设备状态已关机机
A_U -> P_D: 长按开机
activate P_D #33aa77
autonumber 1.2
P_D -> P_D: 屏幕点亮，品牌logo+名称玺得安页面
P_D -> P_D: 动画标语:守护单位用印安全
P_D -> P_D: 进入设备初始化页面
end
deactivate P_D #33aa77





group 网络处理
P_D -> P_D: 印章主界面(设备初始化): 印章图标+版本号+设备ID \n +当前印章名称+状态栏
activate P_D #33aa77
autonumber 1.4.1
P_D -> P_D: 连接网络
P_D -> P_D: 连接服务器
P_D -> P_D: 更新状态栏图标 wifi 4g
P_D -> P_D: 初始化完成:提示用户请登陆
deactivate P_D #33aa77
end


autonumber 1.5
alt wifi未连接，使用蓝牙
P_D<-->P_A: 建立连接
note right: App端设备管理栏显示 \n设备在线，wifi未连接

autonumber 1.5.1
P_A->P_A: 进入设备详情面板
activate P_A 
P_A->P_A: 进入网络配置面板
P_A<--->P_D: 蓝牙通信
'连接成功 开始配置生命体
activate P_D  #33aa77
P_D->P_D: 设备状态栏网络图标切换为蓝牙
note left: 此时其他用户无法进入
P_D->P_D: 设备进入用户登陆状态\n语音播报提示印章已连接\n显示"<用户名称>"
'开始app端进行设置同步
P_A-->P_D: 进行网络配置，切换，取消，忘记
P_A->P_A: 退出网络配置面板
P_A-->P_D: 同步断开连接
'结束app端设置同步
P_D->P_D: 完成设置\n语音播报用户已退出\n显示"用户请登录"
note left #ffcccc : 实验bug: 即使网络配置成功\n状态栏图标无法恢复,依旧是蓝牙
deactivate P_D #33aa77
'结束配置生命体
deactivate P_A

else wifi连接

P_D<-->P_A: 建立连接
note right: App端设备管理栏显示 \n设备在线，wifi连接

end
note left of P_D: 总结用户的登录退出\napp端进入设备详情中具体的功能面板时，用户登录\n而当退出用户设备管理详情时，到主导航栏时，才退出用户登录


'换章流程

alt 设备已绑定印章，则触发卸章

'app 操作
autonumber 2.1.1
P_A->P_A: 进入设备详情面板
'app操作生命周期开始
activate P_A 
P_A->P_A: 进入网络配置面板
P_A--->P_D: 开始通信

'设备端换章周期开始
activate P_D  #33aa77
P_D->P_D: 设备进入用户登陆状态\n语音播报提示印章已连接\n显示"<用户名称>"
' app端选择印章
P_A->P_A : 选择印章\n确认卸章
P_A--->P_D: 通信同步
P_D->P_D: 执行机械动作\n仓梦打开，电机下推\n语音播报仓门已打开\n显示"请更换印章"
P_A->P_A: 设置面板点击确认"完成卸章"
P_A-->P_D: 通信同步
P_D->P_D: 执行机械动作\n仓梦关闭，电机上拉\n语音播报仓门关闭\n显示"无印章"
P_D-->P_A: 通信同步
P_A->P_A: 设置面板自动退出回设备详情面板
'设备端换章周期结束
deactivate P_D #33aa77
'app操作生命周期结束
deactivate P_A 

else 设备无印章，则触发换章

'app 操作
autonumber 2.2.1
P_A->P_A: 进入设备详情面板
'app操作生命周期开始
activate P_A 
P_A->P_A: 进入网络配置面板
P_A--->P_D: 开始通信

'设备端换章周期开始
activate P_D  #33aa77
P_D->P_D: 设备进入用户登陆状态\n语音播报提示印章已连接\n显示"<用户名称>"
' app端选择印章
P_A->P_A : 选择印章\n确认"印章"
P_A--->P_D: 通信同步
P_D->P_D: 执行机械动作\n仓梦打开，电机下推\n语音播报仓门已打开\n显示"请更换印章"
P_A->P_A: 设置面板点击确认"完成装章"
P_A-->P_D: 通信同步
P_D->P_D: 执行机械动作\n仓梦关闭，电机上拉\n语音播报仓门关闭\n显示"无印章"
P_D-->P_A: 通信同步
P_A->P_A: 设置面板自动退出回设备详情面板

'设备端换章周期结束
deactivate P_D #33aa77
'app操作生命周期结束
deactivate P_A 

end


''''''指纹删除
autonumber 3.1.1
group 指纹删除
P_A->P_A: 进入设备详情面板
'开始dev生命周期
activate P_D #33aa77
'开始app生命周期
activate P_A 
P_A->P_A: 进入指纹删除面板
P_A->P_A: 选择指纹，确认删除
P_A--->P_D: 通信同步
end
'结束app生命周期
deactivate P_A
deactivate P_D

'''''指纹录入
autonumber 3.2.1
group 指纹录入
P_A->P_A: 进入设备详情面板
'开始dev生命周期
activate P_D #33aa77
'开始app生命周期
activate P_A 
P_A->P_A: 进入指纹录入面板
P_A->P_A: 选择人员
P_A->P_A: 点击开始录入按钮
P_A--->P_D: 通信同步
'P_D操作
P_D->P_D: 进入指纹录入页面\n指纹图片\n录入次数\n显示"请录入指纹"\n语音播开始录入指纹
  loop 录入三次
    A_U->P_D: 录入一次
    P_D->P_D: 更新指纹录入页面\n指纹图片更新\n录入次数+1\n显示"请录入指纹"\n语音波爆请再按一次
  end

P_D->P_D: 指纹录入页面\n指纹图片\n录入次数3\n显示"请录入指纹"\n语音播报指纹录入成功
P_D-->P_A: 通信同步
'P_A同步结果
P_A->P_A: 面板跳出提示: 上传中
P_A--->P_D: 通信同步
P_D->P_D: 自动退回到欢迎使用印章界面
P_A->P_A: 面板自动跳出退回到设备详情面板
'结束app生命周期
end
deactivate P_A
deactivate P_D

''''' 沾墨

''''' 加墨

''''' 固件升级

''''' 油箱管理

'''''普通用印
autonumber 4.1
group 普通用印
P_A->P_A: 进入设备详情面板
'开始dev生命周期
activate P_D  #33aa77
'开始app生命周期
activate P_A 
P_A->P_A: 点击主导航栏的用印申请
P_A->P_A: 盖印人:填写申请用印表，选择普通用印，提交
P_A->P_A: 审批人:审批申请用印通过
P_A->P_A: 盖印人:选择对应待用印文件，点击开始用印
P_A->P_A: 进入设备连接面板
P_A--->P_D: 通信同步
'P_D操作
P_D->P_D: 进入欢迎使用页面\n语音播报"印章已连接"
P_D-->P_A: 通信同步
P_A->P_A: 进入盖印中面板
P_D->P_D: 进入指纹验证页面\n语音播报"请验证指纹"
A_U->P_D: 完成指纹验证
P_D->P_D: 进入欢迎使用页面\n无语音播报\n显示剩余印章次数
A_U->P_D: 摆放好印章与待印文件
loop 总用印次数n
  P_A->P_A: 拍摄用印前照片，点击提交
  A_U->P_D: 点击按钮
  A_U->P_D: 执行机械结构: 开舱门，下推电机，盖章
  A_U->P_D: 进入欢迎使用页面\n无语音播报\n显示剩余印章次数-1
  A_U->P_D: 执行机械结构: 关舱门，上拉电机
  A_U->P_D: 进入欢迎使用页面\n语音播报"用印完成"\n显示剩余印章次数-1
  P_D-->P_A: 通信同步
  P_A->P_A: 拍摄用印后照片，点击提交
end
P_A->P_A: 电机待退出用印面板
P_D-->P_A: 通信同步
P_D->P_D: 进入用户请登录页面\n语音播报"用户已退出"
P_A->P_A: 在用印记录查看所有记录信息
'结束app生命周期
end
deactivate P_A
deactivate P_D #33aa77

'''''连续用印 造假
autonumber 4.2
group 连续用印
P_A->P_A: 进入设备详情面板
'开始dev生命周期
activate P_D #33aa77
'开始app生命周期
activate P_A 
P_A->P_A: 点击主导航栏的用印申请
P_A->P_A: 盖印人:填写申请用印表，选择连续用印，提交
P_A->P_A: 审批人:审批申请用印通过
P_A->P_A: 盖印人:选择对应待用印文件，点击开始用印
P_A->P_A: 进入设备连接面板
P_A--->P_D: 通信同步
'P_D操作
P_D->P_D: 进入欢迎使用页面\n语音播报"印章已连接"
P_D-->P_A: 通信同步
P_A->P_A: 进入盖印中面板
P_D->P_D: 进入指纹验证页面\n语音播报"请验证指纹"
A_U->P_D: 完成指纹验证
P_D->P_D: 进入欢迎使用页面\n无语音播报\n显示剩余印章次数
A_U->P_D: 摆放好印章与待印文件
A_U->P_D: 点击按钮
A_U->P_D: 执行机械结构: 开舱门，下推电机
loop 总用印次数n
  P_A->P_A: 拍摄用印前照片，点击提交
  A_U->P_D: 按压盖印
  A_U->P_D: 进入欢迎使用页面\n无语音播报\n显示剩余印章次数-1
  P_D-->P_A: 通信同步
  P_A->P_A: 拍摄用印后照片，点击提交
end
A_U->P_D: 执行机械结构: 关舱门，上拉电机
P_A->P_A: 电机待退出用印面板
P_D-->P_A: 通信同步
P_D->P_D: 进入用户请登录页面\n语音播报"用户已退出"
P_A->P_A: 在用印记录查看所有记录信息
'结束app生命周期
end
deactivate P_A
deactivate P_D #33aa77

'''''远程用印
autonumber 4.3
group 远程用印
P_A->P_A: 进入设备详情面板
'开始dev生命周期
activate P_D #33aa77
'开始app生命周期
activate P_A 
P_A->P_A: 点击主导航栏的用印申请
P_A->P_A: 盖印人:填写申请用印表，选择远程用印，选择远程确认人，提交
P_A->P_A: 审批人:审批申请用印通过
P_A->P_A: 盖印人:选择对应待用印文件，点击开始用印
P_A->P_A: 进入设备连接面板
P_A--->P_D: 通信同步
'P_D操作
P_D->P_D: 进入欢迎使用页面\n语音播报"印章已连接"
P_D-->P_A: 通信同步
P_A->P_A: 进入盖印中面板
P_D->P_D: 进入指纹验证页面\n语音播报"请验证指纹"
A_U->P_D: 完成指纹验证
P_D->P_D: 进入欢迎使用页面\n无语音播报\n显示剩余印章次数
A_U->P_D: 摆放好印章与待印文件
loop 总用印次数n
  P_A->P_A: 拍摄用印前照片，点击提交
  P_A->P_A: 远程确认人确认
  A_U->P_D: 点击按钮
  A_U->P_D: 执行机械结构: 开舱门，下推电机，盖章
  A_U->P_D: 进入欢迎使用页面\n无语音播报\n显示剩余印章次数-1
  A_U->P_D: 执行机械结构: 关舱门，上拉电机
  A_U->P_D: 进入欢迎使用页面\n语音播报"用印完成"\n显示剩余印章次数-1
  P_D-->P_A: 通信同步
  P_A->P_A: 拍摄用印后照片，点击提交
end
P_A->P_A: 电机待退出用印面板
P_D-->P_A: 通信同步
P_D->P_D: 进入用户请登录页面\n语音播报"用户已退出"
P_A->P_A: 在用印记录查看所有记录信息
'结束app生命周期
end
deactivate P_A
deactivate P_D #33aa77



''''' 特权用印
autonumber 4.4
group 特权用印
'A_U操作
A_U->P_D: 指纹验证
'开始dev生命周期
activate P_D #33aa77
'开始app生命周期
activate P_A 
alt 验证失败
P_D->P_D: 欢迎使用页面\n无语音播报\n显示"指纹验证失败"
P_D->P_D: 回到进入用户请登录页面\n
else 验证成功
P_D->P_D: 欢迎印章解锁页面\n语音播报"印章已解锁"\n显示"欢迎<用户名>使用"
P_D->P_D: 欢迎使用页面\n语音播报"请点击按钮，开始用印"\n显示"欢迎<用户名>使用"\n图标更新为特权用户
  alt 启动用印
    loop 用印次数n
    A_U->P_D: 点击按钮
    A_U->P_D: 执行机械结构: 开舱门，下推电机
    A_U->P_D: 按压盖印
    end
  else 结束用印
    A_U->P_D: 长按按钮，退出
    P_D->P_D: 进入用户请登录页面\n语音播报:用户已退出
    P_A->P_A: 在用印记录查看所有记录信息
  end
end
end
deactivate P_D #33aa77
deactivate P_A


deactivate P_D

@enduml