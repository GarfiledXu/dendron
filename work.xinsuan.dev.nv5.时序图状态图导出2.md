---
id: y3ro81pgx4og72jbqktffk6
title: 时序图状态图导出2
desc: ''
updated: 1775615678208
created: 1774852125219
---
## 备份数据定义和分类

数据单元

config json:
    system-device_info
    system-time_sync
    system-protocol
    system-ftp
    system-global_behavior
    npm
    npm-progs

config img bin:
    master_img
    thumb_img
    learn_img

alg model bin:
    model

### 掉点保存的json

```json
{
    "system": {
        "device_info": {
            "alias": "NV5"
        },
        "network": {
            "ip": "192.168.10.130",
            "mask": "255.255.255.0",
            "gw": "192.168.10.1"
        },
        "time_sync": {
            "ntp_server": ""
        },
        "protocol": {
            "active": {
                "type": "NONE", 
                "enable": 0
            },
            "fieldbus_param": {
                "handshake": 0,
                "byte_swap": 0,
                "tool_count": 7
            },
            "ethernet_ip": {
                "input_size": 100,
                "output_size": 100,
                "handshake": 1
            },
            "profinet": {
                "name": "",
                "detailed_mode": "",
                "tool_count": 7,
                "control_modules": 1,
                "status_modules": 1
            }
        },
        "ftp": {
            "host": "1.2.3.4",
            "port": 21,
            "user": "admin",
            "pass": "123",
            "save_config": {
                "type": "NG",
                "enable": 1
            }
        },
        "global_behavior": {
            "prog_switch_src": 0,
            "led_mode": "running"
        }
    },

    "npm": {
        "cur_prog": 0,
        "progs": [
            {
                "prog_id": 0, 
                "mode": "standard", 
                "name": "用户随便填的名字，允许重复", 
                "is_inited": true, 
                "vi_param": {
                "anything": "anything" 
                }, 
                "runhistory_cond": {
                  "anything"
                },
                "trig_param": {
                    "mode": 0, 
                    "param": {
                        "enable": 0, 
                        "delay": 50
                    }
                }, 
                "master_img": [
                    {
                        "id": 0,
                        "store_id": "P000_C_0",
                        "name": "用户随便填的名字，允许重复"
                    }, 
                    {
                        "id": 1,
                        "store_id": "P000_C_1",
                        "name": "用户随便填的名字，允许重复"
                    } 
                ],
                "node_list": {
                    "preproc": [
                        {
                            "node_id": "PRE0", 
                            "node_type": "COLOR_FILTER", 
                            "param" : {
                                "anything": "anything" 
                            }
                        }
                    ],
                    "atool": [
                        {
                            "node_id": "T0", 
                            "node_type": "LEARN_TOOL", 
                            "param" : {
                                "anything": "anything" 
                            }
                        },
                        {
                            "node_id": "T1",
                            "node_type": "POSITION_CORRECTION", 
                            "param" : {
                                "anything": "anything" 
                            }
                        }
                    ],
                    "postproc": [
                        {
                            "anything": "anything" 
                        }
                    ],
                    "output_logic": [
                        {
                        "node_id": "OL0", 
                        "node_type": "OUTPUT_LOGIC", 
                            "param" : {
                                "anything": "anything" 
                            }
                        }
                    ]
                } 
            }
        ] 
    }
}
```

### 当前基础报文格式

1. 报文格式
    所有端口接收到的应用层报文遵循以下格式：
    请求报文：
    Command-#Payload[CR]

    ﻿Simple Payload (ASCII) ：用于高频控制。参数间用  ,  分隔。
    Complex Payload (JSON) ：用于参数配置。Payload 为完整的 JSON 字符串。

    响应报文：
    成功： Command-#OK[CR]  或  Command-#ResultData[CR]
    失败： ER-#Command-#ErrorCode[CR]

    符号定义：
    结束符： CR  (0x0D)
    分隔符： -#

2. 传送图像、升级包等二进制数据格式
    上位机发送： Import_Img-#{"size":1024,"height":{int},"width":{int}, "color_space":{int}}-#binary
    下位机响应：
    注册主控： Import_Img-#{"status":"OK","target_cmd": "Reg_MasterImg"}
    添加样本： Import_Img-#{"status":"OK","target_cmd": "ATool_AddSample", "sample_id": "S_001" }
    设置vi源： Import_Img-#{"status":"OK","target_cmd": "Set_ViSrc"}

3. 传送图像、升级包等二进制数据格式
    上位机发送： Import_Img-#{"size":1024,"height":{int},"width":{int}, "color_space":{int}}-#binary
    下位机响应：
    注册主控： Import_Img-#{"status":"OK","target_cmd": "Reg_MasterImg"}
    添加样本： Import_Img-#{"status":"OK","target_cmd": "ATool_AddSample", "sample_id": "S_001" }
    设置vi源： Import_Img-#{"status":"OK","target_cmd": "Set_ViSrc"}

### 指令和接口

tip: `<xxx>`表示格式中的占位部分

查询程序数量和空间占用情况

1. pc->dev: `EXPORT_QUERY-#<json>CR`

    ```JSON
    {
        "is_prog_all":{int},
        "is_attach_run_history":{int},
        "prog_idx":[1,3,4]
    }
    ```

2. dev->pc: `EXPORT_QUERY-#<json>CR`

    ```JSON
    {
        "prog_count":{int},
        "prog_list": [
            {"id":{int}, "name":{string}, "size_mb":{int}, "has_thumb":1, "master_count":2,"master_list":[{"id":0},{"id":1}], "run_history_count": 10, "run_history_size_mb": 20},
            {"id":{int}, "name":{string}, "size_mb":{int}, "has_thumb":0, "master_count":2, "master_list":[{"id":0}], 
                "run_history_count": 30, "run_history_size_mb": 60
            }
        ]
    }
    ```

上位机下发备份文件生成指令，下位机生成完成or异常返回

1. pc->dev: `EXPORT_GENERATE-#<json>CR`

    ```JSON
    {
        "is_prog_all":{int},
        "is_attach_run_history":{int},
        "prog_idx":[1,3,4]
    }
    ```

2. dev->pc: `EXPORT_GENERATE-#<json>CR`

    ```JSON
    {
        "is_success":{int},
        "export_version": {float},
        "bin_size_byte": {int},
        "bin_md5": {int}
    }
    ```

可补充pc主动获取info的指令，用于断点续传，校验
TODO

导出传输文件
发起读取（创建会话）

1. pc->dev: `READ_FILE_BEGIN-#<json>CR`

    ```JSON
    {
        "type": {string},
        "name": {string}
    }
    ```

2. dev->pc: `READ_FILE_BEGIN-#<json>CR`

    ```JSON
    {
        "session_id": {int},
        "file_size": {int},
        "check_type": {int}   // 1=checksum, 2=crc32
    }
    ```

分片读取（循环）

1. pc->dev: `READ_FILE_DATA-#<json>CR`

    ```JSON
    {
        "session_id": {int},
        "offset": {int},
        "chunk_size": {int}
    }
    ```

2. dev->pc: `READ_FILE_DATA-#<json>-#<binary>CR`

    ```JSON
    {
        "session_id": {int},
        "offset": {int},
        "data_len": {int},
        "is_eof": {int},
        "check_type": {int},
        "check_value": {int}
    }
    ```

导入传输文件

发起写入（创建会话 + 加锁）

1. pc->dev: `WRITE_FILE_BEGIN-#<json>CR`

    ```JSON
    {
        "type": {string},
        "name": {string},
        "file_size": {int},
        "check_type": {int}
    }
    ```

2. dev->pc: `WRITE_FILE_BEGIN-#<json>CR`

    ```JSON
    {
        "session_id": {int}
    }
    ```

分片写入（循环，最后一包结束）

1. pc->dev: `WRITE_FILE_DATA-#<json>-#binaryCR`

    ```JSON
    {
        "session_id": {int},
        "offset": {int},
        "data_len": {int},
        "is_eof": {int},
        "check_type": {int},
        "check_value": {int},
        "file_check_value": {int}   // 仅 is_eof=1 时有效
    }
    ```

2. dev->pc: `WRITE_FILE_DATA-#<json>CR`

    ```JSON
    {
        "session_id": {int},
        "offset": {int},
        "data_len": {int},
        "status": 0
    }
    ```

导入解析（本地解析 + 校验）

1. pc->dev: `MPORT_PARSE-#<json>CR`

    ```JSON
    {
        "session_id": {int}
    }
    ```

2. dev->pc: `WRITE_FILE_DATA-#<json>CR`

    ```JSON
    {
        "session_id": {int},
        "status": 0
    }
    ```

数据提交（原子替换）

1. pc->dev: `IMPORT_COMMIT-#<json>CR`

    ```JSON
    {
        "session_id": {int}
    }
    ```

2. dev->pc: `IMPORT_COMMIT-#<json>CR`

    ```JSON
    {
        "session_id": {int},
        "status": 0
    }
    ```

导入取消

1. pc->dev: `MPORT_CANCEL-#<json>CR`

    ```JSON
    {
        "session_id": {int}
    }
    ```

2. dev->pc: `IMPORT_CANCEL-#OKCR`


template

1. pc->dev: `WRITE_FILE_DATA-#<json>-#binaryCR`

    ```JSON
    
    ```

2. dev->pc: `WRITE_FILE_DATA-#<json>CR`

    ```JSON
    
    ```

断点续传
TODO

协议约束
协议约束（必须实现）
1. 单会话限制
同一时间仅允许一个 WRITE_FILE_BEGIN 成功，否则返回 BUSY
2. session 校验
所有 DATA 请求必须校验 session_id
不匹配返回：ER-#XXX-#SESSION_INVALID
3. offset 校验
offset 必须严格匹配当前进度，否则返回错误
4. 超时释放
超过 N 秒无数据 → 自动释放 session
5. 校验规则
check_value = 当前分片校验
file_check_value = 整文件校验（仅最后一包）


协议约束（导入侧）
1. session 必须一致
所有 IMPORT_* 命令必须携带 session_id
否则返回 SESSION_INVALID
2. 状态顺序约束
WRITE_FILE_BEGIN → WRITE_FILE_DATA → IMPORT_PARSE → IMPORT_COMMIT
3. 失败处理
PARSE_FAIL → session 释放
COMMIT_FAIL → session 释放
4. 超时处理
长时间未操作 → 自动释放 session


| 序号 | 指令           | 参数                         | 响应              1   | 说明                         |
|------|----------------|------------------------------|----------------------|------------------------------|
| 1    | EXPORT_QUERY   | 无                           | EXPORT_QUERY-#{"prog_count":{int}, "prog_list":{"id":{int}, "name":{string}, "size_mb":{int}, "has_thumb":1
}, "run_history": }    | 查询设备可导出数据概况       |
| 2    | EXPORT_GENERATE| 导出类型(all/pro_list/...)   | READY + 文件信息     | 下位机生成 bak 文件          |
| 3    | READ_FILE_DATA | Offset, Length               | Binary 数据块        | 按偏移读取 bak 文件          |
| 4    | IMPORT_BEGIN   | 无                           | ACK                  | 进入导入模式                 |
| 5    | IMPORT_DATA    | Binary 数据块                | ACK                  | 写入导入数据                 |
| 6    | IMPORT_COMMIT  | 无                           | ACK/ERR              | 提交并应用导入数据           |
| 7    | IMPORT_ABORT   | 无                           | ACK                  | 中止导入流程                 |

## 时序导出1

```plantuml
@startuml
skinparam sequence {
    ActorBorderColor #03A9F4
    ActorBackgroundColor #B3E5FC
    LifeLineBorderColor #03A9F4
    LifeLineBackgroundColor #B3E5FC
    ParticipantBorderColor #03A9F4
    ParticipantBackgroundColor #B3E5FC
    ArrowColor #4FC3F7
    ArrowFontColor #0288D1
    ArrowFontSize 12
    ArrowThickness 1.5
    FontColor #01579B
    FontSize 14
}
skinparam shadowing true
skinparam backgroundColor #FFFFFF
skinparam dpi 150

actor USER as u
participant PC as p
participant DEV as d

note over d: 阶段 1: 下位机本地分步 IO 构建文件

u ->> p: 发起导出，勾选导出项后触发一次查询（all/pro_list/if_run_image_history）
' pc 查询备份相关信息
' p ->> d: [CMD] EXPORT_PREPARE(all/pro_list/run_history_image)
p ->> d: [CMD] EXPORT_QUERY()
activate d
' dev计算总大小，返回
d ->> d: 遍历数据结构
d --> p: [ACK] 程序数量，运行图像历史数量，占用空间大小
deactivate d

' u ->> p: 选择导出项（all/pro_list/if_run_image_history）
'根据选择显示对应的预估大小
p ->> p: 计算并显示用户当前组合项的总大小以及导出文件路径
' user 点击确定
u ->> p: 点击确认
activate d
' 上位机下发生成
p ->> d: [CMD] EXPORT_GENERATE 
' 下位机生成成功
d ->> d: 序列化内部数据，io输出

' 处理过程说明
' note right of d
'     1. 预留 FileHeader 空间 (Seek 0 + Offset)
'     2. 遍历数据：
'        - 序列化一个 APP_JSON -> 写入 Data 区
'        - 读取关联图像 -> 写入 Data 区
'        - **实时记录** 每个 Unit 的 Offset/Size 到临时索引列表
'     3. 数据写完后，在末尾写入 Index Table
'     4. 回到文件头 (Seek 0)，回填最终 FileHeader
'     5. 计算全文件 MD5
' end note
note right of d
    下位机数据导出处理说明（兼容两种方案）：

    方案一：文件夹打包 + 文件清单
    ------------
    1. 为每个数据单元创建独立文件（APP JSON / 图像 / 缩略图）
    2. 生成 JSON 文件清单，包含：
       - Unit ID
       - 类型（APP_JSON / 图像 / 缩略图）
       - 文件路径
       - 文件大小
       - MD5 校验值
    3. 可直接通过文件系统访问和验证单元
       （适合初期调试、快速对比和增量处理）

    方案二：自定义二进制 bak 文件
    ------------
    1. 预留 FileHeader 空间 (Seek 0 + Offset)
    2. 遍历数据：
       - 序列化一个 APP_JSON -> 写入 Data 区
       - 读取关联图像 / 缩略图 -> 写入 Data 区
       - **实时记录** 每个 Unit 的 Offset / Size / Checksum 到临时索引列表
    3. 数据写完后，在末尾写入 Index Table
    4. 回到文件头 (Seek 0)，回填最终 FileHeader
    5. 计算全文件 MD5
    6. 上位机仅负责分片传输整个 bak 文件，不关心内部单元组织
end note

' 返回generate ack
d -->> p: [ACK] READY (version, TotalSize, MD5)
deactivate d

' 传输备份文件
note over u, d: 阶段 2: 备份文件传输

loop 分片拉取 (Read by Offset)
    p ->> d: [CMD] READ_FILE_DATA (Offset, Length)
    d -->> p: [DATA] Binary_Block
    p ->> p: 直接追加到本地 .bak 文件
    p ->> u: 更新进度 (%)
end

p ->> p: 校验全文件 MD5
p ->> u: 导出成功

@enduml
```

## 时序导出2

```plantuml

@startuml
skinparam sequence {
    ActorBorderColor #03A9F4
    ActorBackgroundColor #B3E5FC
    LifeLineBorderColor #03A9F4
    LifeLineBackgroundColor #B3E5FC
    ParticipantBorderColor #03A9F4
    ParticipantBackgroundColor #B3E5FC
    ArrowColor #4FC3F7
    ArrowFontColor #0288D1
    ArrowFontSize 12
    ArrowThickness 1.5
    FontColor #01579B
    FontSize 14
}
skinparam shadowing true
skinparam backgroundColor #FFFFFF
skinparam dpi 150

actor USER as u
participant PC as p
participant DEV as d

note over d: 阶段 1: 下位机本地分步 IO 构建文件

u ->> p: 发起导出，勾选导出项后触发一次查询（all/pro_list/if_run_image_history）
' pc 查询备份相关信息
p ->> d: [CMD] EXPORT_QUERY()
activate d
' dev计算总大小，返回
d ->> d: 遍历数据结构
d --> p: [ACK] 程序数量，运行图像历史数量，占用空间大小
deactivate d

p ->> p: 计算并显示用户当前组合项的总大小以及导出文件路径
u ->> p: 点击确认

activate d
p ->> d: [CMD] EXPORT_GENERATE 
d ->> d: 序列化内部数据，io输出

note right of d
    下位机数据导出处理说明（兼容两种方案）:

    方案一：文件夹打包 + 文件清单
    ------------
    1. 为每个数据单元创建独立文件（APP JSON / 图像 / 缩略图）
    2. 生成 JSON 文件清单，包含：
       - Unit ID
       - 类型（APP_JSON / 图像 / 缩略图）
       - 文件路径
       - 文件大小
       - MD5 校验值
    3. 可直接通过文件系统访问和验证单元
       （适合初期调试、快速对比和增量处理）

    方案二：自定义二进制 bak 文件
    ------------
    1. 预留 FileHeader 空间 (Seek 0 + Offset)
    2. 遍历数据：
       - 序列化一个 APP_JSON -> 写入 Data 区
       - 读取关联图像 / 缩略图 -> 写入 Data 区
       - **实时记录** 每个 Unit 的 Offset / Size / Checksum 到临时索引列表
    3. 数据写完后，在末尾写入 Index Table
    4. 回到文件头 (Seek 0)，回填最终 FileHeader
    5. 计算全文件 MD5
    6. 上位机仅负责分片传输整个 bak 文件，不关心内部单元组织
end note

d -->> p: [ACK] READY (version, TotalSize, MD5)
deactivate d


' ================================
' 阶段 2: 备份文件传输（已修改）
' ================================

note over u, d: 阶段 2: 备份文件传输（基于 session）

' ✅ 新增：建立读取会话
p ->> d: [CMD] READ_FILE_BEGIN(type, name)
activate d

alt 设备空闲
    d -->> p: [ACK] (session_id, file_size, check_type)
else 设备忙
    d -->> p: [ERR] BUSY
end

deactivate d


loop 分片拉取 (Read by Offset)
    p ->> d: [CMD] READ_FILE_DATA (session_id, offset, chunk_size)
    activate d

    d -->> p: [DATA] (offset, data_len, is_eof, check_value) + Binary_Block

    deactivate d

    p ->> p: 校验分片 + 追加到本地 .bak 文件
    p ->> u: 更新进度 (%)

    alt is_eof == 1
        p -> p: break
        ' breako
    end
end

note over d
    说明：
    1. session_id 由设备生成管理
    1. is_eof=1 表示最后一包
    2. 设备自动释放 session
    3. 若长时间无请求，session 自动超时释放
end note

p ->> p: 校验全文件 MD5
p ->> u: 导出成功

@enduml
```

## 时序导入2

```plantuml

@startuml
skinparam sequence {
    ActorBorderColor #03A9F4
    ActorBackgroundColor #B3E5FC
    LifeLineBorderColor #03A9F4
    LifeLineBackgroundColor #B3E5FC
    ParticipantBorderColor #03A9F4
    ParticipantBackgroundColor #B3E5FC
    ArrowColor #4FC3F7
    ArrowFontColor #0288D1
    ArrowFontSize 12
    ArrowThickness 1.5
    FontColor #01579B
    FontSize 14
}
skinparam shadowing true
skinparam backgroundColor #FFFFFF
skinparam dpi 150

actor USER as u
participant PC as p
participant DEV as d

note over d: 阶段 1: 下位机导入准备 & 文件接收

u ->> p: 选择备份文件 (.bak)

p ->> p: 读取本地文件信息 (size, path)
p ->> p: 预读取 FileHeader (version, index_offset, MD5)

alt 文件非法（Header错误/版本不支持）
    p ->> u: 提示文件不合法
    return
end

' ✅ 修改：引入 session
p ->> d: [CMD] WRITE_FILE_BEGIN (type, name, TotalSize)
activate d

note right of d
    设备进入导入态：
    - 判断是否已有 session（否则返回 BUSY）
    - 创建 session_id
    - 清空临时文件/缓存区
    - 初始化导入状态机
end note

alt 成功
    d -->> p: [ACK] (session_id)
else 忙
    d -->> p: [ERR] BUSY
    deactivate d
    return
end

deactivate d


note over u, d: 阶段 2: 备份文件上传（分片写入，基于 session）

loop 分片上传 (Write by Offset)
    p ->> d: [CMD] WRITE_FILE_DATA (session_id, offset, data_len, is_eof, Binary_Block)
    activate d

    d ->> d: 校验 session_id
    d ->> d: 校验 offset
    d ->> d: 写入本地临时文件

    d -->> p: [ACK] (offset, status=OK)

    deactivate d

    p ->> u: 更新进度 (%)

    alt is_eof == 1
        ' break
    end
end

note over d
    说明：
    1. is_eof=1 表示最后一包
    2. 最后一包应包含 file_check_value（整文件校验）
    3. 若超时未接收数据 → 自动释放 session
end note


note over d: 阶段 3: 下位机本地解析 & 校验（保持原样）

p ->> d: [CMD] IMPORT_PARSE
activate d

note right of d
    下位机解析流程（完全本地执行，兼容两种方案）：

    方案一：文件夹打包 + 文件清单 + 加密
    ----------------------------------
    1. 遍历 JSON 文件清单：
       - Unit ID
       - 类型（APP_JSON / IMAGE / THUMB / RUN_HISTORY）
       - 文件路径
       - MD5
    2. 校验每个单元的 MD5
    3. 根据 type 分类处理：
       • PRO_JSON → 解析 JSON
       • IMAGE / THUMB → 写入图像存储
       • RUN_HISTORY_JSON → 构建历史结构
       • HISTORY_IMAGE → 关联历史图像
    4. 构建完整临时数据结构：
       - 程序列表
       - 图像资源池
       - 运行历史记录

    方案二：自定义二进制 bak 文件 + 加密
    ----------------------------------
    1. 校验整个 bak 文件完整性（全文件 MD5）
    2. 读取 FileHeader：
       - index_offset
       - index_size
    3. 定位 Index Table
    4. 遍历每个 IndexEntry：
       - 根据 offset 读取 UnitHeader + payload
       - 校验 checksum
       - 根据 type 分类处理：
           • PRO_JSON → 解析 JSON
           • IMAGE_* → 写入图像存储
           • RUN_HISTORY_JSON → 构建历史结构
           • HISTORY_IMAGE → 关联历史图像
    5. 构建完整临时数据结构：
       - 程序列表
       - 图像资源池
       - 运行历史记录
end note

alt 解析失败（MD5/格式/数据错误）
    d -->> p: [ERR] PARSE_FAIL
    p ->> u: 导入失败
    deactivate d
    return
end

d -->> p: [ACK] PARSE_OK
deactivate d


note over d: 阶段 4: 数据提交（原子替换）（保持原样）

p ->> d: [CMD] IMPORT_COMMIT
activate d

note right of d
    commit过程：
    - 用临时数据替换正式数据
    - 清理临时文件
    - 标记版本生效
end note

alt 成功
    d -->> p: [ACK] OK
    p ->> u: 导入成功
else 失败
    d -->> p: [ERR] COMMIT_FAIL
    p ->> u: 导入失败
end

deactivate d
@enduml
```

## 时序导入1

```plantuml
@startuml
skinparam sequence {
    ActorBorderColor #03A9F4
    ActorBackgroundColor #B3E5FC
    LifeLineBorderColor #03A9F4
    LifeLineBackgroundColor #B3E5FC
    ParticipantBorderColor #03A9F4
    ParticipantBackgroundColor #B3E5FC
    ArrowColor #4FC3F7
    ArrowFontColor #0288D1
    ArrowFontSize 12
    ArrowThickness 1.5
    FontColor #01579B
    FontSize 14
}
skinparam shadowing true
skinparam backgroundColor #FFFFFF
skinparam dpi 150

actor USER as u
participant PC as p
participant DEV as d

note over d: 阶段 1: 下位机导入准备 & 文件接收

u ->> p: 选择备份文件 (.bak)

p ->> p: 读取本地文件信息 (size, path)
p ->> p: 预读取 FileHeader (version, index_offset, MD5)

alt 文件非法（Header错误/版本不支持）
    p ->> u: 提示文件不合法
    return
end

' 通知设备进入导入模式
p ->> d: [CMD] IMPORT_PREPARE (TotalSize)
activate d

note right of d
    设备进入导入态：
    - 清空临时文件/缓存区
    - 初始化导入状态机
    - 准备接收完整二进制文件
end note

d -->> p: [ACK] READY
deactivate d


note over u, d: 阶段 2: 备份文件上传（分片写入）

loop 分片上传 (Write by Offset)
    p ->> d: [CMD] WRITE_FILE_DATA (Offset, Binary_Block)
    activate d
    d ->> d: 写入本地临时文件（按 Offset）
    d -->> p: [ACK] OK
    deactivate d

    p ->> u: 更新进度 (%)
end


note over d: 阶段 3: 下位机本地解析 & 校验

p ->> d: [CMD] IMPORT_PARSE
activate d

' note right of d
'     解析流程（完全本地执行）：
'     1. 校验文件完整性（MD5）
'     2. 读取 FileHeader：
'        - index_offset
'        - index_size
'     3. 定位 Index Table
'     4. 遍历每个 IndexEntry：
'        - 根据 offset 读取 UnitHeader + payload
'        - 校验 checksum
'        - 根据 type 分类处理：
'            • PRO_JSON → 解析 JSON
'            • IMAGE_* → 写入图像存储
'            • RUN_HISTORY_JSON → 构建历史结构
'            • HISTORY_IMAGE → 关联历史图像
'     5. 构建完整临时数据结构：
'        - 程序列表
'        - 图像资源池
'        - 运行历史记录
' end note
note right of d
    下位机解析流程（完全本地执行，兼容两种方案）：

    方案一：文件夹打包 + 文件清单 + 加密
    ----------------------------------
    1. 遍历 JSON 文件清单：
       - Unit ID
       - 类型（APP_JSON / IMAGE / THUMB / RUN_HISTORY）
       - 文件路径
       - MD5
    2. 校验每个单元的 MD5
    3. 根据 type 分类处理：
       • PRO_JSON → 解析 JSON
       • IMAGE / THUMB → 写入图像存储
       • RUN_HISTORY_JSON → 构建历史结构
       • HISTORY_IMAGE → 关联历史图像
    4. 构建完整临时数据结构：
       - 程序列表
       - 图像资源池
       - 运行历史记录

    方案二：自定义二进制 bak 文件 + 加密
    ----------------------------------
    1. 校验整个 bak 文件完整性（全文件 MD5）
    2. 读取 FileHeader：
       - index_offset
       - index_size
    3. 定位 Index Table
    4. 遍历每个 IndexEntry：
       - 根据 offset 读取 UnitHeader + payload
       - 校验 checksum
       - 根据 type 分类处理：
           • PRO_JSON → 解析 JSON
           • IMAGE_* → 写入图像存储
           • RUN_HISTORY_JSON → 构建历史结构
           • HISTORY_IMAGE → 关联历史图像
    5. 构建完整临时数据结构：
       - 程序列表
       - 图像资源池
       - 运行历史记录
end note

alt 解析失败（MD5/格式/数据错误）
    d -->> p: [ERR] PARSE_FAIL
    p ->> u: 导入失败
    return
end

d -->> p: [ACK] PARSE_OK
deactivate d


note over d: 阶段 4: 数据提交（原子替换）

p ->> d: [CMD] IMPORT_COMMIT
activate d

note right of d
    commit过程：
    - 用临时数据替换正式数据：
        • 覆盖 APP/PRO_JSON
        • 更新图像存储
        • 更新运行历史
    - 清理临时文件
    - 标记版本生效
end note

alt 成功
    d -->> p: [ACK] OK
    p ->> u: 导入成功
else 失败
    d -->> p: [ERR] COMMIT_FAIL
    p ->> u: 导入失败
end

deactivate d

@enduml
```

### state template

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
' start

'end
@enduml
```

### 导出-上位机

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

top to bottom direction

state pc_export_state {

    state "idle" as idle
    state "waiting_meta" as waiting_meta
    state "waiting_ready" as waiting_ready
    state "downloading" as downloading
    state "verifying" as verifying

    [*] --> idle

    idle --> waiting_meta : EXPORT_QUERY
    waiting_meta --> waiting_ready : META_INFO / EXPORT_GENERATE
    waiting_ready --> downloading : READY

    downloading --> downloading : READ_FILE_DATA
    downloading --> verifying : download_done

    verifying --> idle : success
    verifying --> idle : md5_fail

    waiting_meta --> idle : timeout
    waiting_ready --> idle : generate_fail
    downloading --> idle : transfer_fail
}

@enduml

```

### 导出-下位机1

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
' start

state dev_export_state {

    state "idle" as idle
    state "scanning" as scanning
    state "generating" as generating
    state "ready" as ready
    state "serving" as serving

    [*] --> idle

    idle --> scanning : EXPORT_QUERY
    scanning --> idle : META_INFO

    idle --> generating : EXPORT_GENERATE
    generating --> ready : file_ready

    ready --> serving : READ_FILE_DATA
    serving --> serving : READ_FILE_DATA
    serving --> idle : done

    generating --> idle : io_fail
}
@enduml
```

### 导入-上位机1

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
' start
state pc_import_state {

    state "idle" as idle
    state "preparing" as preparing
    state "uploading" as uploading
    state "processing" as processing

    [*] --> idle

    idle --> preparing : select_file / IMPORT_PREPARE
    preparing --> uploading : READY

    uploading --> uploading : WRITE_FILE_DATA
    uploading --> processing : upload_done

    processing --> idle : success
    processing --> idle : fail

    uploading --> idle : transfer_fail
}
@enduml
```

### 导入-下位机1

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
' start
state dev_import_state {

    state "idle" as idle
    state "preparing" as preparing
    state "receiving" as receiving
    state "processing" as processing

    [*] --> idle

    idle --> preparing : IMPORT_PREPARE
    preparing --> receiving : READY

    receiving --> receiving : WRITE_FILE_DATA
    receiving --> processing : receive_done

    processing --> idle : success
    processing --> idle : parse_fail / commit_fail

    receiving --> preparing : abort
}
@enduml
```


导出由pc控制节奏，可以
导入由 PC 主动驱动，因为“数据源在 PC，设备只是被动接收方”
自定义 bin 的核心价值是：统一结构 + 可校验 + 可扩展 + 高效传输  单文件，有索引可按模块解析
为什么要有 session？设备可能同时连多个客户端，但同一时间只能有一个传输。
session 就是给这次传输一个唯一标识，避免数据混乱



## 图片

![picture 0](../../../../../images/a9dc42753bcfe57c41ac0155677468702e208835e4206156cf8a1b9017dd0dee.png)  

![picture 1](../../../../../images/c74e0b40032f39d8d09bded8ee635af6327e67bf0e6afe1caebf19e359bad9cf.png)  

![picture 2](../../../../../images/6bd5e523c43c0ccb3f54811c4d50e0b2d9208d752cba288196451973782aa952.png)  

## 时序导出-方案2

```plantuml
@startuml
skinparam sequence {
    ActorBorderColor #03A9F4
    ActorBackgroundColor #B3E5FC
    LifeLineBorderColor #03A9F4
    LifeLineBackgroundColor #B3E5FC
    ParticipantBorderColor #03A9F4
    ParticipantBackgroundColor #B3E5FC
    ArrowColor #4FC3F7
    ArrowFontColor #0288D1
    ArrowFontSize 12
    ArrowThickness 1.5
    FontColor #01579B
    FontSize 14
}
skinparam shadowing true
skinparam backgroundColor #FFFFFF
skinparam dpi 150

actor USER as u
participant PC as p
participant DEV as d

' ================================
' 阶段 1: 查询数据规模（不变）
' ================================
u ->> p: 发起导出，选择导出项
p ->> d: [CMD] EXPORT_QUERY()
activate d

d ->> d: 遍历数据结构
d -->> p: [ACK] 程序数量 / 运行历史 / 空间估算

deactivate d

p ->> p: 计算总大小 & 显示路径
u ->> p: 点击确认

' ================================
' 阶段 2: 数据单元流式导出（核心修改）
' ================================

note over u, d: 阶段 2: 数据单元流式导出（无本地文件）

' ✅ 建立导出会话
p ->> d: [CMD] EXPORT_BEGIN(type, filter)
activate d

alt 设备空闲
    d ->> d: 初始化导出上下文（游标/迭代器）
    d -->> p: [ACK] (session_id, total_unit_count)
else 设备忙
    d -->> p: [ERR] BUSY
end

deactivate d


' ================================
' 按“数据单元”逐个/分片获取
' ================================

loop 拉取数据单元（Unit）

    p ->> d: [CMD] EXPORT_READ_UNIT(session_id)
    activate d

    d ->> d: 
          • PRO_JSON
          • PRO_MAIN_IMG
          • PRO_THUMB
          • RUN_HISTORY_JSON
          • RUN_IMAGE / THUMB

        - 序列化为 payload
        - 计算 CRC32 / MD5

    d -->> p: 
        [DATA]
        (unit_id, type, size, is_last, check_value)
        + Binary_Payload

    deactivate d

    note right of p
        上位机处理：
        1. 根据 type 分类
        2. 可选：
           - 写入文件夹（调试方案）
           - 写入自定义 bak（正式方案）
        3. 记录 Index / MD5
    end note

    p ->> u: 更新进度

    alt is_last == 1
        break
    end

end


' ================================
' 阶段 3: 收尾
' ================================

' p ->> p:
'     - 若为 bak：
'         • 写 Index Table
'         • 回填 FileHeader
'         • 计算整体 MD5

' p ->> u: 导出完成

@enduml
```

## 方案2的思考

如何保证数据一致性（运行中数据会变）

必须引入“快照（Snapshot）”

如何让用户选择导出内容

必须先给“可视化结构（Manifest）”

如何让用户选择导出内容

必须先给“可视化结构（Manifest）”


1. EXPORT_QUERY        → 获取统计信息
2. EXPORT_SNAPSHOT     → 固定数据状态（关键！）
3. EXPORT_MANIFEST     → 获取结构清单（可选项）
4. USER选择导出项
5. EXPORT_BEGIN        → 下发任务
6. EXPORT_READ_UNIT    → 按Unit拉取
7. EXPORT_END

数据流方向
导出，上位机拉，根据manifest，按自己节奏获取
导入，上位机推，根据manifest，根据下位机反馈推入

## 方案3

现在方案改了，和领导商讨以后，之前有一个信息忽略了，文件分为几类，一个是总的json配置，里面包含了相机的程序逻辑和工具逻辑以及所有的配置项目，路径是确定的，然后是程序关联的主控图像，缩略图像，以及程序下面各种工具的，总结起来就是所有备份迁移需要的东西都是在路径下存在的实体，并且所有文件都是分类好放在一定目录结构下的，那么本质上其实只要暴露对应的文件访问接口，在备份前上位机获取到备份需要的文件列表，然后调用文件接口去操作，就可以直接相当于备份了，导入同样的，把文件还原到原有位置不就行了，这样只需要定义通用的文件传输协议，加上一些权限，安全管理，然后在触发导入导出之后，上位机查询获取到备份列表，自己组合文件操作备份即可

基于文件清单（Manifest）的文件级导入导出备份模型

由于所有需要备份的数据结构都在设备文件系统中存在实体，并且按一定文件夹结构组织，那么只需要按相对路径保持原有的结构进行备份，还原即可

设备 = 一个文件系统
备份 = 文件复制
导入 = 文件还原

因为只要保持文件夹结构来导出恢复文件，就能实现备份
所以我们的指令和接口可以如何设计，即上位机进行文件访问的指令和接口，如何通用化，并且要有权限或者功能限制，我的想法是 在协议中对特定目录或者文件可以设置权限或者是用途type，这是在下位机或者配置文件中可以配置的，比如某个文件夹下的文件，指令对其进行文件操作时，如果没有设定正确的权限值或者用途type，那么会返回对应错误码的，然后就是协议具体的设计，是否有规范呢标准呢，毕竟http都有文件访问的一些功能

设备侧轻量文件服务协议（File Service Protocol）

1. 上位机可浏览文件（list）
2. 可读文件（read）
3. 可写文件（write）
4. 有权限控制（安全）
5. 有业务语义（type）
6. 可选传输前压缩解压以及加密处理

### nfs指令接口定义

查询最大层数设置
查询类型
覆盖or清空or跳过
防止写一半，写入过程有缓存，提交过程

nfs能力查询

1. pc->dev: `FILE_CAP_QUERY-#<json>CR`
2. dev->pc: `FILE_CAP_QUERY-#<json>CR`

   ```json
   {
        "max_chunk_size": 65536,
        "support_compress": ["none", "gzip"],
        "support_encrypt": ["none", "aes128"],
        "root_paths": ["/config", "/prog", "/image"]
    }
   ```

nfs文件列表查询

1. pc->dev: `FILE_LIST-#<json>CR`

   ```json
   {
        "path": "/prog",
        "recursive": 1
    }
   ```

2. dev->pc: `FILE_LIST-#<json>CR`

   ```json
   {
        "file_count": 2,
        "files": [
            {
                "path": "/prog/1/config.json",
                "size": 1024,
                "md5": "xxx",
                "type": "PRO_CONFIG_JSON",
                "perm": 1
            },
            {
                "path": "/prog/1/main.jpg",
                "size": 204800,
                "md5": "xxx",
                "type": "PRO_MAIN_IMAGE",
                "perm": 1
            }
        ]
    }
   ```

另外关于格式，还是太麻烦了，按我目前的格式展出，能否你按我这个格式
[cmd]-{"":"","":xx} 指令描述
这样就都放到一行里就行了，然后关于指令的名称，用FS作为指令前缀，麻烦把所有指令接口罗列一边，按这个格式

能力类
FS_CAP_QUERY-{} 查询设备文件服务能力
FS_CAP_QUERY-{"max_chunk_size":65536,"support_compress":["none","gzip"],"support_encrypt":["none","aes128"],"root_paths":["/config","/prog","/image"]} 返回能力信息

查询类

FS_STAT-{"path":"/a.json"} 查询文件
FS_STAT-{"code":0,"msg":"OK","path":"/a.json","size":100,"md5":"xxx"} 返回成功
FS_STAT-{"code":-2,"msg":"NOT_FOUND","path":"/b.json"} 返回失败

FS_LIST-{"path":"/prog","recursive":1,"max_depth":2,"list_type":"all","max_count":1000,"cursor":0} 查询文件列表
FS_LIST-{"file_count":120,"has_more":1,"next_cursor":100,"files":[{"path":"/prog/1/","is_dir":1},{"path":"/prog/1/config.json","size":1024,"type":"PRO_CONFIG_JSON","is_dir":0}]} 返回文件列表

FS_BATCH_STAT-{"paths":["/a.json","/b.json","/c.json"]} 批量查询文件
FS_BATCH_STAT-{"code":0,"msg":"OK","results":[
    {"path":"/a.json","code":0,"size":100,"md5":"xxx"},
    {"path":"/b.json","code":-2,"msg":"NOT_FOUND"},
    {"path":"/c.json","code":0,"size":1024,"md5":"yyy"}
]} 返回结果

读取类（大文件）
FS_OPEN_READ-{"path":"/prog/1/config.json"} 打开读取会话
FS_OPEN_READ-{"session_id":1,"file_size":10240} 返回读取会话信息

FS_READ_DATA-{"session_id":1,"offset":0,"size":65536} 分片读取文件
FS_READ_DATA-{"offset":0,"data_len":4096,"is_eof":0,"crc32":12345678}#payload(binary) 返回数据分片

FS_CLOSE-{"session_id":1} 关闭读取会话
FS_CLOSE-{"status":0} 返回关闭结果

写入类 （大文件）

FS_OPEN_WRITE-{"path":"/prog/1/config.json","total_size":10240} 打开写入会话
FS_OPEN_WRITE-{"session_id":1} 返回写入会话信息

FS_WRITE_DATA-{"session_id":1,"offset":0}#payload(binary) 写入数据分片
FS_WRITE_DATA-{"written":4096} 返回写入结果


文件操作类
FS_DELETE-{"path":"/prog/1/config.json"} 删除文件
FS_DELETE-{"status":0} 返回删除结果

FS_MKDIR-{"path":"/prog/2/"} 创建目录
FS_MKDIR-{"status":0} 返回创建结果

FS_MKDIR-{"path":"/prog/2/"} 创建目录
FS_MKDIR-{"status":0} 返回创建结果

检验类
FS_CHECKSUM-{"path":"/prog/1/config.json","type":"md5"} 计算文件校验值
FS_CHECKSUM-{"value":"xxxxxxxx"} 返回校验结果

查询
FS_QUERY_SPACE-{"path":"","unit":"MB","include_child":1}
FS_QUERY_SPACE-{"total_size":40,"used_size":50,"free_size":1, "status":0}

FS_QUERY_FREE_SPACE-{"path":"","unit":"MB","include_child":1}
FS_QUERY_FREE_SPACE-{"total_size":40,"used_size":50,"free_size":1, "status":0}
FS_QUERY_DIR_SIZE-{"path":"/target/dir_or_file","include_child":1,"unit":"MB"}
FS_QUERY_DIR_SIZE-{"size":123.5,"file_count":20,"dir_count":5,"status":0,"error_msg":""}

现在帮我按照公司规范格式生成修改所有指令和接口
现在帮我把上面所有的指令转化为下面的格式


查询程序数量和空间占用情况

1. pc->dev:  `EXPORT_QUERY-#<json1>CR `

2. dev->pc: ` EXPORT_QUERY-#<json2>CR `

```json

```

```json

```

## FS 指令接口

template

1. pc->dev: ``
2. dev->pc: ``

```json

```

```json

```

查询设备文件服务能力 (FS_CAP_QUERY)

1. pc->dev: `FS_CAP_QUERY-#<json>CR`
2. dev->pc: `FS_CAP_QUERY-#<json>CR`

```json
{}
```

```json
{
    "max_chunk_size": 65536,
    "support_compress": ["none", "gzip"],
    "support_encrypt": ["none", "aes128"],
    "root_paths": ["/config", "/prog", "/image"]
}
```

查询单个文件状态 (FS_STAT)

1. pc->dev: `FS_STAT-#<json>CR`
2. dev->pc: `FS_STAT-#<json>CR`

```json
{
    "path": "/a.json"
}
```

```json
{
    "code": 0,
    "msg": "OK",
    "path": "/a.json",
    "size": 100,
    "md5": "xxx"
}
```

获取文件列表 (FS_LIST)

1. pc->dev: `FS_LIST-#<json>CR`
2. dev->pc: `FS_LIST-#<json>CR`

```json
{
    "path": "/prog",
    "recursive": 1,
    "max_depth": 2,
    "list_type": "all",
    "max_count": 1000,
    "cursor": 0
}
```

```json
{
    "file_count": 120,
    "has_more": 1,
    "next_cursor": 100,
    "files": [
        {"path": "/prog/1/", "is_dir": 1},
        {"path": "/prog/1/config.json", "size": 1024, "type": "PRO_CONFIG_JSON", "is_dir": 0}
    ]
}
```

批量查询文件 (FS_BATCH_STAT)

1. pc->dev: `FS_BATCH_STAT-#<json>CR`
2. dev->pc: `FS_BATCH_STAT-#<json>CR`

```json
{
    "paths": ["/a.json", "/b.json", "/c.json"]
}
```

```json
{
    "code": 0,
    "msg": "OK",
    "results": [
        {"path": "/a.json", "code": 0, "size": 100, "md5": "xxx"},
        {"path": "/b.json", "code": -2, "msg": "NOT_FOUND"},
        {"path": "/c.json", "code": 0, "size": 1024, "md5": "yyy"}
    ]
}
```


打开读取会话 (FS_OPEN_READ)

1. pc->dev: `FS_OPEN_READ-#<json>CR`
2. dev->pc: `FS_OPEN_READ-#<json>CR`

```json
{
    "path": "/prog/1/config.json"
}
```

```json
{
    "session_id": 1,
    "file_size": 10240
}
```


分片读取数据 (FS_READ_DATA)

1. pc->dev: `FS_READ_DATA-#<json>CR`
2. dev->pc: `FS_READ_DATA-#<json>#<binary_payload>CR`

```json
{
    "session_id": 1,
    "offset": 0,
    "size": 65536
}
```

```json
{
    "offset": 0,
    "data_len": 4096,
    "is_eof": 0,
    "crc32": 12345678
}
```

打开写入会话 (FS_OPEN_WRITE)

1. pc->dev: `FS_OPEN_WRITE-#<json>CR`
2. dev->pc: `FS_OPEN_WRITE-#<json>CR`

```json
{
    "path": "/prog/1/config.json",
    "total_size": 10240
}
```

```json
{
    "session_id": 1
}
```

写入数据分片 (FS_WRITE_DATA)

1. pc->dev: `FS_WRITE_DATA-#<json>#<binary_payload>CR`
2. dev->pc: `FS_WRITE_DATA-#<json>CR`

```json
{
    "session_id": 1,
    "offset": 0
}
```

```json
{
    "written": 4096,
    "status": 0
}
```

关闭会话 (FS_CLOSE)

1. pc->dev: `FS_CLOSE-#<json>CR`
2. dev->pc: `FS_CLOSE-#<json>CR`

```json
{
    "session_id": 1
}
```

```json
{
    "status": 0
}
```

查询程序数量和空间占用 (EXPORT_QUERY)

1. pc->dev: `EXPORT_QUERY-#<json>CR`
2. dev->pc: `EXPORT_QUERY-#<json>CR`

```json
{
    "is_prog_all": 1,
    "is_attach_run_history": 1,
    "prog_idx": [1, 3, 4]
}
```

```json
{
    "prog_count": 2,
    "prog_list": [
        {
            "id": 1, 
            "name": "prog_a", 
            "size_mb": 10, 
            "has_thumb": 1, 
            "master_count": 2, 
            "master_list": [{"id": 0}, {"id": 1}], 
            "run_history_count": 10, 
            "run_history_size_mb": 20
        },
        {
            "id": 3, 
            "name": "prog_b", 
            "size_mb": 15, 
            "has_thumb": 0, 
            "master_count": 1, 
            "master_list": [{"id": 0}], 
            "run_history_count": 30, 
            "run_history_size_mb": 60
        }
    ]
}
```


查询目录详情 (FS_QUERY_DIR_SIZE)

1. pc->dev: `FS_QUERY_DIR_SIZE-#<json>CR`
2. dev->pc: `FS_QUERY_DIR_SIZE-#<json>CR`

```json
{
    "path": "/target/dir_or_file",
    "include_child": 1,
    "unit": "MB"
}
```

```json
{
    "size": 123.5,
    "file_count": 20,
    "dir_count": 5,
    "status": 0,
    "error_msg": ""
}
```

删除文件 (FS_DELETE)

1. pc->dev: `FS_DELETE-#<json>CR`
2. dev->pc: `FS_DELETE-#<json>CR`

```json
{
    "path": "/prog/1/config.json"
}
```

```json
{
    "status": 0
}
```

创建目录 (FS_MKDIR)

1. pc->dev: `FS_MKDIR-#<json>CR`
2. dev->pc: `FS_MKDIR-#<json>CR`

```json
{
    "path": "/prog/2/"
}
```

```json
{
    "status": 0
}
```

计算校验值 (FS_CHECKSUM)

1. pc->dev: `FS_CHECKSUM-#<json>CR`
2. dev->pc: `FS_CHECKSUM-#<json>CR`

```json
{
    "path": "/prog/1/config.json",
    "type": "md5"
}
```

```json
{
    "value": "xxxxxxxx",
    "status": 0
}
```

## 导入导出业务层接口

1. 导出查询
   1. EXPORT_QUERY

    ```json
    {
        "is_prog_all": 1,
        "is_attach_run_history": 1,
        "prog_idx": [1, 3, 4]
    }
    ```

    ```json
    {
        "roots": ["/mnt/sdcard", "/data"],
        "files": [
            { "file_idx": 0, "roots_idx": 0, "relative_path": "log/a.txt", "size": 1024, "md5": "abcd1234" },
            { "file_idx": 1, "roots_idx": 1, "relative_path": "config/a.txt", "size": 2048, "md5": "efgh5678" }
        ],
        "prog_file_list": [
            { "prog_id": "101", "file_idx_list": [1,2,3] }
        ],
        "config_file":{"file_idx":1}
    }
    ```

2. 导出开始
   1. EXPORT_START
   2. OK
   3. ERR
3. 导出结束
   1. EXPORT_END
   2. OK
   3. ERR
4. 导入开始
   1. IMPORT_START
   2. OK
   3. ERR
5. 导入结束
   1. IMPORT_END
   2. OK
   3. ERR


现在基于这个FS的存在，修改一下导入导出的时序图
主要流程就是用户发起导出时，pc查询设备端的备份文件列表，dev会返回一份备份列表，目前比较纠结的东西，设备端的文件列表以及文件按什么结构存储到上位机，比较麻烦的一点，首先我们导出后再导入，肯定是要把文件还原到固定文件夹的，最简单的方案就是 传输的列表就是所有文件的绝对路径，完全拷贝设备中的目录，在上位机这边的备份文件夹从根目录开始创建，完全还原相关文件需要的父文件夹，但是这样查看以及便利提取时就有些麻烦，万一大家的文件层数都比较深，所以是否有办法 根路径+相对路径的方式，假设100个文件，用这个表示，实际上就两个根目录，所有文件都是这个根目录的相对路径，在这个基础创建父目录就行，需要时拼接一下根目录，问题是用什么方式表示合适，可能大家共同的目录比较少

那么接下去就剩修改时序图了还有补充导入导出除了FS以外的指令了,请参考之前的进行修改，先给我导出的时序图

！！！ 除了文件以外，还需要映射到获取到程序列表
现在基于这个FS的存在，修改一下导入导出的时序图
主要流程就是用户发起导出时，pc查询设备端的备份文件列表，dev会返回一份备份列表
然后上位机根据备份列表，创建本地缓存，然后将备份列表中的文件一个一个读取拷贝到本地，校验md5，并且要保持结构，就用前面说的roots单独存这样，然后压缩打包加密，那么导入的过程也一样，发起导入后，解压获取列表，用户进行选择，然后确认导入文件，解析以后调用FS指令导入,最后commit

```plantuml
@startuml
' !pragma teoz true

skinparam sequence {
    ActorBorderColor #03A9F4
    ActorBackgroundColor #B3E5FC
    LifeLineBorderColor #03A9F4
    LifeLineBackgroundColor #B3E5FC
    ParticipantBorderColor #03A9F4
    ParticipantBackgroundColor #B3E5FC
    ArrowColor #4FC3F7
    ArrowFontColor #0288D1
    ArrowFontSize 12
    ArrowThickness 1.5
    FontColor #01579B
    FontSize 14
}
skinparam shadowing true
skinparam backgroundColor #FFFFFF
skinparam dpi 150

' 1 发起导出，选择导出项

' 2 上位机下发查询指令，下位机返回备份文件映射 roots files prog_list

' 3 上位机确定客户当前选择的导出项组合，通过 [CMD][FS] 指令组查询 文件占用空间，以及获取一些用于显示的缩略图文件

' 4 上位机创建缓存文件夹，清理数据，准备接受备份文件

' 5 上位机根据 备份文件清单 通过 [CMD][FS] 指令组合 从下位机拷贝目标文件到当前缓存文件夹，保持清单中描述的原有目录结构

' 6 上位机 所有备份文件传输完毕，打包，加密

actor USER as u
participant PC as p
participant DEV as d
participant nte

' ================================
' 导出流程
' ================================
' legend right

note over of p
阶段1：查询备份列表
end note
' 1. 发起导出，选择导出项
u ->> p: 发起导出，选择导出项

' 2. 上位机下发查询指令，下位机返回备份文件映射 roots/files/prog_list
p ->> d: [CMD] Backup_Manifest_Query[CR]
' 2.1 note

activate d
d ->> d: 遍历相关备份文件，整合成清单列表
d -->> p: [ACK] Backup_Manifest_Query-#<json>[CR]

deactivate d

' 3. 上位机确定当前选择的导出项组合，通过 [CMD][FS] 查询文件占用空间及缩略图
' p ->> d: [CMD][FS_*] 查询目标文件占用空间 + 缩略图
' activate d
' d ->> d: 响应[FS_*]指令集，执行对应文件操作
' d -->> p: [ACK] 列表文件大小信息 + 缩略图
' deactivate d

' 4. 上位机创建缓存文件夹，清理数据，准备接受备份文件
p ->> p: 创建本地缓存文件夹\n将接收到的manifest保存为json文件存入文件夹根目录，用于导入时反向解析
p ->> p: 清理旧数据，准备接受文件并整理

' 4.1 上位机发出导出命令，下位机检查是否存在异常条件
p ->> d: [CMD]Backup_Export_Start[CR] 
activate d
d ->> d: 检查是否存在异常条件\n准备缓存资源
alt 条件异常
d ->> p: ER-#Backup_Export_Start-#33[CR]
p ->> u: 下位机异常，导出终止
else
end
deactivate d

note over of p
阶段2：通用文件接口传输文件
end note
' 5. 上位机根据备份文件清单，通过 [CMD][FS] 指令组合，从下位机拷贝目标文件到当前缓存文件夹，保持原有目录结构
loop 遍历备份文件清单
    p ->> d: [CMD] File_SVC_Read_Open
    activate d
    alt 设备空闲
        d -->> p: [ACK] File_SVC_Read_Open-#{"session_id":1,"file_size":10240}[CR]
    else 设备忙
        d -->> p: [ERR] ER-#File_SVC_Read_Open-#33[CR]
    end
    deactivate d

    loop 执行读取
        p ->> d: [CMD] File_SVC_Read_Data-#{"session_id":1,"offset":0,"size":65536}[CR]
        activate d
        d -->> p: [DATA] File_SVC_Read_Data-#{"offset":0,"data_len":4096,"is_eof":0,"crc32":12345678}-#<bin>[CR]
        deactivate d
        ' note over p: 为保持文件在设备 FS 上的原有路径，同时避免路径冲突或过深，备份清单采用 roots + files 形式：\n roots 为 JSON 数组，每个元素表示一个根目录，但在本地缓存中使用索引值作为文件夹名以保证唯一性；\n files 为数组，每个对象包含 roots_idx（指向根目录索引）和 relative_path（相对于该根目录的路径）；\n 上位机接收文件时，先在缓存目录下根据 roots 索引创建唯一目录，然后再按 relative_path 创建子目录，从而完整还原设备原有路径结构
        ' note over p
        
        p ->> p: 校验分片 + 写入本地缓存文件 + 并保持文件在设备fs上的路径结构
        p ->> u: 更新进度 (%)
    end
    p ->> d: [CMD] File_SVC_Close-#{"session_id":1}[CR]
    alt ok
    d ->> p: [ACK] File_SVC_Close-#OK[CR]
    else
    end

    p ->> p: 校验文件 MD5
end


' 6. 上位机所有备份文件传输完毕，打包，加密
p ->> p: 打包 + 压缩 + 加密

' 6.1 上位机发出结束指令，暂时没有作用
p ->> d: [CMD] Backup_Export_End[CR]
activate d
alt 没毛病
d ->> d: check and cleanup
d ->> p: [ACK] Backup_Export_End-#OK[CR]
p ->> u: 导出完成
end
deactivate d
' note right of nte
note over d, nte
【路径还原与清单说明】

    一、设备端文件示例

    假设设备上存在以下文件：

    /mnt/sdcard/log/a.txt
    /mnt/sdcard/log/b.txt
    /data/config/a.txt

    ----------------------------------------

    二、导出阶段（设备 → 上位机）

    下位机根据导出项生成一份备份清单 JSON：

    roots:
    [
        "/mnt/sdcard",
        "/data"
    ]

    files:
    [
        { "roots_idx": 0, "relative_path": "log/a.txt" },
        { "roots_idx": 0, "relative_path": "log/b.txt" },
        { "roots_idx": 1, "relative_path": "config/a.txt" }
    ]

    说明：
    - roots 表示所有涉及的根目录
    - files 通过 roots_idx + relative_path 描述完整路径

    ----------------------------------------

    三、上位机接收与本地缓存(电脑存储方案取决于上位机，这里提供参考方案)

    上位机在导出过程中：

    1. 解析该 JSON 清单
    2. 在本地缓存目录（如 /backup/）下创建结构：

       /backup/0/log/a.txt
       /backup/0/log/b.txt
       /backup/1/config/a.txt

    说明：
    - 使用 roots 索引（0、1）作为本地根目录名，保证唯一性
    - relative_path 保持原有子目录结构

    ----------------------------------------

    四、清单文件落盘

    上位机会将该 JSON 清单一并保存到备份目录根目录，例如：

    /backup/manifest.json

    说明：
    - manifest.json 是导入时还原路径的唯一依据
    - 不依赖本地目录名（0/1），避免歧义
    - 支持跨设备、跨环境恢复

    ----------------------------------------

    五、路径还原方式（导入时）

    实际路径 = roots[roots_idx] + relative_path

    示例：
    0 + log/a.txt → /mnt/sdcard/log/a.txt
    1 + config/a.txt → /data/config/a.txt
end note 

@enduml
```

```plantuml
@startuml
skinparam sequence {
    ActorBorderColor #03A9F4
    ActorBackgroundColor #B3E5FC
    LifeLineBorderColor #03A9F4
    LifeLineBackgroundColor #B3E5FC
    ParticipantBorderColor #03A9F4
    ParticipantBackgroundColor #B3E5FC
    ArrowColor #4FC3F7
    ArrowFontColor #0288D1
    ArrowFontSize 12
    ArrowThickness 1.5
    FontColor #01579B
    FontSize 14
}
skinparam shadowing true
skinparam backgroundColor #FFFFFF
skinparam dpi 150

actor USER as u
participant PC as p
participant DEV as d
' ================================
' 导入流程（引入 IMPORT_START 会话）
' ================================

note over p
阶段1：解析备份包获取导入清单
end note
' 1. 发起导入，选择待导入的备份文件
u ->> p: 发起导入，选择备份文件

' 2. 上位机解压备份文件，获取 roots/files/prog_list
p ->> p: 解压备份文件包
p ->> p: 解析文件包根目录的 JSON 清单文件\n从而获知包内所有文件对应的下位机实际存储路径\n整理待导入文件队列

' 3. 用户确认导入项组合
' u ->> p: 确认导入的项目和文件
' p ->> p: 计算导入所需空间

' 4. 查询设备目标目录空间是否足够
' p ->> d: [CMD][FS_QUERY_SPACE] 目标路径
' activate d
' d -->> p: [ACK] 可用空间大小
' deactivate d

' alt 空间不足
'     p ->> u: 提示空间不足，取消导入或调整
' else 空间充足
'     ' 继续导入流程
' end

' 5. 上位机触发导入会话
p ->> d: [CMD] Backup_Import_Start[CR]
activate d
d ->> d: check & prepare resource
alt 设备可导入
    d -->> p: [ACK] Backup_Import_Start-#OK[CR]
else 设备忙或状态不允许
    d -->> p: [ERR] ER-#Backup_Import_Start-#33[CR]
    p ->> p: waiting 
end
deactivate d

alt IMPORT_START成功
    ' 6. 上位机准备上传文件到下位机
    ' p ->> p: 创建临时上传队列
    ' p ->> p: 整理文件路径，保持 roots+relative_path 结构

note over p
阶段2：通用文件接口写入备份
end note
    ' 7. 上位机逐个文件通过 [CMD][FS] 上传
    loop 处理导入文件队列
        p ->> d: [CMD] File_SVC_Write_Open-#{"path":"/prog/1/config.json","total_size":10240}[CR]
        activate d
        alt 设备空闲
            d -->> p: [ACK] File_SVC_Write_Open-#{"session_id":1}[CR]
        else 设备忙
            d -->> p: [ERR] ER-#File_SVC_Write_Open-#33[CR]
            p -->> p: wait
        end
        deactivate d

        loop 执行写入
            p ->> d: [CMD] File_SVC_Write_Data-#{"session_id":1,"offset":0}-#<bin>[CR]
            activate d
            d -->> p: [ACK] File_SVC_Write_Data-#{"written":4096,"status":0}[CR]
            deactivate d

            note over p
                文件路径由 roots_idx + relative_path 拼接而成
                上位机按该路径上传文件，保持设备原有目录结构
            end note

            p ->> p: 校验分片
            p ->> u: 更新上传进度 (%)
        end
        p ->> d: [CMD] File_SVC_Close-#{"session_id":1}[CR]
        activate d
        d ->> d: check file md5
        alt ok
        d ->> p: [ACK] File_SVC_Close-#OK[CR]
        deactivate d
        else
        end
    end

    ' 8. 上位机调用业务层完成导入提交
    p ->> d: [CMD] Backup_Import_End[CR]
    activate d
    d ->> d: 清理资源和缓存
    alt 成功
    d -->> p: [ACK] Backup_Import_End-#OK[CR]
    p ->> u: 导入完成
    else 失败
    d ->> p: ER-#Backup_Import_End-#33[CR]
    p ->> u: 导入失败
    deactivate d
end

@enduml
```

导入过程产生的异常如何预防？和异常处理？

![picture 3](../../../../../images/5d53229f0799b36a8f2abd85ebc3e39b30a2e957d02dd8966b98044a6f6c7a99.png)  

![picture 4](../../../../../images/86f6e0ec37d102bccac1b7a8608ae01485313d67922bfba84f37d1305046e9f1.png)  
