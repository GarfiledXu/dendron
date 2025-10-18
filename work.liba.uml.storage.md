---
id: ch7hqg310uqdwmp1ecq0ckg
title: Storage
desc: ''
updated: 1750215241362
created: 1750214849158
---

## flash

#查看flash写入的配置
vendor_storage -r VENDOR_SN_ID -t string
#falsh写入配置数据
vendor_storage -w VENDOR_SN_ID -t file -i /oem/sys.cfg