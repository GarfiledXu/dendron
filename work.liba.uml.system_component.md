---
id: zjb9nyd3trc55qkdnc36mxk
title: System_component
desc: ''
updated: 1742281511035
created: 1742281381086
---



@startuml
' 使用手绘风格，使整体风格更像手绘漫画
skinparam Handwritten true  

' 组件外观调整
skinparam component {
    BackgroundColor #FFEE99
    BorderColor #FFAA33
    FontName Comic Sans MS
    FontSize 16
    FontColor #333333
    RoundCorner 15
    Shadowing true
}

' 连接线风格
skinparam ArrowColor #FF6600
skinparam ArrowThickness 3

title ydn system component diagram 

' 定义 main poll 组件
component "Main Poll" {
    component "MainWindow" as MainWindow <<UI Handler>> {
        skinparam BackgroundColor LightBlue
        skinparam BorderColor Blue
    }
    component "rk_event_poll" as rk_event_poll <<Event Handler>> {
        skinparam BackgroundColor LightGreen
        skinparam BorderColor DarkGreen
    }
}

' 定义下层模块组件
component "http_opt" as http_opt <<HTTP File Transfer>>
component "MqttOperationWorker" as MqttOperationWorker <<MQTT Handler>>
component "MediaModule" as MediaModule <<Media Processing>>
component "motorswitch" as motorswitch <<Motor Processing>>
component "fingerthread" as fingerthread <<Finger Processing>>
component "NetworkModule" as NetworkModule <<Network Config>> {
    skinparam BackgroundColor #D8BFD8
    skinparam BorderColor #9932CC
    component "4G Module" as Module4G <<4G Config>> 
    component "WLAN & BT Module" as WLAN_BT <<WLAN & Bluetooth>>
}

component "rk_nodethread" as rk_nodethread <<Node File Uploader>>

' 连接 main poll 到下层组件
MainWindow --> http_opt
MainWindow --> MqttOperationWorker
MainWindow --> MediaModule
MainWindow --> NetworkModule
MainWindow --> rk_nodethread
MainWindow --> motorswitch
MainWindow --> fingerthread

rk_event_poll --> http_opt
rk_event_poll --> MqttOperationWorker
rk_event_poll --> MediaModule
rk_event_poll --> NetworkModule
rk_event_poll --> rk_nodethread

' 网络模块包含子模块
NetworkModule --|> Module4G
NetworkModule --|> WLAN_BT

' WLAN & BT 模块转发数据到上层
WLAN_BT --> rk_event_poll : Forward Data

@enduml