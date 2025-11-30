---
id: 0e90s1q0knwtzribfrde244
title: Register
desc: ''
updated: 1762782276036
created: 1762744573339
---

## 构建私有镜像仓库，用于共享镜像

### 流程思路

1. 局域网搭建一个register，A机push镜像->B机pull镜像

### 实操

创建register

```bash
#-v <宿主机目录>:<容器内目录>
# /var/lib/registry
# → 是 Docker Registry 容器内 默认的数据存放位置（保存所有镜像层、manifest、tag 信息）
# registry:2 本质上就是一个轻量的 镜像仓库服务器进程
docker run -d \
  --name my_registry \
  --restart=always \
  -p 5000:5000 \
  -v /home/xjf1127/codespace/docker/register/data:/var/lib/registry \
  registry:2
```

验证运行

```bash
docker ps
```

确认服务是否响应

```bash
curl http://localhost:5000/v2/
```

启动web管理界面

```bash
docker run -d \
  --name registry-ui \
  --restart=always \
  -p 8080:80 \
  -e REGISTRY_TITLE="Local Docker Registry" \
  -e REGISTRY_URL="http://localhost:5000" \
  joxit/docker-registry-ui:static
# 映射宿主机 8080 端口为 Web UI 访问端口
# Web UI 内部访问的 registry 服务 URL
# 轻量级前端镜像，支持静态部署
```

浏览器访问验证

```bash
http://localhost:8080
```

配置wsl ip

```bash
root@MARK-II:/home/xjf1127/codespace/docker/register/data# ip addr show eth0 | grep inet
    inet 172.23.109.38/20 brd 172.23.111.255 scope global eth0
    inet6 fe80::215:5dff:fe9e:7346/64 scope link 
```

配置docker

```bash
## 为了让客户端对http可访问，默认docker客户端只对https进行访问
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<EOF
{
  "insecure-registries": ["172.23.109.38:5000"]
}
EOF

# 重启docker
# sudo systemctl restart docker
```

测试连通性

```bash
curl http://172.23.109.38:5000/v2/
```

registry 操作
将镜像上传到registry

```bash
# 查看镜像
docker images
# REPOSITORY   TAG       IMAGE ID       SIZE
# ubuntu       22.04     a2abf6c4d29d   72MB
# 打标签
docker tag ubuntu:22.04 172.23.109.38:5000/ubuntu:22.04

#推送
docker push 172.23.109.38:5000/ubuntu:22.04
```

从registry拉取镜像

```bash
# 拉取
docker pull 172.29.196.23:5000/ubuntu:22.04
# 可行性验证
docker run --rm 172.29.196.23:5000/ubuntu:22.04 cat /etc/os-release
```
