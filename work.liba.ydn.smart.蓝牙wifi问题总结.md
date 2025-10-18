---
id: t1crvbalvi1zj525umredj1
title: 蓝牙wifi问题总结
desc: ''
updated: 1760328184647
created: 1760154208408
---

搭配原理图
当前模块关系

### bluetooth

1. 有概率，非必现，蓝牙连接建立成功后，天玺设备端作为 低功耗蓝牙的服务端，在正常通讯过程中，对应的fd会接收到EPOLLERR和EPOLLHUP事件(当前应用层蓝牙底层代码是从bluez源码中裁剪的低功耗蓝牙相关部分，bluez底层通过epoll机制监听蓝牙fd事件，事件来源系统内核的l2cap协议栈或者蓝牙hci驱动)，引发的具体原因暂未排查到，此时服务端无法进行fd的write(对应业务操作即消息发送)，但还能进行read(若是客户端此时发送消息)
2. 蓝牙驱动 系统内核 用户态的蓝牙代码

```plantuml

' hide empty description 隐藏状态的空描述
hide empty description

' ==== 状态图样式：天蓝可爱风 ====
skinparam state {
    ' 状态背景色：浅天蓝
    BackgroundColor #B3E5FC

    ' 状态边框色：亮天蓝
    BorderColor #03A9F4

    ' 状态字体颜色：深蓝
    ' FontColor #01579B
    FontColor #03253f

    ' 字体大小
    FontSize 14

    ' 开启立体阴影
    ' Shadowing true
}

skinparam shadowing true

' 箭头颜色：亮蓝
skinparam ArrowColor #4FC3F7

' 箭头字体颜色：中蓝
' skinparam ArrowFontColor #0288D1
skinparam ArrowFontColor #028b14

skinparam ArrowFontSize 12
skinparam ArrowThickness 1.5

' 背景白色，简洁
skinparam backgroundColor #FFFFFF
skinparam dpi 150

' 定义排列方向
top to bottom direction


state user_space {
  BLE_CLIENT --> BLE_SERVER: l2cap link
  BLE_SERVER --> BLE_SERVER: epoll_wait: l2cap fd
}
state kernel_l2cap_protocol_stack {

}

state HCI_controller_driver{

}

初始化完成 --> 工作中 : 用户操作
工作中 --> 异常状态 : 错误发生
工作中 --> 结束状态 : 正常退出
异常状态 --> 结束状态 : 修复或退出

@enduml

```

```plantuml
@startuml

' hide empty description 隐藏状态的空描述
hide empty description

' ==== 状态图样式：天蓝可爱风 ====
skinparam state {
    ' 状态背景色：浅天蓝
    BackgroundColor #B3E5FC

    ' 状态边框色：亮天蓝
    BorderColor #03A9F4

    ' 状态字体颜色：深蓝
    FontColor #01579B

    ' 字体大小
    FontSize 14

    ' 开启立体阴影
    ' Shadowing true
}

skinparam shadowing true

' 箭头颜色：亮蓝
skinparam ArrowColor #4FC3F7

' 箭头字体颜色：中蓝
skinparam ArrowFontColor #0288D1

skinparam ArrowFontSize 12
skinparam ArrowThickness 1.5

' 背景白色，简洁
skinparam backgroundColor #FFFFFF
skinparam dpi 150

state kernel_l2cap_protocol_stack {
    [*] --> L2CAP_CONNECT_REQ
    L2CAP_CONNECT_REQ --> L2CAP_CONFIG
    L2CAP_CONFIG --> L2CAP_OPEN: channel established
    L2CAP_OPEN --> L2CAP_DATA: data exchange
    L2CAP_DATA --> L2CAP_OPEN
    L2CAP_OPEN --> L2CAP_DISCONNECT: disconnect request
    L2CAP_DISCONNECT --> [*]
}

state HCI_controller_driver {
    [*] --> HCI_INIT
    HCI_INIT --> HCI_CONNECTED: link established (CONNECT_COMPLETE)
    HCI_CONNECTED --> HCI_DATA: TX/RX ACL packets
    HCI_DATA --> HCI_CONNECTED
    HCI_CONNECTED --> HCI_DISCONNECTED: disconnect (reason=0x13 etc)
    HCI_DISCONNECTED --> [*]
}

BLE_CLIENT --> kernel_l2cap_protocol_stack: write(l2cap_fd)\nL2CAP_CONNECT_REQ
kernel_l2cap_protocol_stack --> HCI_controller_driver: send ACL Data (CONNECT_REQ)
HCI_controller_driver --> kernel_l2cap_protocol_stack: CONNECT_COMPLETE
kernel_l2cap_protocol_stack --> BLE_SERVER: wake epoll (EPOLLIN)

@enduml
```

```plantuml
@startuml

' hide empty description 隐藏状态的空描述
hide empty description

' ==== 状态图样式：天蓝可爱风 ====
skinparam state {
    ' 状态背景色：浅天蓝
    BackgroundColor #B3E5FC

    ' 状态边框色：亮天蓝
    BorderColor #03A9F4

    ' 状态字体颜色：深蓝
    FontColor #01579B

    ' 字体大小
    FontSize 14

    ' 开启立体阴影
    ' Shadowing true
}

skinparam shadowing true

' 箭头颜色：亮蓝
skinparam ArrowColor #4FC3F7

' 箭头字体颜色：中蓝
skinparam ArrowFontColor #0288D1

skinparam ArrowFontSize 12
skinparam ArrowThickness 1.5

' 背景白色，简洁
skinparam backgroundColor #FFFFFF
skinparam dpi 150


' ---------------- 用户态 ----------------
state user_space {
    [*] --> BLE_SERVER
    BLE_SERVER --> EPOLL_MAINLOOP: epoll_wait(l2cap_fd)
    EPOLL_MAINLOOP --> BLE_SERVER: EPOLLERR / EPOLLHUP
    BLE_SERVER --> CLEANUP: close(fd), remove from mainloop
    CLEANUP --> [*]
}

' ---------------- 内核 L2CAP ----------------
state kernel_l2cap_protocol_stack {
    [*] --> L2CAP_OPEN
    L2CAP_OPEN --> L2CAP_ERROR: sk->sk_err set (ECONNRESET / ENETDOWN)
    L2CAP_OPEN --> L2CAP_HUP: channel closed
    L2CAP_ERROR --> sock_error_report: wakeup epoll
    L2CAP_HUP --> sock_error_report: wakeup epoll
    L2CAP_ERROR --> [*]
    L2CAP_HUP --> [*]
}

' ---------------- HCI 驱动 & 控制器 ----------------
state HCI_controller_driver {
    [*] --> HCI_CONNECTED
    HCI_CONNECTED --> HCI_ERROR: controller I/O error\n(e.g. UART drop / firmware reset)
    HCI_CONNECTED --> HCI_DISCONNECTED: Disconnect_Complete
    HCI_ERROR --> report_to_l2cap: notify L2CAP error
    HCI_DISCONNECTED --> report_to_l2cap: notify L2CAP disconnect
    report_to_l2cap --> [*]
}

' ---------------- 层间传播 ----------------
HCI_controller_driver --> kernel_l2cap_protocol_stack: HCI_ERROR / Disconnect_Complete
kernel_l2cap_protocol_stack --> user_space: sk_error_report() → EPOLLERR / EPOLLHUP

@enduml
```


```plantuml
@startuml
hide empty description

skinparam state {
    BackgroundColor #B3E5FC
    BorderColor #03A9F4
    FontColor #01579B
    FontSize 14
}
skinparam shadowing true
skinparam ArrowColor #4FC3F7
skinparam ArrowFontColor #0288D1
skinparam ArrowFontSize 12
skinparam ArrowThickness 1.5
skinparam backgroundColor #FFFFFF
skinparam dpi 150

skinparam state {
    BackgroundColor<<user>> #E8F0FE
    BackgroundColor<<kernel>> #F8F8F8
    BackgroundColor<<hci>> #F8F8F8
}

' ------------------ User Space ------------------
state user_space <<user>> {
    state BLE_CLIENT
    state BLE_SERVER {
        state epoll_wait
        state fd_ready
        state normal_rw
        state fd_error
        state error_handle

        ' 正常读写流程
        epoll_wait --> fd_ready : EPOLLIN / EPOLLOUT
        fd_ready --> normal_rw : read/write

        ' 异常流程
        epoll_wait -[#red]-> fd_error : EPOLLERR / EPOLLHUP
        fd_error -[#red]-> error_handle : 异常处理
    }

    BLE_CLIENT --> BLE_SERVER : l2cap link
}

' ------------------ Kernel / HCI ------------------
state kernel_l2cap_protocol_stack <<kernel>> {
    state l2cap_event
    l2cap_event --> epoll_wait : 通知应用 fd 可读/可写
}

state HCI_controller_driver <<hci>> {
    state hci_event
    hci_event --> l2cap_event : HCI 数据传递
}

@enduml


```

### wifi

1. 遇到较低概率，在wpa_supplicant后台服务运行存活时，应用通过wpa_cli命令发送指令失效，如正常发送connect等操作指令，没有任何结果返回，最终在业务层表现为操作超时

### 4G

1. 仅一个物理串口，该设备节点在拨号成功后，会被pppd进程持有上锁，其他进程无法访问，会在打开设备时阻塞，除非pppd进程结束(意味着终止4g通信)