---
id: 6qmp1uwzegznvplrzbzsa2t
title: Dev_tftp
desc: ''
updated: 1775186710127
created: 1775183958694
---

## PC设置网络-连接设备 telnet or ssh

当前设备有新老旧款，固件中只会存在一个服务，要么telnet 要么ssh

## telnet中设备的文件传输服务

1. 确认设备上的tftp
   确认当前只有客户端

```bash
/oem/app/test # tftp
BusyBox v1.34.1 (2025-11-13 18:36:29 CST) multi-call binary.

Usage: tftp [OPTIONS] HOST [PORT]

Transfer a file from/to tftp server

        -l FILE Local FILE
        -r FILE Remote FILE
        -g      Get file
        -p      Put file
        -b SIZE Transfer blocks in bytes
```

只有 TFTP 客户端 →
👉 所有传输“必须由设备发起”
👉 PC 只能被动作为 TFTP Server

## 设置pc端的tftp服务

![picture 0](../../../../../images/df57a0656a77d3cff17b9ff21e776ef9e6b137670d7c9488f2400a83a37477d0.png)  

## 设备从PC获取文件和传输文件

```bash
/oem/app/test # tftp
BusyBox v1.34.1 (2025-11-13 18:36:29 CST) multi-call binary.

Usage: tftp [OPTIONS] HOST [PORT]

Transfer a file from/to tftp server

        -l FILE Local FILE
        -r FILE Remote FILE
        -g      Get file
        -p      Put file
        -b SIZE Transfer blocks in bytes
/oem/app/test # tftp -g -r test.yuv -l /oem/app/test/test_get.yuv 192.168.100.124
test.yuv             100% |**************************************************************************************************************************************************************| 1800k  0:00:00 ETA
/oem/app/test # ping 192.168.100.124
PING 192.168.100.124 (192.168.100.124): 56 data bytes
^C
--- 192.168.100.124 ping statistics ---
3 packets transmitted, 0 packets received, 100% packet loss
/oem/app/test # ar
arch    arp     arping
/oem/app/test # ar
arch    arp     arping
/oem/app/test # arping 192.168.100.124
ARPING 192.168.100.124 from 192.168.100.123 eth0
Unicast reply from 192.168.100.124 [34:5a:60:3d:f8:40] 0.704ms
Unicast request from 192.168.100.124 [34:5a:60:3d:f8:40] 437.558ms
Unicast reply from 192.168.100.124 [34:5a:60:3d:f8:40] 0.837ms
^CSent 2 probe(s) (1 broadcast(s))
Received 3 response(s) (1 request(s), 0 broadcast(s))
/oem/app/test # ls
test_get.yuv     upload_file.txt
/oem/app/test # ls -la
total 1804
drwxr-x---    2 root     0              304 Jan  1 23:53 .
drwxrwxrwx    4 root     0             4576 Jan  1 23:38 ..
-rw-r-----    1 root     0          1843200 Jan  1 23:53 test_get.yuv
-rw-r-----    1 root     0               31 Jan  1 23:39 upload_file.txt
/oem/app/test # tftp -p -l ./upload_file.txt -r upload_file.txt  192.168.100.124
upload_file.txt      100% |**************************************************************************************************************************************************************|    31  0:00:00 ETA
/oem/app/test #

C:\Users\default-user-xs>ping 192.168.100.123

正在 Ping 192.168.100.123 具有 32 字节的数据:
来自 192.168.100.123 的回复: 字节=32 时间<1ms TTL=64
来自 192.168.100.123 的回复: 字节=32 时间<1ms TTL=64
来自 192.168.100.123 的回复: 字节=32 时间<1ms TTL=64

```

发现的确 设备ping windows无法成功，反向可以，有可能windows存在防火墙

## 封装dev端 tftp客户端脚本

```bash
#!/bin/sh

# ------------------------
# TFTP 脚本配置
# ------------------------
TFTP_SERVER_IP=192.168.100.124
TFTP_DEFAULT_LOCAL_DIR=/oem/app/test

# ------------------------
# 参数检查
# ------------------------
if [ $# -lt 2 ]; then
    echo "Usage:"
    echo "  $0 get <filename>"
    echo "  $0 put <filename>"
    exit 1
fi

TRANSFER_MODE=$1
TARGET_FILENAME=$2

# ------------------------
# 判断本地路径
# ------------------------
case "$TARGET_FILENAME" in
  /*)
    LOCAL_FILE_PATH="$TARGET_FILENAME"
    ;;
  *)
    LOCAL_FILE_PATH="$TFTP_DEFAULT_LOCAL_DIR/$TARGET_FILENAME"
    ;;
esac

# ------------------------
# 执行 TFTP
# ------------------------
case "$TRANSFER_MODE" in
    get)
        echo "[TFTP GET] $TARGET_FILENAME from $TFTP_SERVER_IP ..."
        tftp -g -r "$TARGET_FILENAME" -l "$LOCAL_FILE_PATH" "$TFTP_SERVER_IP"
        ;;
    put)
        echo "[TFTP PUT] $TARGET_FILENAME to $TFTP_SERVER_IP ..."
        tftp -p -r "$TARGET_FILENAME" -l "$LOCAL_FILE_PATH" "$TFTP_SERVER_IP"
        ;;
    *)
        echo "[ERR] Unknown command: $TRANSFER_MODE"
        exit 1
        ;;
esac

# ------------------------
# 状态输出
# ------------------------
if [ $? -eq 0 ]; then
    echo "[OK] $LOCAL_FILE_PATH"
else
    echo "[FAIL]"
fi
```

使用方式

```bash
chmod +x tftp.sh

./tftp.sh get test.yuv
./tftp.sh put upload_file.txt
```

将脚本添加到全局环境变量中，以便调用

放在 /oem/app/test/scripts：

```bash
mkdir -p /oem/app/test/scripts
## 创建文件 dev_tftp.sh
chmod +x /oem/app/test/scripts/dev_tftp.sh
```

2️⃣ 加入 PATH（当前会话生效）：

```bash
export PATH=$PATH:/oem/app/test/scripts
```

3️⃣ 永久生效：

```bash
echo 'export PATH=$PATH:/oem/app/test/scripts' >> /etc/profile
source /etc/profile
```

之后全局调用：

```bash
tftp_dev get test.yuv
tftp_dev put upload_file.txt
```
