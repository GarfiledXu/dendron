---
id: kjfki1af7pbwrasmzp21b6n
title: 协议说明文档
desc: ''
updated: 1778820624408
created: 1778816786822
---

**模块名称**   ：   `nvs_backup_sync`   (设备备份与导入导出服务设计文档)

# 1. 模块概述与设计原则

## 1.1 功能定义

通过上位机的指令触发，将下位机的关键文件导出以及导入，来备份和恢复功能配置

整个导入导出流程划分为两个关键阶段：

1. **清单同步阶段**  ：上位机从下位机获取或解析全局文件快照（  `manifest.json`  ），明确需要传输的文件范围及属性。
1. **文件遍历传输阶段**  ：上位机基于会话（Session）机制，逐个对文件进行分块读取/写入与校验。

---

# 2. 关键机制说明

## 2.1 清单内容详解 manifest.json

清单内容分离了根目录组和文件组，以解决嵌入式设备跨分区备份时的路径冗余问题。

- `roots`   数组：存储所有涉及的绝对根路径（如   `["/oem/cfg", "/oem/data"]`  ）。
- `files`   数组：每个文件对象通过   `roots_idx`   指向对应的根路径。

**路径恢复（通过清单列表拼接文件在下位机的绝对路径）**  ：  `Device_Absolute_Path = roots[files[i].roots_idx] + "/" + files[i].relative_path`  。

## 2.2 上位机在导出过程的本地存储逻辑

**【执行时机】**  ：上位机在调用   `Backup_Manifest_Query`   并成功保存为   `manifest.json`   后，开始遍历   `files`   列表进行正式下载  **之前**  。

**【痛点】**  ：下位机不同分区下可能存在相对路径完全相同的文件（例如   `/oem/cfg/sys/v1.json`   和   `/oem/data/sys/v1.json`  ）。如果上位机直接按相对路径保存，会导致后者覆盖前者。

**【操作指南】**  ：  
上位机人员在创建本地目录树时，遵循以下算法：

1. **建立根映射层**  ：在本地备份文件夹下，根据   `manifest.json`   中的   `roots`   数组长度，创建名为   `0`  ,   `1`  ,   `2`  ... 的子文件夹。
1. **递归创建路径**  ：对于每一个文件，读取其   `roots_idx`  。
1. **保存路径构造**
1. ：
1.     - **正确做法**  ：  `Local_Save_Path = Backup_Dir / {roots_idx} / {relative_path}`  。
    - **示例**  ：文件 A 的   `roots_idx`   为 0，相对路径为   `nvs/config.db`  。则本地路径为   `.../Backup_20240515/0/nvs/config.db`  。

1. **优势**  ：这种逻辑确保了即便下位机路径冲突，在上位机侧也能通过索引文件夹实现物理隔离。

## 2.3 文件传输协议: 文件分片与分页传输机制说明

为了支持大文件传输并应对不稳定的网络环境，协议引入了分片机制。

**1. 会话控制 (Session ID)**

- 上位机通过   `Open`   指令申请句柄，下位机返回   `session_id`  。
- `session_id`   在下位机内部关联了一个打开的文件描述符（FD）和当前文件指针。在   `Close`   之前，该 ID 保持有效，避免了频繁打开/关闭文件的 IO 开销。

**2. 分页偏移量逻辑 (Offset & Size)**

- **offset (偏移量)**  ：标记当前分片在整个文件中的起始字节位置。从 0 开始计数。
- **size (分片大小)**  ：本次请求读取/写入的长度。建议固定为 65536 (64KB)。
- **传输流程**
- ：
  - 上位机发起请求：  `offset=0, size=64K`  。
  - 下位机返回 64K 数据。
  - 上位机将指针累加，发起第二次请求：  `offset=65536, size=64K`  。
  - **以此类推**  ，直到已接收字节总数等于   `File_SVC_STAT`   返回的   `total_size`  。

**3. 容错与重传**

- 如果某一个分片（Chunk）传输失败，上位机无需重新下载整个文件，只需记录当前失败的   `offset`  ，重新发起该分片的请求即可。

---

# 3. 上下位机报文交互详解

## 3.1 备份导出流程 (Export)

### 步骤 1：启动导出

- **TX**  :   `Backup_Export_Start`  
- **RX**  :   `Backup_Export_Start-#OK`  

### 步骤 2：获取清单

- **TX**  :   `Backup_Manifest_Query`  
- **RX**  :   `Backup_Manifest_Query-#{ "roots": ["/oem/cfg", "/oem/data"], "files": [{"roots_idx": 0, "relative_path": "nvs/img.bmp", "size": 1229878, "md5": "f7068f50e..."}] }`  

### 步骤 3：单文件下载循环 (重点)

**1) 获取状态**

- **TX**  :   `File_SVC_STAT-#{"path":"/oem/cfg/nvs/img.bmp"}`  
- **RX**  :   `File_SVC_STAT-#{"size": 1229878, "md5": "f7068f50e..."}`  

**2) 开启会话**

- **TX**  :   `File_SVC_Read_Open-#{"path":"/oem/cfg/nvs/img.bmp"}`  
- **RX**  :   `File_SVC_Read_Open-#{"session_id": 1002}`  

**3) 分片拉取 (Pagination Loop)**

- **第一次**  :   `TX: File_SVC_Read_Data-#{"offset": 0, "session_id": 1002, "size": 65536}`  
- **第二次**  :   `TX: File_SVC_Read_Data-#{"offset": 65536, "session_id": 1002, "size": 65536}`  
- *循环执行直到所有字节下载完毕。*

**4) 关闭会话**

- **TX**  :   `File_SVC_Close-#{"session_id": 1002}`  

### 步骤 4：将缓存文件夹整体打包，压缩/加密

最终文件格式   `***.nvsa**`  

---

## 3.2 备份导入流程 (Import)

### 步骤 0: 将导入的   `*nvsa`  文件解压

**读取其 manifest.json文件反序列化, 遍历所有files，拼接绝对路径，并确认是否存在本地文件**

### **步骤 1**  ：启动导入任务

- **TX**  :   `Backup_Import_Start`  
- **RX**  :   `Backup_Import_Start-#OK`  

### 步骤 2：单文件写入循环

**1) 开启写入会话**

- **TX**  :   `File_SVC_Write_Open-#{"md5": "093ef0ea...", "path": "/oem/cfg/nvs/img.bmp", "total_size": 13366}`  
- **RX**  :   `File_SVC_Write_Open-#{"session_id": 1001}`  

**2) 分片推送 (Chunk Push)**

- **TX**  :   `File_SVC_Write_Data-#{"crc32": 1415676312, "data_len": 13366, "offset": 0, "session_id": 1001}-#[binary data]`  
- **RX**  :   `File_SVC_Write_Data-#{"written": 13366}`  

**3) 闭环核验 (Critical)**

- **TX**  :   `File_SVC_Close-#{"session_id": 1001}`  
- **TX**  :   `File_SVC_STAT-#{"path": "/oem/cfg/nvs/img.bmp"}`  
- **RX**  : 返回该路径 MD5，上位机比对本地 MD5，若失败需提示“数据损坏”。

---

## 3.3 结束与生效

- **TX**  :   `Backup_Import_End`  
- **TX**  :   `Production_Test-#{"type":"exit_mode"}`  
- **TX**  :   `Sys_Reboot-#{"mode":"normal"}`  

---

# 4. 上位机开发注意事项

1. **进度条计算**  ：进度条应基于文件总数或总字节数计算。由于存在大量小文件，建议以“已完成字节数 / 清单总字节数”展示
1. **校验逻辑说明：**
本协议采用“分片 CRC32 + 全局 MD5”的双重校验以确保文件传输可靠性：
1.     - **分片级校验 (CRC32)**  ：在子文件传输过程中，每帧报文均携带下位机计算的当前分片 CRC32 码。上位机实时校验，若失败则触发该分片的重读/重写
    - **文件级校验 (MD5)**  ：单个子文件完整落盘/读取后，对文件进行 MD5 校验。若校验未通过，则触发该子文件的全量重传

1. **异常处理与回滚机制说明：**
1.     - **异常判定**  ：交互过程中若返回错误响应（错误格式详见指令列表），即视为导入失败 通常原因有: 传输文件不具备导入导出合法权限/下位机系统io异常等
    - **静默备份**  ：鉴于导入操作会直接覆写现有配置，建议上位机在执行导入前，在后台隐式执行一次导出操作（对用户无感知）以生成临时备份
    - **故障恢复**  ：若导入动作因异常中断，上位机应立即使用该临时备份覆盖受损数据，完成全局配置的安全回滚

**打包后文件后缀格式:**  `***.nvsa**`  

# **5. 导出交互时序uml**

![image.png](http://172.18.1.12/atlas/files/public/69d5bafae4965e8c38449eb5/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjY5ZDViYWZhZTQ5NjVlOGMzODQ0OWViNSIsIjY5ZDViZTZhZTQ5NjVlOGMzODQ0OWViNiJdLCJpYXQiOjE3NzkwNzIyMTUsImV4cCI6MTc3OTA4MzAxNX0.rnPErBEYxZGw4ulaLY1l023_T9zmyQZZXCoEKw2i-6o)

# **6. 导入交互时序uml**

![image.png](http://172.18.1.12/atlas/files/public/69d5be6ae4965e8c38449eb6/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjY5ZDViYWZhZTQ5NjVlOGMzODQ0OWViNSIsIjY5ZDViZTZhZTQ5NjVlOGMzODQ0OWViNiJdLCJpYXQiOjE3NzkwNzIyMTUsImV4cCI6MTc3OTA4MzAxNX0.rnPErBEYxZGw4ulaLY1l023_T9zmyQZZXCoEKw2i-6o)

# **7. 导入导出功能业务层协议**

|分类|指令名称|请求/响应|参数|功能描述|完整报文示例|
|---|---|---|---|---|---|
|**查询-导出**|**Backup_Manifest_Query**|上位机请求|```
无

```|查询待导出程序的元数据与文件清单|**Backup_Manifest_Query**  [CR]|
|||下位机响应|```
{
    "roots": [
        "/mnt/sdcard",
        "/data"
    ],
    "files": [
        {
            "roots_idx": 0,
            "relative_path": "log/a.txt",
            "size": 1024,
            "md5": "abcd"
        }
    ]
}
```|返回根路径索引、文件列表及程序关联关系|成功:,**Backup_Manifest_Query**  **-#**  {"roots":["/mnt/sdcard","/data"],"files":[{"roots_idx":0,"relative_path":"log/a.txt","size":1024,"md5":"abcd"}]}  [CR],失败:,**ER**  **-#**  **Backup_Manifest_Query**  **-#101**  [CR],enum class Error {,        SUCCESS = 0,,        ERR_NOT_FOUND = 101,,        ERR_PERMISSION = 102,,        ERR_BUSY = 103,,        ERR_INVALID_SID = 104,,        ERR_IO = 105,,        ERR_TIMEOUT = 106,,        ERR_PARAM = 107,,        ERR_ALREADY_EXISTS = 108,,        ERR_MEMORY = 109,    // 新增：捕获 std::bad_alloc,        ERR_UNKNOWN = 199   // 新增：捕获未知异常,    };|
|**备份导出开始**|**Backup_Export_Start**|上位机请求|```
无
```|请求开启导出|**Backup_Export_Start**  [CR]|
|||下位机响应|```
无
```|确认进入导出状态或返回错误（ERR）|成功:,**Backup_Export_Start**  **-#**  **OK**  [CR],失败:,**ER**  **-#**  **Backup_Export_Start**  **-#105**  [CR]|
|**备份导出结束**|**Backup_Export_End**|上位机请求|```
无
```|请求结束导出事务（释放资源）|**Backup_Export_End**  [CR]|
|||下位机响应|```
无
```|确认导出完成|成功:,**Backup_Export_End**  **-#**  **OK**  [CR],失败:,**ER**  **-#**  **Backup_Export_End**  **-#105**  [CR]|
|**备份导入开始**|**Backup_Import_Start**|上位机请求|```
无
```|请求开启导入（预分配空间/清空缓存）|**Backup_Import_Start**  [CR]|
|||下位机响应|```
无
```|确认准备就绪或返回错误（ERR）|成功:,**Backup_Import_Start**  **-#**  **OK**  [CR],失败:,**ER**  **-#**  **Backup_Import_Start**  **-#105**  [CR]|
|**备份导入结束**|**Backup_Import_End**|上位机请求|```
无
```|结束导入并提交修改|**Backup_Import_End**  [CR]|
|||下位机响应|```
无
```|确认导入成功并完成校验|成功:,**Backup_Import_End**  **-#**  **OK**  [CR],失败:,**ER**  **-#**  **Backup_Import_End**  **-#105**  [CR]|


# 8. 通用文件服务接口协议定义 (V1.0)

  [http://172.18.1.12/wiki/spaces/RJKF/pages/UXA0UC23](http://172.18.1.12/wiki/spaces/RJKF/pages/UXA0UC23)  
