---
id: a3i60xm9mj3ndoe2o6287p2
title: Port_transform_curl_upload_file
desc: ''
updated: 1742377997785
created: 1742377793340
---


### 使用`adb端口转发`+`设备端curl`+`pc端file server`

```bash

#device
curl -F "file=@/oem/xjf1127/curl_upload_test_file" http://127.0.0.1:6003/upload

#pc


```