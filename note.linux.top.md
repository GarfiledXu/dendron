---
id: 8wsoc2qki9nhq86aggtzyoi
title: Top
desc: ''
updated: 1743383748465
created: 1743383483532
---

### 在linux下使用top命令来查看具体进程的内存 cpu占用

```bash
## 目标程序 indep_venc_muxer
[root@RV1126_RV1109:/userdata/xjf1127/save_record_video]# top -bn 60 | grep indep

  PID USER      PR  NI    VIRT    RES  %CPU %MEM     TIME+ S COMMAND
 3103 root      20   0  354.7m  70.8m  38.3  9.7   0:00.59 S          `- indep+
 3103 root      20   0  376.6m  70.8m  47.1  9.7   0:01.31 S          `- indep+
 3103 root      20   0  376.6m  70.8m  42.3  9.7   0:01.97 S          `- indep+
 3103 root      20   0  376.6m  70.8m  42.9  9.7   0:02.63 S          `- indep+
 3103 root      20   0  376.6m  70.8m  41.9  9.7   0:03.28 S          `- indep+
 3103 root      20   0  376.6m  71.1m  42.6  9.7   0:03.94 S          `- indep+
 3103 root      20   0  376.6m  71.1m  43.2  9.7   0:04.61 S          `- indep+
 3103 root      20   0  376.6m  71.1m  45.5  9.7   0:05.31 S          `- indep+
 3103 root      20   0  376.6m  71.1m  43.2  9.7   0:05.98 S          `- indep+
 3103 root      20   0  376.6m  71.1m  41.7  9.7   0:06.63 S          `- indep+
 3103 root      20   0  376.6m  71.1m  42.3  9.7   0:07.29 S          `- indep+
 3103 root      20   0  376.6m  71.1m  43.9  9.7   0:07.97 S          `- indep+
 3103 root      20   0  376.6m  71.1m  42.6  9.7   0:08.63 S          `- indep+
 3103 root      20   0  376.6m  71.1m  43.2  9.7   0:09.30 S          `- indep+
 3103 root      20   0  376.6m  71.1m  42.9  9.7   0:09.96 S          `- indep+
 3103 root      20   0  376.6m  71.1m  45.2  9.7   0:10.66 S          `- indep+
 3103 root      20   0  376.6m  71.1m  41.7  9.7   0:11.31 S          `- indep+
 3103 root      20   0  376.6m  71.1m  42.9  9.7   0:11.97 S          `- indep+
 3103 root      20   0  376.6m  71.1m  41.3  9.7   0:12.61 S          `- indep+
 3103 root      20   0  376.6m  71.1m  42.2  9.7   0:13.26 S          `- indep+


```