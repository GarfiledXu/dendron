---
id: z60fc9dtvv4yszij1yjthru
title: 蓝牙wifi问题总结3
desc: ''
updated: 1760401597144
created: 1760360529659
---

### smart_v05 蓝牙 wifi当前问题总结

### bluetooth

1. 有概率，非必现，蓝牙连接建立成功后，天玺设备端作为 低功耗蓝牙的服务端，在正常通讯过程中，对应的fd会接收到EPOLLERR和EPOLLHUP事件(当前应用层蓝牙底层代码是从bluez源码中裁剪的低功耗蓝牙相关部分，bluez底层通过epoll机制监听蓝牙fd事件，事件来源系统内核的l2cap协议栈或者蓝牙hci驱动)，引发的具体原因暂未排查到，此时服务端无法进行fd的write(对应业务操作即消息发送)，但还能进行read(若是客户端此时发送消息).

```plantuml

@startuml

hide empty description
skinparam state {
    BackgroundColor #B3E5FC
    BorderColor #03A9F4
    FontColor #03253f
    FontSize 14
}

skinparam shadowing true
skinparam ArrowColor #4FC3F7
skinparam ArrowFontColor #028b14
skinparam ArrowFontSize 12
skinparam ArrowThickness 1.5
skinparam backgroundColor #FFFFFF
skinparam dpi 150
top to bottom direction

' ---------------- 用户态 ----------------
state user_space_bluez {
    ' [*] --> BLE_CLIENT客户端
    [*] --> BLE_SERVER服务端
    ' BLE_CLIENT客户端 --> BLE_SERVER服务端: 链接管理 / 数据收发
    BLE_SERVER服务端 --> epoll_mainloop: epoll_wait
    epoll_mainloop --> BLE_SERVER服务端
    epoll_mainloop --> fd_ready: EPOLLIN|EPOLLOUT
    epoll_mainloop -[#red]-> fd_error: EPOLLERR  / EPOLLHUP 
    fd_error -[#red]-> timeout_disconnect
    fd_ready --> write_read_data
    timeout_disconnect --> BLE_CLIENT客户端
    write_read_data --> BLE_CLIENT客户端
}

' ---------------- 内核 L2CAP ----------------
state kernel_l2cap_protocol_stack {
    [*] --> L2CAP_LINK: 链路管理
    L2CAP_LINK --> L2CAP_DATA: 数据收发
    L2CAP_LINK --> L2CAP_ERROR: 错误 / 挂断
    L2CAP_DATA --> L2CAP_LINK
    L2CAP_ERROR --> sock_error_report: 通知 epoll
}

' ---------------- HCI 驱动 & 控制器 ----------------
state HCI_controller_driver {
    [*] --> HCI_LINK: 控制器管理
    HCI_LINK --> HCI_DATA: 数据传输
    HCI_LINK --> HCI_ERROR: 控制器异常 / 断开
    HCI_DATA --> HCI_LINK
    HCI_ERROR --> report_to_l2cap: 上报 L2CAP
}

' ---------------- 层间交互 ----------------
HCI_controller_driver --> kernel_l2cap_protocol_stack: 异常 / 事件
' kernel_l2cap_protocol_stack --> user_space: epoll 事件
kernel_l2cap_protocol_stack --> epoll_mainloop: epoll 事件

@enduml

```

### wifi

1. 遇到较低概率，在wpa_supplicant后台服务运行存活时，应用通过wpa_cli命令发送指令失效，如正常发送connect等操作指令，没有任何结果返回，最终在业务层表现为操作超时.

```plantuml

@startuml
hide empty description

skinparam state {
    BackgroundColor #B3E5FC
    BorderColor #03A9F4
    FontColor #03253f
    FontSize 14
}
skinparam shadowing true
skinparam ArrowColor #4FC3F7
skinparam ArrowFontColor #028b14
skinparam ArrowFontSize 12
skinparam ArrowThickness 1.5
skinparam backgroundColor #FFFFFF
skinparam dpi 150
top to bottom direction

' ---------------- 用户态 ----------------
state user_space {
    ' [*] --> wpa_supplicant服务: 启动后台服务 (wpa_supplicant)
    [*] --> app
    app --> connect_success
    connect_success --> udhcpc: 路由优先级切换/动态ip获取
    udhcpc --> socket: data_transform
    app --> wpa_supplicant服务: 后台服务left_control
    app --> wpa_cli: opt (connect / scan / status / disconnect)
    app --> wpa_cli_lib接口: event_subscribe
    wpa_cli_lib接口 --> app: event_publish
    wpa_supplicant服务 --> wpa_cli_lib接口: domain_socket

    wpa_cli --> wpa_supplicant服务: domain_socket
    wpa_supplicant服务 --> wpa_cli
}

@enduml



```


