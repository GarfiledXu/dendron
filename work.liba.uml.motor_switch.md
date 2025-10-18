---
id: d1f5lo4tw2tjikaozoe4cv5
title: Motor_switch
desc: ''
updated: 1748503015586
created: 1747798093164
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

' [*] --> 初始状态
' ..>

state rkpoll_thread {
    state "motor_sporting" as rkpoll_sporting
    state "operation_updated" as rkpoll_operation_updated
  [*] --> rkpoll_operation_updated
  rkpoll_sporting -left[dashed]-> timeout_check: reset check_flag
  timeout_check --> timeout_check
  timeout_check --> report_motor_error: timeout
  rkpoll_operation_updated --> rkpoll_operation_updated
  rkpoll_operation_updated --> rkpoll_sporting: (enable/disable motor gpio)\n (舱门电机: open/close)\n (下落电机: forward/backward)
  rkpoll_sporting --> rkpoll_sporting: keep action
}


state motor_switch_check_thread {
    state "motor_sporting" as limit_switch_motor_sporting
    limit_switch_combine_status_middle --> limit_switch_combine_status_action_finished
    limit_switch_combine_status_middle --> limit_switch_motor_sporting
    limit_switch_motor_sporting --> limit_switch_combine_status_middle
    ' limit_switch_combine_status_action_finished --> limit_switch_combine_status_middle
    limit_switch_combine_status_middle-->limit_switch_combine_status_middle: get each limit switch state\ncombine to one value
}
rkpoll_sporting-up[dashed]->limit_switch_combine_status_middle

state mainwindow_ui_thread {
    state "motor_sporting" as mainwindow_sporting
    mainwindow_sporting --> mainwindow_sporting:keep action
    operation_updated --> mainwindow_sporting: (enable/disable motor gpio)\n (舱门电机: open/close)\n (下落电机: forward/backward)
    operation_updated --> [*] : donoting
}
limit_switch_combine_status_action_finished-[dashed]->operation_updated: emit signal
mainwindow_sporting-[dashed]->limit_switch_combine_status_middle

' =========================
' 添加旁白说明
' =========================
' note bottom of rkpoll_thread
legend bottom left
=== 限位开关 === 
bit0: 舱门关，低有效
bit1: 舱门开，低有效
bit2: 印章复位，低有效
bit3: 盖章到位(O型板)，低有效
下落:收章:开门:关门

=== MOTOR_POSITON: rkpoll判断 ===
0: READY,
1: GATE_ON,
2: GATE_OFF,
3: STAMP_OUT,
4: STAMP_INING,
5: STAMP_IN

=== RST_STEP 枚举 === 
0: RST_READY      
1: MOTOR_IDLE
2: MOTORB_IN
3: MOTORB_OUT
4: MOTORA_OPEN
5: MOTORA_OFF
6: MOTOR_TEST
7: MOTORB_IN_DELAY
8: MOTORB_OUT_DELAY

=== 状态值含义 ===
      下落:收章:开门:关门
      可以拆分成两种动作，印章运动，舱门运动
      bit0-bit1或者bit3-bit2存在11，即表示印章在运动，或者舱门在运动处于中间状态
      前后对存在一个位有0，则表示到了限位开关状态，运动到位
      只有前后对都存在一个0，才没有运动状态，只有到位状态
      到底是关仓还是开仓，下落还是收回，需要根据上一个到位状态判断
0x0e: 1110 章运动中,舱门关闭，对应沾墨动作
0x0a: 1010 章收到位，舱门关闭  
0x05: 0101 章出到位，舱门打开  
0x0b: 1011 章收到位，舱门运动中打开或关闭
0x0d: 1101 章运动中，舱门打开 
0x09: 1001 收章到位，舱门已打开  
0x06: 0110 章出到位，舱门关闭，对应蘸墨到位
' end note

' note left of rkpoll_thread
' legend bottom
=== OPERATION 枚举（部分）===
-1: UILOGHIDE  
 0: IDLE  
 1: ADDINKING  
 2: FINGERENTER  
 3: FINGERREFUSE  
 4: FINGEREWAIT  
 5: STAMPSWTICH  
 6: STAMPSWTICH_FINISH  
 7: STAMPING_REMOTE  
 8: STAMP_RDY  
 9: STAMPING  
10: STAMP_START  
11: STAMP_DISPOSE  
12: STAMP_COMPELETE  
13: WAITEXAMINE  
14: PASSEXAMINE  
15: LOCKING  
16: UNLOCKING  
17: ADDINK_FINISH  
18: MOVE_WARN  
19: STAMP_REMOTE_ALLOW  
20: STAMP_REMOTE_RDY  
21: STAMP_WAITTING_COMMAND  
22: FINGERIMPORT  
23: WITH_INK  
24: LOCKED  
25: UNLOCKED  
26: SETLOCK  
27: REALSELOCK  
28: STAMP_PRERUN  
29: STAMP_WILL  
30: STAMP_CONSECUTIVE  
31: STAMP_CONSECUTIVE_RESET  
32: STAMP_CONSECUTIVE_WITH_INK  
33: STAMP_CONSECUTIVERDY  
34: STAMP_CONSECUTIVE_PAUSE  
35: STAMPSWTICH_CHECK  
36: VERIFY_FINGER  
37: UPGRADING
' end note
end legend

@enduml

```