---
id: 8rrf622y48npb0rbrwdvncw
title: Wifi
desc: ''
updated: 1751437793930
created: 1751421866711
---

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


state wpa_cli_prepare {
    [*] --> wlan_iface_ready : insmod wifi driver success
    wlan_iface_ready --> wlan_iface_up : interface up success
    wlan_iface_up --> wpa_supplicant_running : wpa_supplicant launched
    ' wpa_supplicant_running --> ip_obtained : udhcpc success
    ' ip_obtained --> READY
    wpa_supplicant_running --> wpa_cli_ready
}
wpa_cli_ready --> wpa_cli_status_disconnect: auto try_associate_by_configure_ssid
wpa_cli_status_disconnect --> wpa_cli_status_scaning: network_connect_fail or disable==1
wpa_cli_status_disconnect --> wpa_cli_status_completed: select network/ \n add network + reconnect
wpa_cli_status_scaning --> wpa_cli_status_completed: auto next ssid connected success

wpa_cli_status_completed --> wpa_cli_status_completed: udpcpc fail
wpa_cli_status_completed --> obtained_ip: udpcpc success

obtained_ip --> wlan_iface_up: killall wpa_supplicant
obtained_ip --> wpa_cli_status_disconnect: disconnect
' READY --> DISCONNECTED : 无连接记录

' DISCONNECTED --> CONNECTING
' CONNECTING --> CONNECTED : dhcp success

' CONNECTED --> DISCONNECTED : 网络断开
' CONNECTED --> SCANNING : 网络质量差触发 roam
' SCANNING --> CONNECTING : 选择网络后重新连接

' state CONNECTING {
'     [*] --> SCANNING
'     SCANNING --> ASSOCIATING
'     ASSOCIATING --> AUTHENTICATING
'     AUTHENTICATING --> 4WAY_HANDSHAKE
'     4WAY_HANDSHAKE --> GROUP_HANDSHAKE
'     GROUP_HANDSHAKE --> COMPLETED
'     COMPLETED --> WAIT_IP
'     WAIT_IP --> CONNECTED : DHCP 成功
' }


@enduml

```