---
id: 69px43knnkxztait9eygjwr
title: Msg_record
desc: ''
updated: 1777373196815
created: 1777013186286
---

## 点击配置相机

```bash
[SEND] Set_WorkMode
[RECV] Set_WorkMode | Set_WorkMode-#OK
[SEND] Get_TrigParam
[RECV] Get_TrigParam | Get_TrigParam-#{"mode":0,"inner_delay":0,"ext_delay":0,"enable":0}
[SEND] Get_SnapCfg
[RECV] Get_SnapCfg | Get_SnapCfg-#{"base":{"exp":2200,"gain":1,"light_mode":1,"light_type":1},"focus":{"pos":410},"ai":{"enable":0,"brightness_auto":0,"focus_auto":0},pe_hor":0,"trape_ver
[SEND] Get_ProgInfo
[RECV] Get_ProgInfo | Get_ProgInfo-#{"tool_count":1,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}
[SEND] Get_MasterImg
[SEND] Get_ProgInfo
[RECV] Get_ProgInfo | Get_ProgInfo-#{"tool_count":1,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}
[SEND] ATool_GetAlias
[RECV] ATool_GetAlias | ATool_GetAlias-#{"node_name":""}
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":404.37200927734375,"y":503.6929931640625},{"x":803.544982

## 确定注册基准


```bash
[SEND] Get_ProgInfo
[RECV] Get_ProgInfo | Get_ProgInfo-#{"tool_count":1,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}
[SEND] ATool_GetAlias
[RECV] ATool_GetAlias | ATool_GetAlias-#{"node_name":""}
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":929931640625},{"x":803.544982
[SEND] Get_ProgInfo
[RECV] Get_ProgInfo | Get_ProgInfo-#{"tool_count":1,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}
[SEND] ATool_GetAlias
[RECV] ATool_GetAlias | ATool_GetAlias-#{"node_name":""}
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":404.37200927734375,"y":503.6929931640625},{"x":803.544982
```


## 添加工具 ocr


```bash
[SEND] ATool_Add
[RECV] ATool_Add | ATool_Add-#{"id":"T01"}
[SEND] Set_ViSrc
[RECV] Set_ViSrc | Set_ViSrc-#OK
[SEND] Import_Img
[RECV] Import_Img | Import_Img-#{"status":"OK","target_cmd":"Set_ViSrc"}
[OcrDateTimeVM] buildParamPayload: baseRegion is invalid!
[SEND] ATool_SetParam
[RECV] ATool_SetParam | ATool_SetParam-#OK
[SEND] ATool_SetDepend
[RECV] ATool_SetDepend | ATool_SetDepend-#OK
[SEND] Get_ProgInfo
[RECV] Get_ProgInfo | Get_ProgInfo-#{"tool_count":2,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"},{"id":"T01","type":"OCR_TOOL"}],"master_count":1,
[SEND] ATool_GetAlias
[RECV] ATool_GetAlias | ATool_GetAlias-#{"node_name":""}
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":929931640625},{"x":803.544982
[SEND] ATool_GetAlias
[RECV] ATool_GetAlias | ATool_GetAlias-#{"node_name":""}
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":0,"y":0},{"x":0,"y":0}]}],"da
[SEND] Set_ViSrc
[RECV] Set_ViSrc | Set_ViSrc-#OK
[SEND] Import_Img
[RECV] Import_Img | Import_Img-#{"status":"OK","target_cmd":"Set_ViSrc"}
[SEND] Trigger_Read
[RECV] Trigger_Read | Trigger_Read-#OK
[RECV] Report_Result | {"res_id":6742,"tot_res":{"status":-1,"vi_time":1,"vi_time_avg":13,"vi_time_max":23,"vi_time_min":0,"cycle_time":206,"cycle_time_avg":213,"cycle_time_max":959,"cycle_time_min":0},"atools":[{"node_id":
```

## 手动拖动roi框选


```bash
[SEND] ATool_SetParam
[RECV] ATool_SetParam | ATool_SetParam-#OK
[SEND] ATool_SetDepend
[RECV] ATool_SetDepend | ATool_SetDepend-#OK
[SEND] Trigger_Read
[RECV] Trigger_Read | Trigger_Read-#OK
[RECV] Report_Result | {"res_id":6743,"tot_res":{"status":-1,"vi_time":1,"vi_time_avg":13,"vi_time_max":23,"vi_time_min":0,"cycle_time":386,"cycle_time_avg":213,"cycle_time_max":959,"cycle_time_min":0},"atools":[{"node_id":
```


## 编辑修改判断条件确认后


```bash
[SEND] ATool_SetParam
[RECV] ATool_SetParam | ATool_SetParam-#OK
[SEND] ATool_SetDepend
[RECV] ATool_SetDepend | ATool_SetDepend-#OK
[SEND] Trigger_Read
[RECV] Trigger_Read | Trigger_Read-#OK
[RECV] Report_Result | {"res_id":6744,"tot_res":{"status":-1,"vi_time":1,"vi_time_avg":12,"vi_time_max":23,"vi_time_min":0,"cycle_time":385,"cycle_time_avg":213,"cycle_time_max":959,"cycle_time_min":0},"atools":[{"node_id":
```


## 工具设定确认后


```bash
[SEND] Get_ProgInfo
[RECV] Get_ProgInfo | Get_ProgInfo-#{"tool_count":2,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"},{"id":"T01","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}
[SEND] ATool_GetAlias
[RECV] ATool_GetAlias | ATool_GetAlias-#{"node_name":""}
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":404.37200927734375,"y":503.6929931640625},{"x":803.544982
[SEND] ATool_GetAlias
[RECV] ATool_GetAlias | ATool_GetAlias-#{"node_name":""}
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":415.71600341796875,"y":496.1300048828125},{"x":735.716003
[SEND] Set_ViSrc
[RECV] Set_ViSrc | Set_ViSrc-#OK
[SEND] Get_ProgInfo
[RECV] Get_ProgInfo | Get_ProgInfo-#{"tool_count":2,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"},{"id":"T01","type":"OCR_TOOL"}],"master_count":1,
[SEND] ATool_GetAlias
[RECV] ATool_GetAlias | ATool_GetAlias-#{"node_name":""}
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":929931640625},{"x":803.544982
[SEND] ATool_GetAlias
[RECV] ATool_GetAlias | ATool_GetAlias-#{"node_name":""}
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":415.71600341796875,"y":496.1300048828125},{"x":735.716003
```


## 程序配置环节，不是以添加工具而是编辑已有工具


```bash
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":415.71600341796875,"y":496.1300048828125},{"x":735.716003
[SEND] ATool_GetAlias
[RECV] ATool_GetAlias | ATool_GetAlias-#{"node_name":""}
[SEND] Set_ViSrc
[RECV] Set_ViSrc | Set_ViSrc-#OK
[SEND] Import_Img
[RECV] Import_Img | Import_Img-#{"status":"OK","target_cmd":"Set_ViSrc"}
[SEND] Trigger_Read
[RECV] Trigger_Read | Trigger_Read-#OK
[RECV] Report_Result | {"res_id":6745,"tot_res":{"status":-1,"vi_time":1,"vi_time_avg":12,"vi_time_max":23,"vi_time_min":0,"cycle_time":381,"cycle_time_avg":213,"cycle_time_max":959,"cycle_time_min":0},"atools":[{"node_id":
```

## 完成工具设定


```bash
[SEND] Save_Config
[RECV] Save_Config | Save_Config-#OK
[SEND] Get_ProgList
[RECV] Get_ProgList | Get_ProgList-#{"count":32,"list":[{"id":0,"name":"prog_0","work_mode":"standard"},{"id":1,"name":"prog_1","work_mode":"standard"},{"id":2,"name":"prog_2","work_mode":"standard"},{"id":3,"name":"prog_3
```

## 点击运行-停止


```bash
[SEND] Get_ProgInfo
[RECV] Get_ProgInfo | Get_ProgInfo-#{"tool_count":2,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"},{"id":"T01","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}
[SEND] Get_ProgInfo
[RECV] Get_ProgInfo | Get_ProgInfo-#{"tool_count":2,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"},{"id":"T01","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":404.37200927734375,"y":503.6929931640625},{"x":803.544982
[SEND] Get_AtoolUserParam
[RECV] Get_AtoolUserParam | Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":415.71600341796875,"y":496.1300048828125},{"x":735.716003
[SEND] Set_AutoSend
[RECV] Set_AutoSend | Set_AutoSend-#OK
[SEND] Run
[RECV] Run | Run-#OK
[RECV] Report_Result | {"res_id":6757,"tot_res":{"status":-1,"vi_time":13,"vi_time_avg":12,"vi_time_max":23,"vi_time_min":0,"cycle_time":393,"cycle_time_avg":214,"cycle_time_max":959,"cycle_time_min":0},"atools":[{"node_id"
[RECV] Report_Result | {"res_id":6758,"tot_res":{"status":-1,"vi_time":13,"vi_time_avg":12,"vi_time_max":23,"vi_time_min":0,"cycle_time":394,"cycle_time_avg":214,"cycle_time_max":959,"cycle_time_min":0},"atools":[{"node_id"
[SEND] Stop
[RECV] Stop | Stop-#OK
[RECV] Report_Result | {"res_id":6759,"tot_res":{"status":-1,"vi_time":13,"vi_time_avg":12,"vi_time_max":23,"vi_time_min":0,"cycle_time":398,"cycle_time_avg":214,"cycle_time_max":959,"cycle_time_min":0},"atools":[{"node_id"
[SEND] Set_AutoSend
[RECV] Set_AutoSend | Set_AutoSend-#OK
```

## 主页面切换程序

```bash
[SEND] SW_Prog
[RECV] SW_Prog | SW_Prog-#OK
```

## 开机连接时

``bash
[SEND] SW_Prog
[SEND] Connect
[RECV] Connect | Connect-#OK
[SEND] Set_GlobalImgFmt
[RECV] Set_GlobalImgFmt | Set_GlobalImgFmt-#OK
[SEND] Get_ProgList
[RECV] Get_ProgList | Get_ProgList-#{"count":32,"list":[{"id":0,"name":"prog_0","work_mode":"standard"},{"id":1,"name":"prog_1","work_mode":"standard"},{"id":2,"name":"prog_2","work_mode":"standard"},{"id":3,"name":"prog_3
[SEND] Get_ProgID
[RECV] Get_ProgID | Get_ProgID-#{"id":1}
[SEND] Get_ProgInfo
[RECV] Get_ProgInfo | Get_ProgInfo-#{"tool_count":2,"has_thumb":0,"tool_list":[{"id":"T00","type":"POS_ADJUST_TOOL"},{"id":"T01","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}
[SEND] Get_MasterImg
[SEND] Get_SnapCfg
[RECV] Get_SnapCfg | Get_SnapCfg-#{"base":{"exp":2200,"gain":1,"light_mode":1,"light_type":1},"focus":{"pos":410},"ai":{"enable":0,"brightness_auto":0,"focus_auto":0},"preprocess":{"trapezoidal":0,"trape_hor":0,"trape_ver
[SEND] Get_TrigParam
[RECV] Get_TrigParam | Get_TrigParam-#{"mode":0,"inner_delay":0,"ext_delay":0,"enable":0}
[SEND] Get_IOConfig
[RECV] Get_IOConfig | Get_IOConfig-#{"inputs":[{"port":1,"func":"Trigger","mode":"Edge","polarity":"Rising"},{"port":2,"func":"Off","mode":"Edge","polarity":"Rising"}],"outputs":[{"port":1,"logic":"NO","mode":"Pulse","puls
```

## 追加字符学习的功能


```bash
[2026-04-24T16:26:42.951] [INFO ] [SEND] ATool_SetParam
[2026-04-24T16:26:42.999] [INFO ] [RECV] ATool_SetParam | ATool_SetParam-#OK
[2026-04-24T16:26:43.050] [INFO ] [SEND] ATool_SetDepend
[2026-04-24T16:26:43.056] [INFO ] [RECV] ATool_SetDepend | ATool_SetDepend-#OK
[2026-04-24T16:26:43.108] [INFO ] [SEND] ATool_StartLearn
[2026-04-24T16:26:43.113] [INFO ] [RECV] ATool_StartLearn | ATool_StartLearn-#OK
[2026-04-24T16:26:43.172] [INFO ] [SEND] ATool_AddSampleFromMaster
[2026-04-24T16:26:43.178] [INFO ] [RECV] ATool_AddSampleFromMaster | ATool_AddSampleFromMaster-#OK
[2026-04-24T16:26:43.230] [INFO ] [SEND] ATool_AddCtrlMsg
[2026-04-24T16:26:43.237] [INFO ] [RECV] ATool_AddCtrlMsg | ATool_AddCtrlMsg-#OK
[2026-04-24T16:26:43.290] [INFO ] [SEND] ATool_DoLearning
[2026-04-24T16:26:43.297] [INFO ] [RECV] ATool_DoLearning | ATool_DoLearning-#OK
[2026-04-24T16:26:46.328] [INFO ] [SEND] ATool_LearnFinish
[2026-04-24T16:26:46.334] [INFO ] [RECV] ATool_LearnFinish | ATool_LearnFinish-#OK
[2026-04-24T16:26:46.344] [INFO ] [SEND] Trigger_Read
[2026-04-24T16:26:46.392] [INFO ] [RECV] Trigger_Read | Trigger_Read-#OK
[2026-04-24T16:26:46.428] [INFO ] [RECV] Report_Result | {"res_id":4,"tot_res":{"status":-1,"vi_time":1,"vi_time_avg":1,"vi_time_max":1,"vi_time_min":0,"cycle_time":42,"cycle_time_avg":358,"cycle_time_max":961,"cycle_time_min":0},"atools":[{"node_id":"T00",

```
