---
id: hx7uktmbkw5e7w7zpdiyq10
title: 报文留档
desc: ''
updated: 1781012414690
created: 1780987309286
---

### apply_user_param

```bash
//差异点仅在float数值精度
<1>[RAW  IN] : {"confidence":80,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"adjust_1_month_prior":0,"calc_order":0,"day_offset":0,"minute_offset":0,"month_comp_setting":0,"month_offset":0,"sync_enable":0,"tolerance_mins":0,"zero_padding_enable":0},"control_character":""},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","pts":[{"x":355.214,"y":299.498},{"x":675.214,"y":299.498},{"x":675.214,"y":539.498},{"x":355.214,"y":539.498}],"shape":"rect"}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0}

<2>[DTO OUT] : {"confidence":80,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0},"control_character":""},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":355.2139892578125,"y":299.49798583984375},{"x":675.2139892578125,"y":299.49798583984375},{"x":675.2139892578125,"y":539.49798583984375},{"x":355.2139892578125,"y":539.49798583984375}]}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0}
```

### export_user_param_to_json

```bash
//刚上电时，配置来源是持久化的json内容反序列化，说明config的序列化和反序列化异常
<3>export_user_param_to_json: {"confidence":80,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0},"control_character":""},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0}

//经过上位机配置下发后，配置来源是上位机，内容于<2>完全匹配
<4>export_user_param_to_json: {"confidence":80,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0},"control_character":""},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":355.2139892578125,"y":299.49798583984375},{"x":675.2139892578125,"y":299.49798583984375},{"x":675.2139892578125,"y":539.49798583984375},{"x":355.2139892578125,"y":539.49798583984375}]}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0}
```

### apply_config

```bash
加载配置后读取到的原始json {"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]}],"date_format":{"date_type":0},"Judgment_criteria":{"Control_character":"05Y1月01日","calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0}},"Search_char_enable":0,"Search_char_range_enable":0,"Search_char_range":0,"High_speed_mode":0,"Escape_character":1,"Dictionary":0,"num_ocr_schemes":2,"ignore_separator":{"enable":0,"symbols":""},"schemes":[{"sc_id":"P001_T00_L_1","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"pts":[{"x":635.67999267578125,"y":359.23001098632812},{"x":674.97998046875,"y":359.23001098632812},{"x":674.97998046875,"y":418.64999389648438},{"x":635.67999267578125,"y":418.64999389648438}],"ch_e":"","ch_r":"日","sa_type":0}]},{"sc_id":"P001_T00_L_2","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"pts":[{"x":323.70999145507812,"y":381.8800048828125},{"x":361.1199951171875,"y":381.8800048828125},{"x":361.1199951171875,"y":438.6400146484375},{"x":323.70999145507812,"y":438.6400146484375}],"ch_e":"","ch_r":"2","sa_type":1}]},{"sc_id":"P001_T00_L_3","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"pts":[{"x":490.10000610351562,"y":363.10000610351562},{"x":523.719970703125,"y":363.10000610351562},{"x":523.719970703125,"y":425.41000366210938},{"x":490.10000610351562,"y":425.41000366210938}],"ch_e":"","ch_r":"Y","sa_type":0}]}]}
8

转化后的 异常！！！
{"config":{"param":{"confidence":80,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0},"control_character":""},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0},"learn_info":{"ok_count":0,"ng_count":0,"is_learned":false,"samples":[]}},"id":""}    

```

### 上电读取配置 pass

```bash
Thread 1 "xs-nvs" hit Breakpoint 1, OcrEngine::apply_config_from_json (this=0x7feeadc9d0, json_obj_in=0x7ff7110310)
    at /home/jfxu/nv/app/nvs/src/atools/ocr_tool/priv/ocr_engine.cpp:60
warning: 60     /home/jfxu/nv/app/nvs/src/atools/ocr_tool/priv/ocr_engine.cpp: No such file or directory
(gdb) c
Continuing.
2026-06-09 21:29:08.572 INFO  [ndm_mod] nvs_device_mgr.c:137 @nvs_device_ntp_sync_retry_thread() - NTP: Sync verified SUCCESS via date check!
[LWP 1923 exited]
2026-06-09 21:29:08.573 WARN  [ocr_mod] ocr_helper.cpp:270 @print_json_context() - decode [OcrParam_Dto] passed with [1] auto-healed alert(s)
2026-06-09 21:29:08.573 WARN  [ocr_mod] ocr_helper.cpp:272 @print_json_context() -   [alert 1: Missing field [confidence], using default]
2026-06-09 21:29:08.575 INFO  [ocr_mod] ocr_helper.cpp:290 @print_json() -  [raw_in] : [{"ocrmode":"date","ocr_tool_name":"","threshold":50,"pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]}],"date_format":{"date_type":0},"Judgment_criteria":{"Control_character":"05Y1月01日","calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0}},"Search_char_enable":0,"Search_char_range_enable":0,"Search_char_range":0,"High_speed_mode":0,"Escape_character":1,"Dictionary":0,"num_ocr_schemes":2,"ignore_separator":{"enable":0,"symbols":""},"schemes":[{"sc_id":"P001_T00_L_1","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"pts":[{"x":635.67999267578125,"y":359.23001098632812},{"x":674.97998046875,"y":359.23001098632812},{"x":674.97998046875,"y":418.64999389648438},{"x":635.67999267578125,"y":418.64999389648438}],"ch_e":"","ch_r":"日","sa_type":0}]},{"sc_id":"P001_T00_L_2","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"pts":[{"x":323.70999145507812,"y":381.8800048828125},{"x":361.1199951171875,"y":381.8800048828125},{"x":361.1199951171875,"y":438.6400146484375},{"x":323.70999145507812,"y":438.6400146484375}],"ch_e":"","ch_r":"2","sa_type":1}]},{"sc_id":"P001_T00_L_3","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"pts":[{"x":490.10000610351562,"y":363.10000610351562},{"x":523.719970703125,"y":363.10000610351562},{"x":523.719970703125,"y":425.41000366210938},{"x":490.10000610351562,"y":425.41000366210938}],"ch_e":"","ch_r":"Y","sa_type":0}]}]}]
2026-06-09 21:29:08.576 INFO  [ocr_mod] ocr_helper.cpp:290 @print_json() -  [param] : [{"confidence":80,"threshold":50,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0},"control_character":"05Y1月01日"},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0}]
2026-06-09 21:29:08.577 INFO  [ocr_mod] ocr_helper.cpp:290 @print_json() -  [schemes] : [[{"sc_id":"P001_T00_L_1","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"sa_type":0,"ch_e":"","ch_r":"日","pts":[{"x":635.67999267578125,"y":359.23001098632812},{"x":674.97998046875,"y":359.23001098632812},{"x":674.97998046875,"y":418.64999389648438},{"x":635.67999267578125,"y":418.64999389648438}]}]},{"sc_id":"P001_T00_L_2","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"sa_type":1,"ch_e":"","ch_r":"2","pts":[{"x":323.70999145507812,"y":381.8800048828125},{"x":361.1199951171875,"y":381.8800048828125},{"x":361.1199951171875,"y":438.6400146484375},{"x":323.70999145507812,"y":438.6400146484375}]}]},{"sc_id":"P001_T00_L_3","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"sa_type":0,"ch_e":"","ch_r":"Y","pts":[{"x":490.10000610351562,"y":363.10000610351562},{"x":523.719970703125,"y":363.10000610351562},{"x":523.719970703125,"y":425.41000366210938},{"x":490.10000610351562,"y":425.41000366210938}]}]}]]


```


### 上电通过读取配置途径下发系统广播 pass

```bash
Thread 1 "xs-nvs" hit Breakpoint 1, OcrEngine::apply_config_from_json (this=0x7feeadc9d0, json_obj_in=0x7ff04a9070)
    at /home/jfxu/nv/app/nvs/src/atools/ocr_tool/priv/ocr_engine.cpp:60
60      in /home/jfxu/nv/app/nvs/src/atools/ocr_tool/priv/ocr_engine.cpp
(gdb)
Continuing.
2026-06-09 21:29:12.143 INFO  [ocr_mod] ocr_helper.cpp:290 @print_json() -  [contained_print] : [{"sys_opts":{"user_tcp_detailed_fmt":1}}]
2026-06-09 21:29:12.143 INFO  [ocr_mod] ocr_helper.cpp:290 @print_json() -  [raw_in] : [{"sys_opts":{"user_tcp_detailed_fmt":1}}]
2026-06-09 21:29:12.143 INFO  [ocr_mod] ocr_helper.cpp:290 @print_json() -  [sys_conf] : [{"sys_opts":{"user_tcp_detailed_fmt":1}}]



```

### 导出下位机param pass

```bash
2026-06-09 21:31:12.723 INFO  [ocr_mod] ocr_engine.cpp:169 @export_user_param_to_json() - export_user_param_to_json: {"confidence":80,"threshold":50,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0},"control_character":"05Y1月01日"},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0}


2026-06-09 21:31:15.008 INFO  [ndm_mod] nvs_device_mgr.c:3417 @process_ndm_ncm_request() - Processing NVS_NDM_NCM_REQ_GET_ATOOL_USER_PARAMS in main thread
2026-06-09 21:31:15.009 INFO  [ocr_mod] ocr_engine.cpp:169 @export_user_param_to_json() - export_user_param_to_json: {"confidence":80,"threshold":50,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0},"control_character":"05Y1月01日"},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0}


```

### 下发 userparam pass

```bash
2026-06-09 21:31:19.296 WARN  [ocr_mod] ocr_helper.cpp:272 @print_json_context() -   [alert 1: Missing field [threshold], using default]
2026-06-09 21:31:19.296 INFO  [ocr_mod] ocr_helper.cpp:284 @print_json_diagnostics() - ====> json diagnostics: apply_user_param
2026-06-09 21:31:19.296 INFO  [ocr_mod] ocr_helper.cpp:285 @print_json_diagnostics() -  [RAW  IN] : [{"confidence":80,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"adjust_1_month_prior":0,"calc_order":0,"day_offset":0,"minute_offset":0,"month_comp_setting":0,"month_offset":0,"sync_enable":0,"tolerance_mins":0,"zero_padding_enable":0},"control_character":"05Y1月01日"},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","pts":[{"x":285.259,"y":333.531},{"x":714.683,"y":333.531},{"x":714.683,"y":449.986},{"x":285.259,"y":449.986}],"shape":"rect"}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0}]
2026-06-09 21:31:19.297 INFO  [ocr_mod] ocr_helper.cpp:286 @print_json_diagnostics() -  [DTO OUT] : [{"confidence":80,"threshold":50,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0},"control_character":"05Y1月01日"},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":285.25900268554688,"y":333.531005859375},{"x":714.6829833984375,"y":333.531005859375},{"x":714.6829833984375,"y":449.98599243164062},{"x":285.25900268554688,"y":449.98599243164062}]}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0}]


```

### 下点保存配置 pass

```bash
2026-06-09 21:36:54.608 INFO  [ocr_mod] ocr_engine.cpp:129 @export_config_to_json() - export_config generated: [{"confidence":80,"threshold":50,"date_format":{"date_type":0},"dictionary":0,"escape_character":1,"high_speed_mode":0,"ignore_separator":{"enable":0,"symbols":""},"judgment_criteria":{"calendar_synchronization":{"sync_enable":0,"month_offset":0,"day_offset":0,"minute_offset":0,"tolerance_mins":0,"zero_padding_enable":0,"calc_order":0,"month_comp_setting":0,"adjust_1_month_prior":0},"control_character":"05Y1月01日"},"ocr_tool_name":"","ocrmode":"date","pos_adjust_ref":"","regions":[{"op":"base","shape":"rect","pts":[{"x":279.58700561523438,"y":335.42098999023438},{"x":709.010986328125,"y":335.42098999023438},{"x":709.010986328125,"y":451.87600708007812},{"x":279.58700561523438,"y":451.87600708007812}]}],"search_char_enable":0,"search_char_range":0,"search_char_range_enable":0,"schemes":[{"sc_id":"P001_T00_L_1","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"sa_type":0,"ch_e":"","ch_r":"日","pts":[{"x":635.67999267578125,"y":359.23001098632812},{"x":674.97998046875,"y":359.23001098632812},{"x":674.97998046875,"y":418.64999389648438},{"x":635.67999267578125,"y":418.64999389648438}]}]},{"sc_id":"P001_T00_L_2","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"sa_type":1,"ch_e":"","ch_r":"2","pts":[{"x":323.70999145507812,"y":381.8800048828125},{"x":361.1199951171875,"y":381.8800048828125},{"x":361.1199951171875,"y":438.6400146484375},{"x":323.70999145507812,"y":438.6400146484375}]}]},{"sc_id":"P001_T00_L_3","is_master":0,"num_ocr_samples":1,"region":{"shape":0,"pts":[{"x":296.60299682617188,"y":342.9840087890625},{"x":726.0269775390625,"y":342.9840087890625},{"x":726.0269775390625,"y":459.43899536132812},{"x":296.60299682617188,"y":459.43899536132812}]},"samples":[{"sa_id":0,"sa_type":0,"ch_e":"","ch_r":"Y","pts":[{"x":490.10000610351562,"y":363.10000610351562},{"x":523.719970703125,"y":363.10000610351562},{"x":523.719970703125,"y":425.41000366210938},{"x":490.10000610351562,"y":425.41000366210938}]}]}]}]

```
