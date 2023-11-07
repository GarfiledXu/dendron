---
id: f6b1q6pi0l66cjuuw92ji09
title: Board Service
desc: ''
updated: 1696902058003
created: 1695101047589
---

#### to be repaired
- samba download fail
- rabbitmq get msg strip num limit
- add cache number control
- license transfer and manager
- log broker error, redirect to same path

#### optimize and code plan
- auto script test
  - launch task and prepare env by python script
- log
  - whether using 



#### logger demand
#### strategy of logger
**logger rotating stage**
1. output by target file name
2. check filesize and count when each output
3. if condition reach, and single file reach and max num not, rename target file by add file tag, to continue output, if single file reach and max num reach, rename target file by add file tag and delete oldest single file to continue
**strategy 1: rotating by log length**
1. using spdlog as log library
2. take memory and file number rotating strategy
3. using multi logger to record log on different local file
4. in unit of each parent task, record the tag of relative logger, and upload these log file end of task finished(or record current exist logger file name enqueue list and force rotate logger file to ensure the new task output is saved on new logger file on the begin of parent task, reduce these file in the end)
> 获取任务开始前文件列表+当前所在文件，在任务结束后遍历文件列表去除任务前除了任务所在文件外的文件，将所得文件列表上传
> 关键，获取当前logger回滚日志正在活动写入的文件名

**strategy 2: using tree struct, top rotating by task counter, and following second rotating by log file length**
1. record the rotating file name begin of task, upload the relative log end of task
> 将根据日志长度滚动作为一个单元匹操作，而任务计数则作为条件在顶层控制，用来更新/定向/滚动每次的单元操作，每次更新task，就更新日志滚动的根目录，相当于重置日志输出路径，只要控制滚动总数，那么总体的日志长度是小于n*max的



#### log supplement
##### java request
 {"parent_task_id":2983,"testbed_type":"2","hostname":"172.17.11.189","port":5672,"username":"admin","password":"admin","exchange":"cabinplatform.exchange","vhost":"cabin_platform_vhost","receive_routing_key":"cabinplatform.2983.0","receive_queue_name":"cabinplatform.2983","send_routing_key":"cabinplatform_comolete.2983.0","send_queue_name":"cabinplatform_comolete.2983","upload_result_dir_path":"smb://172.17.11.202/objective_result/2983/2983","username_smb":"IOTFS_Cabin","password_smb":"W3Qa5CZ345zXs","metrial_number":210,"createdTime":"2023-09-22 09:55:15"}
**need add target ip**

#### test flow




#### configure format
- launch cfg
  