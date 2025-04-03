---
id: o6ufr9p3s8r110rub2bvsnv
title: After_sale
desc: ''
updated: 1743671182790
created: 1743669345264
---

### 20250403 建德-固件升级后网络问题 2.5.0 版本 

1. 推送ppp拨号配置文件

```bash
adb push ./quecteludev-ppp /etc/ppp/peers/
### 
chmod 755 /etc/ppp/peers/quecteludev-ppp
```

2. 推送curl到设备/bin

由于服务器访问改为了https，所以需要用到curl

```bash
adb push ./curl /bin
###
chmod 755 /bin/curl
```

3. 验证

```bash
killall daemon_service ydn-rk

./daemon_service 

tail -f log_*

```
