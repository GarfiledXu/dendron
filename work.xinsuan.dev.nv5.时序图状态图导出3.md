---
id: nfaz8k2kb25b68us7z2u6bq
title: 时序图状态图导出3
desc: ''
updated: 1775110879461
created: 1775028030561
---

## 功能定义

## 指令和接口定义

### 备份导入导出业务层协议

### 通用文件服务协议

0. template
   1. cmd
   2. 上位机请求:xxxx

        ```json

        ```

   3. 下位机响应:xxxx

        ```json

        ```

   4. 完整报文

        ```json
        ```

        ```json
        ```

1. 查询-支持功能
   1. FS_CAP_QUERY
   2. 上位机请求:查询设备文件服务能力

        ```json
        无
        ```

   3. 下位机响应:返回支持的块大小、压缩方式及根路径

        ```json
        {
            "max_chunk_size": 65536,
            "support_compress": [
                "none",
                "gzip"
            ],
            "root_paths": [
                "/xxx/xxx/config",
                "/xx/xx/xx/prog"
            ]
        }
        ```

   4. 完整报文

        ```json
        ```

        ```json
        ```

2. 查询-单文件状态
   1. FS_STAT
   2. 上位机请求:查询单个文件详细状态

        ```json
        {"path":"/a.json"}
        ```

   3. 下位机响应:返回文件大小、MD5等信息

        ```json
        {
            "code": 0,
            "msg": "OK",
            "path": "/a.json",
            "size": 100,
            "md5": "xxx"
        }
        ```

   4. 完整报文

        ```json
        ```

        ```json
        ```

3. 查询-文件列表
   1. FS_LIST
   2. 上位机请求:遍历指定目录下的文件

        ```json
        {"path":"/prog","recursive":1,"max_count":1000,"cursor":0}
        ```

   3. 下位机响应:返回文件列表

        ```json
        {
            "file_count": 120,
            "has_more": 1,
            "next_cursor": 100,
            "files": [
                {
                    "path": "/prog/1/",
                    "is_dir": 1
                }
            ]
        }
        ```

   4. 完整报文

        ```json
        ```

        ```json
        ```

4. io-读取文件打开
   1. FS_FILE_READ_OPEN
   2. 上位机请求:打开文件，准备读取

        ```json
        {"path":"/prog/1/config.json"}
        ```

   3. 下位机响应:打开文件fd，返回分配的 Session ID 及文件总大小

        ```json
        {"session_id":1,"file_size":10240}
        ```

   4. 完整报文

        ```json
        ```

        ```json
        ```

5. io-文件读取
   1. FS_FILE_READ_DATA
   2. 上位机请求:按偏移量，请求文件数据

        ```json
        {"session_id":1,"offset":0,"size":65536}
        ```

   3. 下位机响应:返回 JSON 描述及紧跟的二进制数据

        ```json
        {"offset":0,"data_len":4096,"is_eof":0,"crc32":12345678}#<binary_payload>
        ```

   4. 完整报文

        ```json
        ```

        ```json
        ```

6. io-写入文件打开
   1. FS_FILE_WRITE_OPEN
   2. 上位机请求:请求文件写入

        ```json
        {"path":"/prog/1/config.json","total_size":10240}
        ```

   3. 下位机响应:打开文件，准备写入

        ```json
        {"session_id":1}
        ```

   4. 完整报文

        ```json
        ```

        ```json
        ```

7. io-写入文件写入
   1. FS_FILE_WRITE_DATA
   2. 上位机请求:发送包含二进制负载的分片数据

        ```json
        {"session_id":1,"offset":0}#<binary_payload>
        ```

   3. 下位机响应:返回实际写入长度及状态

        ```json
        {"written":4096,"status":0}
        ```

   4. 完整报文

        ```json
        ```

        ```json
        ```

8. io-文件关闭
   1. FS_CLOSE
   2. 上位机请求:结束当前的读写会话并释放资源

        ```json
        {"session_id":1}
        ```

   3. 下位机响应:xxxx

        ```json
        {"status":0}
        ```

   4. 完整报文

        ```json
        ```

        ```json
        ```

9. 操作-文件删除
   1. FS_DELETE
   2. 上位机请求:删除指定路径的文件或目录

        ```json
        {"path":"/prog/1/config.json"}
        ```

   3. 下位机响应:返回操作结果状态码

        ```json
        {"status":0}
        ```

   4. 完整报文

        ```json
        ```

        ```json
        ```

10. 操作-创建文件夹
    1. FS_MKDIR
    2. 上位机请求:在指定路径创建新目录

        ```json
        {"path":"/prog/2/"}
        ```

    3. 下位机响应:返回操作结果状态码

        ```json
        {"status":0}
        ```

    4. 完整报文

        ```json
        ```

        ```json
        ```


## uml时序图

### 备份导出时序

### 备份导入时序
