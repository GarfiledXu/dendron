---
id: 69px43knnkxztait9eygjwr
title: Msg_record
desc: ''
updated: 1778220634086
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

### debug

```bash
[2026-05-08T14:09:39.127] [INFO ] [SEND] ATool_SetParam
[2026-05-08T14:09:39.134] [DEBUG] [TCP] TX: ATool_SetParam-#{"config":{"confidence":80,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"judgment_criteria":{"calendar_synchronization":{"adjust_1_month_prior":0,"calc_order":0,"day_offset":0,"minute_offset":0,"month_comp_setting":0,"month_offset":0,"sync_enable":0,"tolerance_mins":0,"zero_padding_enable":0},"control_character":"2025年12月012025年12月01"},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","pts":[{"x":591.55,"y":564.195},{"x":945.346,"y":564.195},{"x":945.346,"y":684.431},{"x":591.55,"y":684.431}],"shape":"rect"}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0},"id":"T00"}
[2026-05-08T14:09:39.135] [DEBUG] [TCP] RX: ATool_SetParam-#OK
[2026-05-08T14:09:39.136] [INFO ] [RECV] ATool_SetParam | ATool_SetParam-#OK
[2026-05-08T14:09:39.143] [DEBUG] [NET] Queued command: ATool_SetDepend (priority=Normal, queue=1, depth=1)
[2026-05-08T14:09:39.187] [INFO ] [SEND] ATool_SetDepend
[2026-05-08T14:09:39.191] [DEBUG] [TCP] TX: ATool_SetDepend-#{"depend_nodes":[],"id":"T00"}
[2026-05-08T14:09:39.191] [DEBUG] [TCP] RX: ATool_SetDepend-#OK
[2026-05-08T14:09:39.192] [INFO ] [RECV] ATool_SetDepend | ATool_SetDepend-#OK
[2026-05-08T14:09:39.195] [DEBUG] [ToolSettingsPanel] Trigger_Read after live SetParam
[2026-05-08T14:09:39.195] [DEBUG] [NET] Queued command: Trigger_Read (priority=Normal, queue=1, depth=1)
[2026-05-08T14:09:39.196] [INFO ] [SEND] Trigger_Read
[2026-05-08T14:09:39.199] [DEBUG] [TCP] TX: Trigger_Read
[2026-05-08T14:09:39.200] [DEBUG] [TCP] RX: Trigger_Read-#OK
[2026-05-08T14:09:39.200] [INFO ] [RECV] Trigger_Read | Trigger_Read-#OK
[2026-05-08T14:09:39.401] [DEBUG] [TCP][BINARY][parse] cmd=Report_ResultImg hasSepAfterJson=true binaryLen=100427 len=100427 size=-1 data_len=-1 attachments=0 json={"len":100427,"eof":1,"w":1280,"h":960,"color_space":1,"format":"jpeg"}
[2026-05-08T14:09:39.410] [DEBUG] [TCP][BINARY][start] cmd=Report_ResultImg declared=100427 actual=100427 len=100427 size=-1 data_len=-1 bufferedAfterHeader=0
[2026-05-08T14:09:39.410] [DEBUG] [TCP][BINARY][complete] cmd=Report_ResultImg total=100427
[2026-05-08T14:09:39.411] [DEBUG] [NET][BINARY][recv] cmd=Report_ResultImg inFlight=none len=100427 (len=100427 size=-1) eof=1 recvBytes=100427
[2026-05-08T14:09:39.412] [DEBUG] [NET] Async Report_ResultImg: size=100427, w=1280, h=960, color_space=1, format=2
[2026-05-08T14:09:39.412] [DEBUG] [FRAME_CACHE] receiving: interval=5715ms receiving_fps=0.2
[2026-05-08T14:09:39.415] [DEBUG] [TCP][JSON] cmd=Report_Result hasSepAfterJson=false jsonFound=true crPos=2937
[2026-05-08T14:09:39.416] [DEBUG] [TCP] RX: Report_Result-#{"res_id":374,"tot_res":{"status":-1,"vi_time":1,"vi_time_avg":12,"vi_time_max":531,"vi_time_min":0,"cycle_time":197,"cycle_time_avg":56,"cycle_time_max":1701,"cycle_time_min":0},"atools":[{"node_id":"T00","node_type":"OCR_TOOL","status":-1,"score":0,"num_result":10,"shape":"rect","matched_region":[{"region_score":0,"region":{"op":"base","shape":"rect","pts":[{"x":591.54998779296875,"y":564.19500732421875},{"x":945.34600830078125,"y":564.19500732421875},{"x":945.34600830078125,"y":684.4310302734375},{"x":591.54998779296875,"y":684.4310302734375}]}}],"assistive_region":[],"num_result":11,"ocr_results":[{"result_string":"2025年12月01","pts":[{"x":0,"y":0},{"x":0,"y":0},{"x":0,"y":0},{"x":0,"y":0}]},{"result_string":"2","pts":[{"x":613.4390869140625,"y":615.6787109375},{"x":644.43035888671875,"y":615.6787109375},{"x":644.43035888671875,"y":662.08056640625},{"x":613.4390869140625,"y":662.08056640625}]},{"result_string":"0","pts":[{"x":643.80889892578125,"y":612.25164794921875},{"x":670.45928955078125,"y":612.25164794921875},{"x":670.45928955078125,"y":658.7825927734375},{"x":643.80889892578125,"y":658.7825927734375}]},{"result_string":"2","pts":[{"x":671.47393798828125,"y":608.50885009765625},{"x":698.594482421875,"y":608.50885009765625},{"x":698.594482421875,"y":655.94732666015625},{"x":671.47393798828125,"y":655.94732666015625}]},{"result_string":"5","pts":[{"x":698.50823974609375,"y":605.94049072265625},{"x":725.2435302734375,"y":605.94049072265625},{"x":725.2435302734375,"y":653.6988525390625},{"x":698.50823974609375,"y":653.6988525390625}]},{"result_string":"年","pts":[{"x":725.09466552734375,"y":600.43182373046875},{"x":756.526123046875,"y":600.43182373046875},{"x":756.526123046875,"y":649.5625},{"x":725.09466552734375,"y":649.5625}]},{"result_string":"1","pts":[{"x":759.28143310546875,"y":597.48822021484375},{"x":778.1885986328125,"y":597.48822021484375},{"x":778.1885986328125,"y":644.9266357421875},{"x":759.28143310546875,"y":644.9266357421875}]},{"result_string":"2","pts":[{"x":780.4217529296875,"y":594.18060302734375},{"x":807.56982421875,"y":594.18060302734375},{"x":807.56982421875,"y":640.58251953125},{"x":780.4217529296875,"y":640.58251953125}]},{"result_string":"月","pts":[{"x":807.80010986328125,"y":590.65850830078125},{"x":835.3638916015625,"y":590.65850830078125},{"x":835.3638916015625,"y":637.06036376953125},{"x":807.80010986328125,"y":637.06036376953125}]},{"result_string":"0","pts":[{"x":835.174560546875,"y":587.03289794921875},{"x":861.824951171875,"y":587.03289794921875},{"x":861.824951171875,"y":634.47137451171875},{"x":835.174560546875,"y":634.47137451171875}]},{"result_string":"1","pts":[{"x":865.53936767578125,"y":583.849853515625},{"x":884.91595458984375,"y":583.849853515625},{"x":884.91595458984375,"y":631.288330078125},{"x":865.53936767578125,"y":631.288330078125}]}],"cost_time":193,"cost_time_avg":34,"cost_time_max":193,"cost_time_min":0}]}
[2026-05-08T14:09:39.418] [INFO ] [RECV] Report_Result | {"res_id":374,"tot_res":{"status":-1,"vi_time":1,"vi_time_avg":12,"vi_time_max":531,"vi_time_min":0,"cycle_time":197,"cycle_time_avg":56,"cycle_time_max":1701,"cycle_time_min":0},"atools":[{"node_id":

```
