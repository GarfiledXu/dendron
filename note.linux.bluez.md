---
id: wz2q15ctxmshpwey51v4xo6
title: Bluez
desc: ''
updated: 1751599843688
created: 1751250049581
---

### env

```bash
sudo apt update
sudo apt install -y bluez bluez-tools bluetooth libbluetooth-dev

sudo apt install -y \
    bluez \
    bluez-tools \
    libbluetooth-dev \
    libglib2.0-dev \
    libdbus-1-dev \
    libudev-dev \
    libical-dev \
    libreadline-dev \
    dbus \
    python3-dbus \
    python3-pydbus \
    bluetooth \
    pulseaudio-module-bluetooth
```

### wsl2如何识别window上的usb蓝牙

### 1126 device env

1. bluetoothd
2. bluetoothctl
3. dbus
4. dbus-daemon
5. hciattach
6. rfkill
7. readline

### dbus

#### dbus init.d

```bash
#!/bin/sh
### BEGIN INIT INFO
# Provides:          dbus
# Required-Start:    $local_fs
# Required-Stop:     $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: D-Bus message bus
### END INIT INFO

PID_FILE="/run/dbus/system_bus.pid"
DBUS_USER="dbus"

# Create dbus user if it doesn't exist
if ! id -u $DBUS_USER &>/dev/null; then
    echo "Creating D-Bus user"
    adduser --system --no-create-home --group $DBUS_USER
fi

# Create the D-Bus runtime directory if it doesn't exist
if [ ! -d /run/dbus ]; then
    echo "Creating /run/dbus directory"
    mkdir -p /run/dbus
    chown $DBUS_USER:$DBUS_USER /run/dbus
    chmod 755 /run/dbus
fi

case "$1" in
  start)
    # Check and remove the old PID file if D-Bus is not running
    if [ -e $PID_FILE ]; then
        if ps -p $(cat $PID_FILE) > /dev/null 2>&1; then
            echo "D-Bus is already running (PID: $(cat $PID_FILE))."
            exit 1
        else
            echo "Removing stale PID file."
            rm -f $PID_FILE
        fi
    fi

    echo "Starting D-Bus"
    dbus-daemon --system --fork --nopidfile
    echo $! > $PID_FILE  # Save the new PID
    ;;
    
  stop)
    if [ ! -e $PID_FILE ]; then
        echo "D-Bus is not running (PID file does not exist)."
        exit 1
    fi

    echo "Stopping D-Bus"
    kill $(cat $PID_FILE) && rm -f $PID_FILE
    ;;
    
  restart)
    $0 stop
    $0 start
    ;;

  status)
    if [ -e $PID_FILE ]; then
        echo "D-Bus is running (PID: $(cat $PID_FILE))."
    else
        echo "D-Bus is not running."
    fi
    ;;
    
  *)
    echo "Usage: /etc/init.d/dbus {start|stop|restart|status}"
    exit 1
    ;;
esac

exit 0
```

#### dbus muanl run

```bash
root@Rockchip:/run$ mkdir dbus
root@Rockchip:/run$ cd dbus/

root@Rockchip:/run/dbus$ dbus-daemon --system  --print-pid
dbus[2362]: Unknown username "pulse" in message bus configuration file
dbus-daemon[2362]: Failed to start message bus: Could not get UID and GID for username "dbus"

mkdir -p /home/dbus
adduser dbus

root@Rockchip:/$ dbus-daemon --system  --print-pid
dbus[2600]: Unknown username "pulse" in message bus configuration file
2601

#D-Bus 的配置文件通常位于 /etc/dbus-1/system.conf 和 /etc/dbus-1/session.conf。
# 这些文件定义了 D-Bus 守护进程的行为，包括权限、服务和用户访问控制等。

root@Rockchip:/etc/dbus-1/system.d$ ls
pulseaudio-system.conf  bluealsa.conf           bluetooth.conf
root@Rockchip:/etc/dbus-1/system.d$ rm bluealsa.conf 
root@Rockchip:/etc/dbus-1/system.d$ rm pulseaudio-system.conf 
root@Rockchip:/etc/dbus-1/system.d$ cd ..
root@Rockchip:/etc/dbus-1$ ls
system.d      system.conf   session.conf
root@Rockchip:/etc/dbus-1$ dbus-daemon --system  --print-pid
dbus-daemon[2949]: Failed to start message bus: The pid file "/run/messagebus.pid" exists, if the message bus is not running, remove this file
root@Rockchip:/etc/dbus-1$ killall dbus-daemon
root@Rockchip:/etc/dbus-1$ dbus-daemon --system  --print-pid
dbus-daemon[2957]: Failed to start message bus: The pid file "/run/messagebus.pid" exists, if the message bus is not running, remove this file
root@Rockchip:/etc/dbus-1$ killall dbus-daemon
killall: dbus-daemon: no process killed
root@Rockchip:/etc/dbus-1$ rm /run/messagebus.pid 
root@Rockchip:/etc/dbus-1$ dbus-daemon --system  --print-pid
2975

```

#### dbus test

```bash
## test
root@Rockchip:/$ dbus-monitor --system
root@Rockchip:/etc/dbus-1$ dbus-send --system --dest=org.freedesktop.DBus --type=method_call /org/freedesktop/DBus org.freedesktop.DBus.ListNames
```

### bluetooth

#### bluetoothctl

```bash
root@Rockchip:/etc/init.d$ bluetoothctl
## 输入后卡住，阻塞
##需要启动bluetoothd

## 多次重启后，进入交互
root@Rockchip:/$ bluetoothctl 
Agent registered        uetoothd...
[CHG] Controller FC:23:CD:57:98:F4 Pairable: yes
[bluetooth]#   

## 但还是没有任何反应
root@Rockchip:/$ bluetoothctl 
Agent registered        uetoothd...
[CHG] Controller FC:23:CD:57:98:F4 Pairable: yes
[bluetooth]# devices
[bluetooth]# device                                                            
[bluetooth]# scan on                                                           
[bluetooth]# power on                                                          
[bluetooth]# power on                                                          
[bluetooth]#       
```

#### bluetoothd

init.d

```bash
#!/bin/sh
 
PATH=/sbin:/bin:/usr/sbin:/usr/bin
DESC=bluetooth
 
DAEMON=/usr/libexec/bluetooth/bluetoothd
 
# If you want to be ignore error of "org.freedesktop.hostname1",
# please enable NOPLUGIN_OPTION.
# NOPLUGIN_OPTION="--noplugin=hostname"
NOPLUGIN_OPTION=""
SSD_OPTIONS="--oknodo --quiet --exec $DAEMON -- $NOPLUGIN_OPTION"
 
test -f $DAEMON || exit 0
 
# FIXME: any of the sourced files may fail if/with syntax errors
test -f /etc/default/bluetooth && . /etc/default/bluetooth
test -f /etc/default/rcS && . /etc/default/rcS
 
set -e
 
case $1 in
  start)
        echo "Starting $DESC"
 
        if test "$BLUETOOTH_ENABLED" = 0; then
                echo "disabled. see /etc/default/bluetooth"
                exit 0
        fi
 
        start-stop-daemon --start --background $SSD_OPTIONS
        echo "${DAEMON##*/}"
 
  ;;
  stop)
        echo "Stopping $DESC"
        if test "$BLUETOOTH_ENABLED" = 0; then
                echo "disabled."
                exit 0
        fi
        start-stop-daemon --stop $SSD_OPTIONS
        echo "${DAEMON}"
  ;;
  restart|force-reload)
        $0 stop
        sleep 1
        $0 start
  ;;
  status)
         pidof ${DAEMON} >/dev/null
         status=$?
        if [ $status -eq 0 ]; then
                 echo "bluetooth is running."
        else
                echo "bluetooth is not running"
        fi
        exit $status
   ;;
   *)
        N=/etc/init.d/bluetooth
        echo "Usage: $N {start|stop|restart|force-reload|status}" >&2
        exit 1
        ;;
esac
 
exit 0
```

#### bt-agent

help

```bash
root@Rockchip:/etc/init.d$ bt-adapter 
Usage:
  bt-adapter [OPTION?] - a bluetooth adapter manager

Version UNKNOWN

Help Options:
  -h, --help                   Show help options

Application Options:
  -l, --list                   List all available adapters
  -a, --adapter=<name|mac>     Adapter Name or MAC
  -i, --info                   Show adapter info
  -d, --discover               Discover remote devices
  -s, --set                    Set adapter property

Set Options:
  --set <property> <value>
  Where `property` is one of:
     Alias
     Discoverable
     DiscoverableTimeout
     Pairable
     PairableTimeout
     Powered

Report bugs to <zxteam@gmail.com>.Project home page <http://code.google.com/p/bluez-tools/>.
root@Rockchip:/etc/init.d$ bt-adapter -l
Available adapters:
BlueZ 5.65 (FC:23:CD:57:98:F4)
root@Rockchip:/etc/init.d$ bt-adapter -a FC:23:CD:57:98:F4 -i
[hci0]
  Name: BlueZ 5.65
  Address: FC:23:CD:57:98:F4
  Alias: BlueZ 5.65 [rw]
  Class: 0x0
  Discoverable: 0 [rw]
  DiscoverableTimeout: 180 [rw]
  Discovering: 0
  Pairable: 0 [rw]
  PairableTimeout: 0 [rw]
  Powered: 1 [rw]
  UUIDs: [00001801-0000-1000-8000-00805f9b34fb, 00001800-0000-1000-8000-00805f9b34fb, PnPInformation, AVRemoteControlTarget, AVRemoteControl, 0000180a-0000-1000-8000-00805f9b34fb]
```



### hci

#### hciconfig

hci相关命令操作不依赖dbus，和bluetooth

获取hci节点状态

```bash
root@Rockchip:/etc/init.d$ hciconfig
hci0:   Type: Primary  Bus: UART
        BD Address: FC:23:CD:57:98:F4  ACL MTU: 1021:8  SCO MTU: 255:12
        DOWN 
        RX bytes:1111 acl:0 sco:0 events:32 errors:0
        TX bytes:867 acl:0 sco:0 commands:32 errors:0
```

使能适配器

```bash
root@Rockchip:/etc/init.d$ hciconfig hci0 up
root@Rockchip:/etc/init.d$ hciconfig
hci0:   Type: Primary  Bus: UART
        BD Address: FC:23:CD:57:98:F4  ACL MTU: 1021:8  SCO MTU: 255:12
        UP RUNNING 
        RX bytes:2222 acl:0 sco:0 events:64 errors:0
        TX bytes:1530 acl:0 sco:0 commands:65 errors:0
```

#### hcitool

使用说明

```bash
hcitool - HCI Tool ver 5.65
Usage:
        hcitool [options] <command> [command parameters]
Options:
        --help  Display help
        -i dev  HCI device
Commands:
        dev     Display local devices
        inq     Inquire remote devices
        scan    Scan for remote devices
        name    Get name from remote device
        info    Get information from remote device
        spinq   Start periodic inquiry
        epinq   Exit periodic inquiry
        cmd     Submit arbitrary HCI commands
        con     Display active connections
        cc      Create connection to remote device
        dc      Disconnect from remote device
        sr      Switch central/peripheral role
        cpt     Change connection packet type
        rssi    Display connection RSSI
        lq      Display link quality
        tpl     Display transmit power level
        afh     Display AFH channel map
        lp      Set/display link policy settings
        lst     Set/display link supervision timeout
        auth    Request authentication
        enc     Set connection encryption
        key     Change connection link key
        clkoff  Read clock offset
        clock   Read local or remote clock
        lescan  Start LE scan
        leinfo  Get LE remote information
        lealadd Add device to LE Accept List
        lealrm  Remove device from LE Accept List
        lealsz  Read size of LE Accept List
        lealclr Clear LE Accept List
        lewladd Deprecated. Use lealadd instead.
        lewlrm  Deprecated. Use lealrm instead.
        lewlsz  Deprecated. Use lealsz instead.
        lewlclr Deprecated. Use lealclr instead.
        lerladd Add device to LE Resolving List
        lerlrm  Remove device from LE Resolving List
        lerlclr Clear LE Resolving List
        lerlsz  Read size of LE Resolving List
        lerlon  Enable LE Address Resolution
        lerloff Disable LE Address Resolution
        lecc    Create a LE Connection
        ledc    Disconnect a LE Connection
        lecup   LE Connection Update

For more information on the usage of each command use:
        hcitool <command> --help
```










































#### bluetoothctl bluetoothd

```bash
root@Rockchip:/etc/init.d$ /usr/libexec/bluetooth/bluetoothd 

root@Rockchip:/$ bluetoothctl 
Agent registered        uetoothd...
[CHG] Controller FC:23:CD:57:98:F4 Pairable: yes
[bluetooth]# 
```

**/etc/init.d/bluetooth**

```bash
#!/bin/sh
 
PATH=/sbin:/bin:/usr/sbin:/usr/bin
DESC=bluetooth
 
DAEMON=/usr/libexec/bluetooth/bluetoothd
 
# If you want to be ignore error of "org.freedesktop.hostname1",
# please enable NOPLUGIN_OPTION.
# NOPLUGIN_OPTION="--noplugin=hostname"
NOPLUGIN_OPTION=""
SSD_OPTIONS="--oknodo --quiet --exec $DAEMON -- $NOPLUGIN_OPTION"
 
test -f $DAEMON || exit 0
 
# FIXME: any of the sourced files may fail if/with syntax errors
test -f /etc/default/bluetooth && . /etc/default/bluetooth
test -f /etc/default/rcS && . /etc/default/rcS
 
set -e
 
case $1 in
  start)
	echo "Starting $DESC"
 
	if test "$BLUETOOTH_ENABLED" = 0; then
		echo "disabled. see /etc/default/bluetooth"
		exit 0
	fi
 
	start-stop-daemon --start --background $SSD_OPTIONS
	echo "${DAEMON##*/}"
 
  ;;
  stop)
	echo "Stopping $DESC"
	if test "$BLUETOOTH_ENABLED" = 0; then
		echo "disabled."
		exit 0
	fi
	start-stop-daemon --stop $SSD_OPTIONS
	echo "${DAEMON}"
  ;;
  restart|force-reload)
	$0 stop
	sleep 1
	$0 start
  ;;
  status)
	 pidof ${DAEMON} >/dev/null
	 status=$?
        if [ $status -eq 0 ]; then
                 echo "bluetooth is running."
        else
                echo "bluetooth is not running"
        fi
        exit $status
   ;;
   *)
	N=/etc/init.d/bluetooth
	echo "Usage: $N {start|stop|restart|force-reload|status}" >&2
	exit 1
	;;
esac
 
exit 0
 
# vim:noet

root@Rockchip:/etc/init.d$ /etc/init.d/bluetooth start
/bin/sh: /etc/init.d/bluetooth: Permission denied

chmod +x /etc/init.d/bluetooth


Commands:
        dev     Display local devices
        inq     Inquire remote devices
        scan    Scan for remote devices
        name    Get name from remote device
        info    Get information from remote device
        spinq   Start periodic inquiry
        epinq   Exit periodic inquiry
        cmd     Submit arbitrary HCI commands
        con     Display active connections
        cc      Create connection to remote device
        dc      Disconnect from remote device
        sr      Switch central/peripheral role
        cpt     Change connection packet type
        rssi    Display connection RSSI
        lq      Display link quality
        tpl     Display transmit power level
        afh     Display AFH channel map
        lp      Set/display link policy settings
        lst     Set/display link supervision timeout
        auth    Request authentication
        enc     Set connection encryption
        key     Change connection link key
        clkoff  Read clock offset
        clock   Read local or remote clock
        lescan  Start LE scan
        leinfo  Get LE remote information
        lealadd Add device to LE Accept List
        lealrm  Remove device from LE Accept List
        lealsz  Read size of LE Accept List
        lealclr Clear LE Accept List
        lewladd Deprecated. Use lealadd instead.
        lewlrm  Deprecated. Use lealrm instead.
        lewlsz  Deprecated. Use lealsz instead.
        lewlclr Deprecated. Use lealclr instead.
        lerladd Add device to LE Resolving List
        lerlrm  Remove device from LE Resolving List
        lerlclr Clear LE Resolving List
        lerlsz  Read size of LE Resolving List
        lerlon  Enable LE Address Resolution
        lerloff Disable LE Address Resolution
        lecc    Create a LE Connection
        ledc    Disconnect a LE Connection
        lecup   LE Connection Update

For more information on the usage of each command use:
        hcitool <command> --help
root@Rockchip:/etc/init.d$ hcitool  -i hci0 rssi
rssi: too few arguments (minimal: 1)
Usage:
        rssi <bdaddr>
root@Rockchip:/etc/init.d$ hcitool  -i hci0 dev
Devices:
        hci0    FC:23:CD:57:98:F4
root@Rockchip:/etc/init.d$ hcitool  -i hci0 san
Unknown command - "san"
root@Rockchip:/etc/init.d$ hcitool  -i hci0 scan
Scanning ...
root@Rockchip:/etc/init.d$ hcitool  -i hci0 scan
Scanning ...

root@Rockchip:/etc/init.d$ hcitool  -i hci0 info
info: too few arguments (minimal: 1)
Usage:
        info <bdaddr>
root@Rockchip:/etc/init.d$ hcitool dev
Devices:
        hci0    FC:23:CD:57:98:F4
root@Rockchip:/etc/init.d$ hcitool  -i hci0 info FC:23:CD:57:98:F4
Requesting information ...
Can't create connection: Input/output error
root@Rockchip:/etc/init.d$ hcitool  -i hci0 info 1C:3C:78:82:04:4E
Requesting information ...
Can't create connection: Input/output error
root@Rockchip:/etc/init.d$ hcitool  -i hci0 info 1C:3C:78:60:2E:E3
Requesting information ...
        BD Address:  1C:3C:78:60:2E:E3
        Device Name: 飞哥的iphone4spromaxultr
        LMP Version:  (0xd) LMP Subversion: 0xa1
        Manufacturer: Broadcom Corporation (15)
```



