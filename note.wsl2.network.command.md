---
id: hvatwkwvxu7ig6ysbsfxr67
title: Command
desc: ''
updated: 1762988122490
created: 1762920716428
---

## command manual

### ip query

wsl 获取虚拟网卡网关地址`172.23.96.1`，以及wsl本机虚拟网卡上的地址`172.23.109.38`，虚拟网卡网段`172.23.96.0/20`

```bash
root@MARK-II:/home/xjf1127/codespace/docker/register/data# ip route
default via 172.23.96.1 dev eth0 proto kernel 
172.23.96.0/20 dev eth0 proto kernel scope link src 172.23.109.38 

root@MARK-II:/home/xjf1127/codespace/docker/register/data# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.23.109.38  netmask 255.255.240.0  broadcast 172.23.111.255
        inet6 fe80::215:5dff:fe9e:7346  prefixlen 64  scopeid 0x20<link>
        ether 00:15:5d:9e:73:46  txqueuelen 1000  (Ethernet)
        RX packets 13990  bytes 14477455 (14.4 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 7524  bytes 652256 (652.2 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

win 查询wsl在虚拟网卡上的地址 `172.23.109.38`

```bash
C:\Users\13178>wsl hostname -I
wsl: 检测到 localhost 代理配置，但未镜像到 WSL。NAT 模式下的 WSL 不支持 localhost 代理。
172.23.109.38
```

**补充说明**
        1. via 172.23.96.1：表示下一跳网关是 172.23.96.1。
        👉 这个地址就是 宿主机在 WSL 虚拟网段上的 IP，也可以理解为 WSL 的“默认网关”。
        所有从 WSL 发出的外部访问（比如访问互联网、访问宿主机外部网络）都要经由它。
        2. 172.23.96.0/20：这是 WSL 内部虚拟网络的子网（子网掩码为 255.255.240.0）。
        子网范围大概是 172.23.96.0 ~ 172.23.111.255。
        3. src 172.23.109.38：这是 WSL 虚拟机（也就是你当前的 Linux 实例）在该网段上的 IP 地址

### win port proxy

1. 注册

    ```bash
        netsh interface portproxy add v4tov4 listenport=<宿主机监听端口> listenaddress=<宿主机监听IP> connectport=<目标端口> connectaddress=<目标IP>
        # netsh interface portproxy add v4tov4 listenport=8081 listenaddress=0.0.0.0 connectport=8080 connectaddress=172.23.109.38
        # 将宿主机 8081 端口转发到 WSL2 8080
    ```

2. 删除

    ```bash
        netsh interface portproxy delete v4tov4 listenport=<端口> listenaddress=<IP>
        <!-- netsh interface portproxy delete v4tov4 listenport=8081 listenaddress=0.0.0.0 -->
    ```

3. 清空所有代理

    ```bash
        netsh interface portproxy reset
    ```

4. 查看代理状态

    ```bash
        netstat -ano | findstr :8081
    ```

### 完整使用场景

#### 局域网下，电脑A-wsl2 启动docker registry服务，电脑B-wsl2 进行服务访问

1. client:
   1. 通过某个途径，将loclhost某端口的数据转发到局域网其他ip中，启动代理
   2. 获取wsl虚拟网卡网关地址，以此地址和目标端口进行连接创建
   3. 在wsl中运行client，将步骤2的地址和端口作为目标地址
2. server:
   1. 预设好宿主机转发的端口，以及wsl接收的端口，启动代理
   2. 在wsl中运行server，监听虚拟网卡地址，以此地址和端口进行监听
