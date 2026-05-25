---
id: sfvt0yczhwfrqlf3dh9qify
title: Json_message
desc: ''
updated: 1779173099795
created: 1778466733929
---

## 参数交互-工具设定阶段（拖动roi框，修改参数点击确定（包括子面板））

### 上位机->下位机 `ATool_SetParam`

```json
ATool_SetParam-#{
    "config": {
        "confidence": 80,
        "date_format": {
            "date_type": 0
        },
        "dictionary": 0,
        "escape_character": 1,
        "high_speed_mode": 0,
        "ignore_separator": {
            "enable": 0,
            "symbols": ""
        },
        "judgment_criteria": {
            "calendar_synchronization": {
                "adjust_1_month_prior": 0,
                "calc_order": 0,
                "day_offset": 0,
                "minute_offset": 0,
                "month_comp_setting": 0,
                "month_offset": 0,
                "sync_enable": 0,
                "tolerance_mins": 0,
                "zero_padding_enable": 0
            },
            "control_character": "2"
        },
        "ocr_tool_name": "",
        "ocrmode": "date",
        "pos_adjust_ref": "",
        "regions": [
            {
                "op": "base",
                "pts": [
                    {
                        "x": 468.656,
                        "y": 454.535
                    },
                    {
                        "x": 871.61,
                        "y": 454.535
                    },
                    {
                        "x": 871.61,
                        "y": 606.913
                    },
                    {
                        "x": 468.656,
                        "y": 606.913
                    }
                ],
                "shape": "rect"
            }
        ],
        "search_char_enable": 0,
        "search_char_range": 0,
        "search_char_range_enable": 0
    },
    "id": "T00"
}
```

### 下位机->上位机（`ATool_SetParam`ack）

```json
ATool_SetParam-#OK
```

### 上位机->下位机 `ATool_SetDepend`

```json
ATool_SetDepend-#{"depend_nodes":[],"id":"T00"}
```

### 下位机->上位机 `ATool_SetDepend` ack

```json
ATool_SetDepend-#OK
```

### 上位机->下位机 `Trigger_Read`

```json
Trigger_Read
```

### 下位机->上位机 `Trigger_Read` ack

```json
Trigger_Read-#OK
```

### 下位机->上位机 `Report_Result`

```json
Report_Result-#{
    "res_id": 32,
    "tot_res": {
        "status": -1,
        "vi_time": 0,
        "vi_time_avg": 28,
        "vi_time_max": 58,
        "vi_time_min": 0,
        "cycle_time": 201,
        "cycle_time_avg": 73,
        "cycle_time_max": 1326,
        "cycle_time_min": 0
    },
    "atools": [
        {
            "node_id": "T00",
            "node_type": "OCR_TOOL",
            "status": -1,
            "score": 0,
            "num_result": 10,
            "shape": "rect",
            "matched_region": [
                {
                    "region_score": 0,
                    "region": {
                        "op": "base",
                        "shape": "rect",
                        "pts": [
                            {
                                "x": 468.656005859375,
                                "y": 454.53500366210938
                            },
                            {
                                "x": 871.6099853515625,
                                "y": 454.53500366210938
                            },
                            {
                                "x": 871.6099853515625,
                                "y": 606.91302490234375
                            },
                            {
                                "x": 468.656005859375,
                                "y": 606.91302490234375
                            }
                        ]
                    }
                }
            ],
            "assistive_region": [],
            "num_result": 10,
            "ocr_results": [
                {
                    "result_string": "2",
                    "pts": [
                        {
                            "x": 478.60714721679688,
                            "y": 503.44949340820312
                        },
                        {
                            "x": 510.72259521484375,
                            "y": 503.44949340820312
                        },
                        {
                            "x": 510.72259521484375,
                            "y": 555.2147216796875
                        },
                        {
                            "x": 478.60714721679688,
                            "y": 555.2147216796875
                        }
                    ]
                },
                {
                    "result_string": "0",
                    "pts": [
                        {
                            "x": 514.548583984375,
                            "y": 503.12542724609375
                        },
                        {
                            "x": 541.99151611328125,
                            "y": 503.12542724609375
                        },
                        {
                            "x": 541.99151611328125,
                            "y": 555.7974853515625
                        },
                        {
                            "x": 514.548583984375,
                            "y": 555.7974853515625
                        }
                    ]
                },
                {
                    "result_string": "2",
                    "pts": [
                        {
                            "x": 548.218994140625,
                            "y": 502.8973388671875
                        },
                        {
                            "x": 575.66192626953125,
                            "y": 502.8973388671875
                        },
                        {
                            "x": 575.66192626953125,
                            "y": 556.5966796875
                        },
                        {
                            "x": 548.218994140625,
                            "y": 556.5966796875
                        }
                    ]
                },
                {
                    "result_string": "5",
                    "pts": [
                        {
                            "x": 581.67449951171875,
                            "y": 504.18438720703125
                        },
                        {
                            "x": 609.63751220703125,
                            "y": 504.18438720703125
                        },
                        {
                            "x": 609.63751220703125,
                            "y": 559.24896240234375
                        },
                        {
                            "x": 581.67449951171875,
                            "y": 559.24896240234375
                        }
                    ]
                },
                {
                    "result_string": "年",
                    "pts": [
                        {
                            "x": 614.42938232421875,
                            "y": 501.9171142578125
                        },
                        {
                            "x": 646.80035400390625,
                            "y": 501.9171142578125
                        },
                        {
                            "x": 646.80035400390625,
                            "y": 558.03411865234375
                        },
                        {
                            "x": 614.42938232421875,
                            "y": 558.03411865234375
                        }
                    ]
                },
                {
                    "result_string": "1",
                    "pts": [
                        {
                            "x": 656.874267578125,
                            "y": 502.24163818359375
                        },
                        {
                            "x": 673.06842041015625,
                            "y": 502.24163818359375
                        },
                        {
                            "x": 673.06842041015625,
                            "y": 555.65118408203125
                        },
                        {
                            "x": 656.874267578125,
                            "y": 555.65118408203125
                        }
                    ]
                },
                {
                    "result_string": "2",
                    "pts": [
                        {
                            "x": 682.3138427734375,
                            "y": 500.33444213867188
                        },
                        {
                            "x": 710.16265869140625,
                            "y": 500.33444213867188
                        },
                        {
                            "x": 710.16265869140625,
                            "y": 555.4119873046875
                        },
                        {
                            "x": 682.3138427734375,
                            "y": 555.4119873046875
                        }
                    ]
                },
                {
                    "result_string": "月",
                    "pts": [
                        {
                            "x": 716.5513916015625,
                            "y": 502.06790161132812
                        },
                        {
                            "x": 744.400146484375,
                            "y": 502.06790161132812
                        },
                        {
                            "x": 744.400146484375,
                            "y": 555.7672119140625
                        },
                        {
                            "x": 716.5513916015625,
                            "y": 555.7672119140625
                        }
                    ]
                },
                {
                    "result_string": "0",
                    "pts": [
                        {
                            "x": 749.8892822265625,
                            "y": 500.71084594726562
                        },
                        {
                            "x": 776.79022216796875,
                            "y": 500.71084594726562
                        },
                        {
                            "x": 776.79022216796875,
                            "y": 554.41015625
                        },
                        {
                            "x": 749.8892822265625,
                            "y": 554.41015625
                        }
                    ]
                },
                {
                    "result_string": "1",
                    "pts": [
                        {
                            "x": 788.5694580078125,
                            "y": 501.67019653320312
                        },
                        {
                            "x": 806.84039306640625,
                            "y": 501.67019653320312
                        },
                        {
                            "x": 806.84039306640625,
                            "y": 554.34222412109375
                        },
                        {
                            "x": 788.5694580078125,
                            "y": 554.34222412109375
                        }
                    ]
                }
            ],
            "cost_time": 198,
            "cost_time_avg": 76,
            "cost_time_max": 199,
            "cost_time_min": 0
        }
    ]
}
```

### 其他-工具设定确认后跳转到输入输出设定时


```bash
Get_ProgInfo-#{"id":1}
Get_ProgInfo-#{"tool_count":1,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}

ATool_GetAlias-#{"id":"T00"}
ATool_GetAlias-#{"node_name":""}

Get_AtoolUserParam-#{"id":"T00"}
Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":468.656005859375,"y":454.53500366210938},{"x":871.6099853515625,"y":454.53500366210938},{"x":871.6099853515625,"y":606.91302490234375},{"x":468.656005859375,"y":606.91302490234375}]}],"date_format":{"date_type":0},"Judgment_criteria":{"Control_character":"2","calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0}},"Search_char_enable":0,"Search_char_range_enable":0,"Search_char_range":0,"High_speed_mode":0,"Escape_character":1,"Dictionary":0,"num_ocr_schemes":0,"ignore_separator":{"enable":0,"symbols":""}}

Set_ViSrc-#{"format":0,"src":0}
Set_ViSrc-#OK

ATool_GetAlias-#{"id":"T00"}
ATool_GetAlias-#{"node_name":""}

Get_AtoolUserParam-#{"id":"L00"}
Get_AtoolUserParam-#{"combine_mode":"and","inputs":[]}

Get_AtoolUserParam-#{"id":"L01"}
Get_AtoolUserParam-#{"combine_mode":"and","inputs":[]}

#重复至 L07

Get_OutputAssign-#{"id":1}
Get_OutputAssign-#{"output_assign":[{"port":1,"source":"Total_OK"},{"port":2,"source":"Off"},{"port":3,"source":"Off"}]}

Set_OutputAssign-#{"id":1,"port":1,"source":"Total_OK"}
Set_OutputAssign-#OK


Get_AtoolUserParam-#{"id":"TR"}
Get_AtoolUserParam-#{"judge_src_logic":"all_ok"}
```

### 设定点击确认以后-退回到运行主界面

```json

Save_Config
Save_Config-#OK

Get_ProgList-#{"count":32,"list":[{"id":0,"name":"prog_0","work_mode":"standard"},{"id":1,"name":"prog_1","work_mode":"standard"},{"id":2,"name":"prog_2","work_mode":"none"},{"id":3,"name":"prog_3","work_mode":"none"},{"id":4,"name":"prog_4","work_mode":"none"},{"id":5,"name":"prog_5","work_mode":"none"},{"id":6,"name":"prog_6","work_mode":"none"},{"id":7,"name":"prog_7","work_mode":"none"},{"id":8,"name":"prog_8","work_mode":"none"},{"id":9,"name":"prog_9","work_mode":"none"},{"id":10,"name":"prog_10","work_mode":"none"},{"id":11,"name":"prog_11","work_mode":"none"},{"id":12,"name":"prog_12","work_mode":"none"},{"id":13,"name":"prog_13","work_mode":"none"},{"id":14,"name":"prog_14","work_mode":"none"},{"id":15,"name":"prog_15","work_mode":"none"},{"id":16,"name":"prog_16","work_mode":"none"},{"id":17,"name":"prog_17","work_mode":"none"},{"id":18,"name":"prog_18","work_mode":"none"},{"id":19,"name":"prog_19","work_mode":"none"},{"id":20,"name":"prog_20","work_mode":"none"},{"id":21,"name":"prog_21","work_mode":"none"},{"id":22,"name":"prog_22","work_mode":"none"},{"id":23,"name":"prog_23","work_mode":"none"},{"id":24,"name":"prog_24","work_mode":"none"},{"id":25,"name":"prog_25","work_mode":"none"},{"id":26,"name":"prog_26","work_mode":"none"},{"id":27,"name":"prog_27","work_mode":"none"},{"id":28,"name":"prog_28","work_mode":"none"},{"id":29,"name":"prog_29","work_mode":"none"},{"id":30,"name":"prog_30","work_mode":"none"},{"id":31,"name":"prog_31","work_mode":"none"}]}

```

## 运行阶段 开始运行-停止运行

### 点击运行

```json
Get_ProgInfo-#{"id":1}
Get_ProgInfo-#{"tool_count":1,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}

Get_AtoolUserParam-#{"id":"T00"}
Get_AtoolUserParam-#{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":468.656005859375,"y":454.53500366210938},{"x":871.6099853515625,"y":454.53500366210938},{"x":871.6099853515625,"y":606.91302490234375},{"x":468.656005859375,"y":606.91302490234375}]}],"date_format":{"date_type":0},"Judgment_criteria":{"Control_character":"2","calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0}},"Search_char_enable":0,"Search_char_range_enable":0,"Search_char_range":0,"High_speed_mode":0,"Escape_character":1,"Dictionary":0,"num_ocr_schemes":0,"ignore_separator":{"enable":0,"symbols":""}}

Set_AutoSend-#{"enable":1}
Set_AutoSend-#OK

Run
Run-#OK

//接收
Report_Result-#{"res_id":33,"tot_res":{"status":-1,"vi_time":11,"vi_time_avg":28,"vi_time_max":58,"vi_time_min":0,"cycle_time":50,"cycle_time_avg":73,"cycle_time_max":1326,"cycle_time_min":0},"atools":[{"node_id":"T00","node_type":"OCR_TOOL","status":-1,"score":0,"num_result":12,"shape":"rect","matched_region":[{"region_score":0,"region":{"op":"base","shape":"rect","pts":[{"x":468.656005859375,"y":454.53500366210938},{"x":871.6099853515625,"y":454.53500366210938},{"x":871.6099853515625,"y":606.91302490234375},{"x":468.656005859375,"y":606.91302490234375}]}}],"assistive_region":[],"num_result":12,"ocr_results":[{"result_string":"3","pts":[{"x":815.19775390625,"y":462.65463256835938},{"x":843.89984130859375,"y":462.65463256835938},{"x":843.89984130859375,"y":503.33773803710938},{"x":815.19775390625,"y":503.33773803710938}]},{"result_string":"6","pts":[{"x":845.64794921875,"y":463.715576171875},{"x":873.3935546875,"y":463.715576171875},{"x":873.3935546875,"y":504.398681640625},{"x":845.64794921875,"y":504.398681640625}]},{"result_string":"2","pts":[{"x":466.5899658203125,"y":558.3875732421875},{"x":495.29202270507812,"y":558.3875732421875},{"x":495.29202270507812,"y":609.1793212890625},{"x":466.5899658203125,"y":609.1793212890625}]},{"result_string":"0","pts":[{"x":499.63168334960938,"y":558.7158203125},{"x":525.869384765625,"y":558.7158203125},{"x":525.869384765625,"y":608.491455078125},{"x":499.63168334960938,"y":608.491455078125}]},{"result_string":"2","pts":[{"x":533.24993896484375,"y":559.300537109375},{"x":560.4476318359375,"y":559.300537109375},{"x":560.4476318359375,"y":610.09228515625},{"x":533.24993896484375,"y":610.09228515625}]},{"result_string":"5","pts":[{"x":567.2674560546875,"y":561.92242431640625},{"x":593.50518798828125,"y":561.92242431640625},{"x":593.50518798828125,"y":610.35443115234375},{"x":567.2674560546875,"y":610.35443115234375}]},{"result_string":"年","pts":[{"x":598.4334716796875,"y":560.0960693359375},{"x":632.7208251953125,"y":560.0960693359375},{"x":632.7208251953125,"y":613.20281982421875},{"x":598.4334716796875,"y":613.20281982421875}]},{"result_string":"1","pts":[{"x":641.39166259765625,"y":561.2691650390625},{"x":658.60626220703125,"y":561.2691650390625},{"x":658.60626220703125,"y":609.701171875},{"x":641.39166259765625,"y":609.701171875}]},{"result_string":"2","pts":[{"x":668.03509521484375,"y":561.59564208984375},{"x":695.7806396484375,"y":561.59564208984375},{"x":695.7806396484375,"y":610.02764892578125},{"x":668.03509521484375,"y":610.02764892578125}]},{"result_string":"月","pts":[{"x":703.82025146484375,"y":563.176513671875},{"x":731.0179443359375,"y":563.176513671875},{"x":731.0179443359375,"y":611.60858154296875},{"x":703.82025146484375,"y":611.60858154296875}]},{"result_string":"0","pts":[{"x":736.23291015625,"y":563.36041259765625},{"x":763.978515625,"y":563.36041259765625},{"x":763.978515625,"y":611.79241943359375},{"x":736.23291015625,"y":611.79241943359375}]},{"result_string":"1","pts":[{"x":775.107177734375,"y":563.89788818359375},{"x":793.57965087890625,"y":563.89788818359375},{"x":793.57965087890625,"y":609.0321044921875},{"x":775.107177734375,"y":609.0321044921875}]}],"cost_time":35,"cost_time_avg":74,"cost_time_max":199,"cost_time_min":0}]}

// 持续接收Report_Result

Stop
Stop-#OK

Set_AutoSend-#{"enable":0}
Set_AutoSend-#OK
```

## 追加字符学习-ai匹配功能

### 追加2个字符

```json
ATool_SetParam-#{"config":{"confidence":80,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"adjust_1_month_prior":0,"calc_order":0,"day_offset":0,"minute_offset":0,"month_comp_setting":0,"month_offset":0,"sync_enable":0,"tolerance_mins":0,"zero_padding_enable":0},"control_character":"2"},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","pts":[{"x":468.656,"y":454.535},{"x":871.61,"y":454.535},{"x":871.61,"y":606.913},{"x":468.656,"y":606.913}],"shape":"rect"}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0},"id":"T00"}
ATool_SetParam-#OK

ATool_SetDepend-#{"depend_nodes":[],"id":"T00"}
ATool_SetDepend-#OK

ATool_StartLearn-#{"id":"T00"}
ATool_StartLearn-#OK

ATool_AddSampleFromMaster-#{"id":"T00","index":0}
ATool_AddSampleFromMaster-#OK

// 有可能是上位机内部日志，Sending learning samples: [{"ch_r":"1","sa_id":0,"sa_pts":[{"x":771.82,"y":494.83},{"x":821.82,"y":494.83},{"x":821.82,"y":574.83},{"x":771.82,"y":574.83}]},{"ch_r":"2","sa_id":1,"sa_pts":[{"x":474.99,"y":492.94},{"x":518.05,"y":492.94},{"x":518.05,"y":567.21},{"x":474.99,"y":567.21}]}]

ATool_AddCtrlMsg-#{"id":"T00","sample_id":"0","samples":"[{\"ch_r\":\"1\",\"sa_id\":0,\"sa_pts\":[{\"x\":771.82,\"y\":494.83},{\"x\":821.82,\"y\":494.83},{\"x\":821.82,\"y\":574.83},{\"x\":771.82,\"y\":574.83}]},{\"ch_r\":\"2\",\"sa_id\":1,\"sa_pts\":[{\"x\":474.99,\"y\":492.94},{\"x\":518.05,\"y\":492.94},{\"x\":518.05,\"y\":567.21},{\"x\":474.99,\"y\":567.21}]}]"}
ATool_AddCtrlMsg-#OK

ATool_DoLearning-#{"id":"T00"}
ATool_DoLearning-#OK

ATool_LearnFinish-#{"id":"T00"}
ATool_LearnFinish-#OK

Trigger_Read
Trigger_Read-#OK

Report_Result-#{"res_id":38,"tot_res":{"status":-1,"vi_time":1,"vi_time_avg":28,"vi_time_max":58,"vi_time_min":0,"cycle_time":205,"cycle_time_avg":73,"cycle_time_max":1326,"cycle_time_min":0},"atools":[{"node_id":"T00","node_type":"OCR_TOOL","status":-1,"score":0,"num_result":10,"shape":"rect","matched_region":[{"region_score":0,"region":{"op":"base","shape":"rect","pts":[{"x":468.656005859375,"y":454.53500366210938},{"x":871.6099853515625,"y":454.53500366210938},{"x":871.6099853515625,"y":606.91302490234375},{"x":468.656005859375,"y":606.91302490234375}]}}],"assistive_region":[],"num_result":10,"ocr_results":[{"result_string":"2","pts":[{"x":478.60714721679688,"y":503.44949340820312},{"x":510.72259521484375,"y":503.44949340820312},{"x":510.72259521484375,"y":555.2147216796875},{"x":478.60714721679688,"y":555.2147216796875}]},{"result_string":"0","pts":[{"x":514.548583984375,"y":503.12542724609375},{"x":541.99151611328125,"y":503.12542724609375},{"x":541.99151611328125,"y":555.7974853515625},{"x":514.548583984375,"y":555.7974853515625}]},{"result_string":"2","pts":[{"x":548.218994140625,"y":502.8973388671875},{"x":575.66192626953125,"y":502.8973388671875},{"x":575.66192626953125,"y":556.5966796875},{"x":548.218994140625,"y":556.5966796875}]},{"result_string":"5","pts":[{"x":581.67449951171875,"y":504.18438720703125},{"x":609.63751220703125,"y":504.18438720703125},{"x":609.63751220703125,"y":559.24896240234375},{"x":581.67449951171875,"y":559.24896240234375}]},{"result_string":"年","pts":[{"x":614.42938232421875,"y":501.9171142578125},{"x":646.80035400390625,"y":501.9171142578125},{"x":646.80035400390625,"y":558.03411865234375},{"x":614.42938232421875,"y":558.03411865234375}]},{"result_string":"1","pts":[{"x":656.874267578125,"y":502.24163818359375},{"x":673.06842041015625,"y":502.24163818359375},{"x":673.06842041015625,"y":555.65118408203125},{"x":656.874267578125,"y":555.65118408203125}]},{"result_string":"2","pts":[{"x":682.3138427734375,"y":500.33444213867188},{"x":710.16265869140625,"y":500.33444213867188},{"x":710.16265869140625,"y":555.4119873046875},{"x":682.3138427734375,"y":555.4119873046875}]},{"result_string":"月","pts":[{"x":716.5513916015625,"y":502.06790161132812},{"x":744.400146484375,"y":502.06790161132812},{"x":744.400146484375,"y":555.7672119140625},{"x":716.5513916015625,"y":555.7672119140625}]},{"result_string":"0","pts":[{"x":749.8892822265625,"y":500.71084594726562},{"x":776.79022216796875,"y":500.71084594726562},{"x":776.79022216796875,"y":554.41015625},{"x":749.8892822265625,"y":554.41015625}]},{"result_string":"1","pts":[{"x":788.5694580078125,"y":501.67019653320312},{"x":806.84039306640625,"y":501.67019653320312},{"x":806.84039306640625,"y":554.34222412109375},{"x":788.5694580078125,"y":554.34222412109375}]}],"cost_time":201,"cost_time_avg":74,"cost_time_max":201,"cost_time_min":0}]}

```

### 删除一个字符后重新学习

```bash
重复学习流程
```

## 设定阶段更新主控图

```bash
Reg_MasterImg
Reg_MasterImg-#{"index":0}

Import_Img-#{"color_space":1,"height":960,"size":1228800,"width":1280}-#[binary: 1228801 bytes]
Import_Img-#{"status":"OK","target_cmd":"Reg_MasterImg"}

Get_ProgInfo-#{"id":1}
Get_ProgInfo-#{"tool_count":1,"has_thumb":1,"tool_list":[{"id":"T00","type":"OCR_TOOL"}],"master_count":1,"master_list":[{"id":0}]}

Get_MasterImg-#{"index":0}
[NET] Binary image complete: Get_MasterImg | binarySize=1228800, w=1280, h=960, color_space=1, format=1



```

## 查询学习信息

```json
{
  "master_ctl_ids": [
    "0"
  ],
  "rt_ext_ids": [
    "P000_T00_L_1",
    "P000_T00_L_2"
  ],
  "is_learned": 1,
  "schemes": [
    {
      "sc_id": "0",
      "is_master": 1,
      "num_ocr_samples": 3,
      "samples": [
        {
          "sa_id": 0,
          "sa_pts": [
            { "x": 954.260009765625, "y": 428.2099914550781 },
            { "x": 1030.4300537109375, "y": 428.2099914550781 },
            { "x": 1030.4300537109375, "y": 542.6300048828125 },
            { "x": 954.260009765625, "y": 542.6300048828125 }
          ],
          "ch_r": "8"
        },
        {
          "sa_id": 1,
          "sa_pts": [
            { "x": 644.1900024414062, "y": 443.3299865722656 },
            { "x": 716.5700073242188, "y": 443.3299865722656 },
            { "x": 716.5700073242188, "y": 557.75 },
            { "x": 644.1900024414062, "y": 557.75 }
          ],
          "ch_r": "8"
        },
        {
          "sa_id": 2,
          "sa_pts": [
            { "x": 334.4800109863281, "y": 458.4599914550781 },
            { "x": 398.94000244140625, "y": 458.4599914550781 },
            { "x": 398.94000244140625, "y": 567.2100219726562 },
            { "x": 334.4800109863281, "y": 567.2100219726562 }
          ],
          "ch_r": "8"
        }
      ]
    },
    {
      "sc_id": "P000_T00_L_1",
      "is_master": 0,
      "num_ocr_samples": 1,
      "samples": [
        {
          "sa_id": 0,
          "sa_pts": [
            { "x": 959.9299926757812, "y": 428.2099914550781 },
            { "x": 1032.3199462890625, "y": 428.2099914550781 },
            { "x": 1032.3199462890625, "y": 546.4099731445312 },
            { "x": 959.9299926757812, "y": 546.4099731445312 }
          ],
          "ch_r": "0"
        }
      ]
    },
    {
      "sc_id": "P000_T00_L_2",
      "is_master": 0,
      "num_ocr_samples": 1,
      "samples": [
        {
          "sa_id": 0,
          "sa_pts": [
            { "x": 647.969970703125, "y": 439.54998779296875 },
            { "x": 720.3499755859375, "y": 439.54998779296875 },
            { "x": 720.3499755859375, "y": 557.75 },
            { "x": 647.969970703125, "y": 557.75 }
          ],
          "ch_r": "0"
        }
      ]
    }
  ]
}
```
