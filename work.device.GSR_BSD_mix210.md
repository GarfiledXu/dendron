---
id: qk9yl15jhvolo3zdzuid313
title: GSR_BSD_mix210
desc: ''
updated: 1715245254634
created: 1715241334888
---

### opearation cmd
```bash
### 上电 must

#### login
root
arcsoft
#### kill video, 否则sd卡内容会被清空
killall -9 video
#### mount 
cd ~ && mount -t vfat /dev/sda1 /mnt/sda1


### 插拔sdk卡
cd ~ && umount /mnt/sda1
cd ~ && mount -t vfat /dev/sda1 /mnt/sda1

#### 运行程序
cd /mnt/sda1/test && chmod 777 * && export LD_LIBRARY_PATH=./lib:./alg_lib:$LD_LIBRARY_PATH
 ./GSR_BSD_tool
```