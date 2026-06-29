---
id: p3qiyhdqcp0qmumdq0aj8rw
title: Test_msg
desc: ''
updated: 1782441987547
created: 1782440344444
---

## 创建工具进入设定-指令流程

origin

```bash
[2026-06-26T10:31:26.190] [DEBUG] [TCP] TX: Get_ProgID
[2026-06-26T10:31:26.192] [DEBUG] [TCP] RX: Get_ProgID-#{"id":1}
[2026-06-26T10:31:26.246] [INFO ] [SEND] Check_CRC
[2026-06-26T10:31:26.250] [DEBUG] [TCP] RX: Check_CRC-#{"remain_samples": 18, "status": "OK"}
[2026-06-26T10:31:26.302] [DEBUG] [TCP] TX: ATool_Add-#{"type":"DIAM_TOOL"}
[2026-06-26T10:31:26.306] [DEBUG] [TCP] RX: ATool_Add-#{"id":"T00"}

[2026-06-26T10:31:26.360] [DEBUG] [TCP] TX: Get_ProgInfo-#{"id":1}
[2026-06-26T10:31:26.363] [DEBUG] [TCP] RX: Get_ProgInfo-#{"tool_count":1,"has_thumb":1,"tool_list":[{"id":"T00","type":"DIAM_TOOL"}],"master_count":1,"master_list":[{"id":0}]}
[2026-06-26T10:31:26.415] [DEBUG] [TCP] TX: ATool_GetAlias-#{"id":"T00"}
[2026-06-26T10:31:26.417] [DEBUG] [TCP] RX: ATool_GetAlias-#{"node_name":""}
[2026-06-26T10:31:26.471] [DEBUG] [TCP] TX: Get_AtoolUserParam-#{"id":"T00"}
[2026-06-26T10:31:26.473] [DEBUG] [TCP] RX: Get_AtoolUserParam-#{"action":0,"select_mode":0,"edge_direction":0,"extract_sensitivity":50,"reference_circle":{"x":640,"y":480,"r":100,"inner_r":0},"regions":[],"scale_ratio":1,"enable_upper_limit":0,"lower_limit":0,"upper_limit":0}

[2026-06-26T10:31:26.529] [DEBUG] [TCP] TX: ATool_SetDepend-#{"depend_nodes":[],"id":"T00"}
[2026-06-26T10:31:26.531] [DEBUG] [TCP] RX: ATool_SetDepend-#OK
[2026-06-26T10:31:26.586] [DEBUG] [TCP] TX: ATool_GetAlias-#{"id":"T00"}
[2026-06-26T10:31:26.587] [DEBUG] [TCP] RX: ATool_GetAlias-#{"node_name":""}

[2026-06-26T10:31:26.642] [DEBUG] [TCP] TX: Set_ViSrc-#{"format":1,"src":1}
[2026-06-26T10:31:26.643] [DEBUG] [TCP] RX: Set_ViSrc-#OK

[2026-06-26T10:31:26.697] [DEBUG] [TCP] TX: Import_Img-#{"color_space":1,"height":960,"size":1228800,"width":1280}-#[binary: 1228801 bytes]
[2026-06-26T10:31:26.807] [DEBUG] [TCP] RX: Import_Img-#{"status":"OK","target_cmd":"Set_ViSrc"}

```

simplify

```bash
TX: Get_ProgID

```

## 工具已创建-进入工具设定-指令流程

```bash
[2026-06-26T10:42:39.544] [DEBUG] [TCP] TX: Get_AtoolUserParam-#{"id":"T00"}
[2026-06-26T10:42:39.545] [DEBUG] [TCP] RX: Get_AtoolUserParam-#{"action":0,"select_mode":0,"edge_direction":0,"extract_sensitivity":50,"reference_circle":{"x":640,"y":480,"r":100,"inner_r":0},"regions":[],"scale_ratio":1,"enable_upper_limit":0,"lower_limit":0,"upper_limit":0}
[2026-06-26T10:42:39.598] [DEBUG] [TCP] TX: ATool_GetAlias-#{"id":"T00"}
[2026-06-26T10:42:39.601] [DEBUG] [TCP] RX: ATool_GetAlias-#{"node_name":""}
```

## 设定阶段-拖动roi-指令流程

```bash

```

## 多工具情况