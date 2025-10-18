---
id: c4hbees0qvx7m38wnl1t1fv
title: Wifi_manul_config
desc: ''
updated: 1760068574854
created: 1760062377694
---

## 手动配置

```bash
ip link set wlan0 up
wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
wpa_cli remove_network 0
wpa_cli remove_network 1
wpa_cli add_network
wpa_cli set_network 0 ssid "\"Libawall\""
wpa_cli set_network 0 psk "\"11223344\""
wpa_cli enable_network 0
wpa_cli save_config
wpa_cli reconnect
wpa_cli status
```
