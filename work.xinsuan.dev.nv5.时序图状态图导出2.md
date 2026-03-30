---
id: y3ro81pgx4og72jbqktffk6
title: 时序图状态图导出2
desc: ''
updated: 1774878208485
created: 1774852125219
---
## 指令和数据类型

prog_json
prog_master_image
prog_learn_image
prog_thumb_image
run_image_history_json
run_master_image
run_thumb_image

### 指令

0. 查询程序数量和空间占用情况
1. pc->dev
2. cmd: EXPORT_QUERY-#TODO

    ```JSON
    {
        "is_prog_all":{int},
        "is_attach_run_history":{int},
        "prog_idx":[1,3,4]
    }
    ```

3. rly: EXPORT_QUERY-#TODO

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

0. 上位机下发备份文件生成指令，下位机生成完成or异常返回
1. pc->dev
2. cmd: EXPORT_GENERATE-#TODO

    ```JSON
    {
        "is_prog_all":{int},
        "is_attach_run_history":{int},
        "prog_idx":[1,3,4]
    }
    ```

3. rly: EXPORT_GENERATE-#TODO

    ```JSON
    {
        "is_success":{int},
        "export_version": {float},
        "bin_size_byte": {int},
        "bin_md5": {int}
    }
    ```
可补充pc主动获取info的指令，用于断点续传，校验

0. 

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

### 时序导入1

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

