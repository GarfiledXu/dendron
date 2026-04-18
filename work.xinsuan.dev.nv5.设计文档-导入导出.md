---
id: j5omphz8xb8aiu9yui5y9ux
title: 设计文档 导入dao'chu
desc: ''
updated: 1775617082723
created: 1775617077836
---

文档一：设备配置备份与恢复业务设计说明
1. 概述
本文档说明上位机（PC/APP）与下位机（设备端）配置备份与恢复的交互流程。整体原则是：上位机控制进度和解析清单，下位机只负责具体的文件读写和最后的目录重命名。 实际的文件传输使用底层的 File_SVC 接口。

2. 路径处理方案
为了避免下位机解析大 JSON 导致内存不足，清单文件 (manifest.json) 仅由上位机负责处理。文件路径通过索引拼接：

roots： 根目录数组（如 ["/mnt/sdcard", "/data"]）。

files： 具体文件信息，通过 roots_idx + relative_path 拼接出完整的设备端路径。

3. 导出流程 (设备 -> 上位机)
获取清单： 上位机发送 Backup_Manifest_Query，下位机返回包含根目录和文件列表的 JSON。

上位机准备： 上位机在本地按 roots 索引创建对应的文件夹，并把收到的 JSON 保存为 manifest.json。

开始导出： 上位机发送 Backup_Export_Start，下位机锁定相关配置文件，防止被其他程序修改。

拉取文件： 上位机遍历本地清单，通过 File_SVC_Read_Open、Data、Close 指令，逐个把文件下载到本地对应的目录中。

结束导出： 文件全部下载完后，上位机发送 Backup_Export_End，下位机解除文件锁定。

4. 导入流程 (上位机 -> 设备)
上位机准备： 上位机解析本地的 manifest.json，算出总文件大小。

开始导入： 上位机发送 Backup_Import_Start 并附带总大小。下位机检查磁盘空间，并在内部新建一个隐藏的临时目录（如 .tmp_backup）。

写入文件与校验： * 上位机通过 File_SVC_Write_Open 请求建文件，并把该文件的预期 MD5 传过去。

通过 File_SVC_Write_Data 传文件内容。

上位机发 File_SVC_Close，下位机此时计算刚写完的文件的 MD5，如果不匹配直接返回报错，上位机重传该文件。

目录替换生效： 所有文件传输并校验成功后，上位机发送 Backup_Import_End。下位机将当前的配置目录改名为备份目录，将 .tmp_backup 改名为正式配置目录，完成升级。

文档二：通用文件操作 (File_SVC) 接口说明
1. 概述
File_SVC 是用于大文件分片传输的底层封装接口，不包含具体业务逻辑。基于 session_id 进行上下文管理。

2. 内存与生命周期
缓存复用： 每次打开文件会分配一个 session_id。数据读取所需的内存 Buffer 和该 ID 绑定，在首次分片读取时分配，后续分片复用该 Buffer，避免频繁 malloc/free。

超时清理： 下位机会定时检查，如果某个 session_id 长时间没有读写动作，会自动关闭文件句柄并释放内存，防止上位机异常断开导致的资源泄漏。

3. 读取接口 (文件下载)
File_SVC_Read_Open：传入目标路径。返回 session_id 和文件总大小。

File_SVC_Read_Data：传入 session_id、偏移量和需要读取的大小。返回实际读取长度、偏移量和二进制文件数据。

File_SVC_Close：传入 session_id，关闭文件描述符并释放内存。

4. 写入接口 (文件上传)
File_SVC_Write_Open：传入目标路径和文件总大小。返回 session_id。

File_SVC_Write_Data：传入 session_id、偏移量和二进制数据。返回已写入长度。

File_SVC_Close：传入 session_id，下位机执行刷盘操作。关闭文件并释放内存。