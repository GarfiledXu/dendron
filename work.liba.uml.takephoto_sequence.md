---
id: fxpdgs3fi8mrls5xgfupuec
title: Takephoto_sequence
desc: ''
updated: 1742456464632
created: 1742354188037
---


@startuml

participant main_process T_M
participant takephoto_process T_T1
participant takephoto_process_localserver T_T2
participant takephoto_process_event_loop T_T3
participant takephoto_process_takethread T_T4

'主线程最终调用 mediamodule中的initConnects(){定义emit:MediaModule::initMediaService->映射CaptureWorker::init，触发MediaModule::initMediaService} 
'CaptureWorker::init(){定义emit:QTimer::timeout->映射CaptureWorker::checkUserdataPartSizeAndProcessIsIdle}
'
autonumber
T_M->




@enduml