---
id: x1nxlujm8w9mhgka1118i16
title: 免密排查指南
desc: ''
updated: 1778320704079
created: 1778320701977
---

# SSH 免密登录指南与实战排查记录

# 1. SSH 是什么

SSH（Secure Shell）是一种远程登录协议。

常见用途：

- 远程终端登录
- 文件传输
- 远程执行命令
- 端口转发
- 自动化部署

默认端口：

```bash
22
```

常见命令：

```bash
ssh user@ip
```

例如：

```bash
ssh admin@192.168.10.200
```

---

# 2. SSH 登录的两种主要认证方式

SSH 最常见有两种认证机制：

| 认证方式 | 说明 |
|---|---|
| 密码认证 | 输入账号密码 |
| 公钥认证（免密） | 使用密钥对认证 |

---

# 3. SSH 密码登录机制

密码登录流程：

```text
客户端 ---> 输入密码 ---> SSH服务端
```

服务端：

1. 接收用户名
2. 校验密码
3. 正确则登录

例如：

```bash
ssh admin@192.168.10.200
```

然后输入：

```text
admin@192.168.10.200's password:
```

---

# 4. SSH 免密登录机制（核心）

SSH 免密实际上不是“不认证”。

而是：

```text
使用密钥认证替代密码认证
```

---

# 5. SSH 密钥对概念

SSH 使用：

```text
公钥 + 私钥
```

机制。

---

## 私钥（Private Key）

保存于客户端。

例如：

```bash
~/.ssh/id_rsa
~/.ssh/id_ed25519
```

特点：

- 绝不能泄露
- 用于身份证明
- 客户端持有

---

## 公钥（Public Key）

保存于服务端。

例如：

```bash
authorized_keys
```

特点：

- 可以公开
- 服务端用它验证客户端身份

---

# 6. SSH 免密登录原理

流程：

```text
客户端:
    拿私钥签名

服务端:
    用公钥验证签名

验证成功:
    允许登录
```

因此：

```text
真正敏感的是私钥
```

而不是公钥。

---

# 7. SSH 默认密钥文件

客户端：

```bash
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

或者：

```bash
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

说明：

| 文件 | 作用 |
|---|---|
| id_rsa | 私钥 |
| id_rsa.pub | 公钥 |

---

# 8. 生成 SSH 密钥

推荐：

```bash
ssh-keygen -t ed25519
```

旧方案：

```bash
ssh-keygen -t rsa
```

生成后：

```bash
~/.ssh/
```

下会出现：

```text
id_ed25519
id_ed25519.pub
```

---

# 9. 服务端 authorized_keys

SSH 服务端会读取：

```bash
authorized_keys
```

文件。

里面保存：

```text
允许登录的公钥
```

例如：

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMp2Sh9VHFlT+USEoKEY840ujcjnSyeO0XmD+IgUUUMu jfxu@xssw-ubuntu
```

---

# 10. SSH 服务端如何验证

登录时：

```text
客户端:
    我有私钥

服务端:
    我看看你的公钥在不在 authorized_keys

如果在:
    给客户端随机挑战

客户端:
    用私钥签名

服务端:
    用公钥验证签名

验证通过:
    登录成功
```

---

# 11. SSH 免密常见失败原因

最常见：

| 问题 | 说明 |
|---|---|
| authorized_keys 路径错误 | SSH 没读这个文件 |
| 权限错误 | OpenSSH 会拒绝 |
| 公钥内容错误 | 格式损坏 |
| HOME 错误 | SSH 找错目录 |
| sshd_config 配置问题 | 禁用了公钥认证 |
| 用户不匹配 | key 放错用户 |
| 服务端不支持算法 | 老系统常见 |
| 文件换行错误 | CRLF 导致 |

---

# 12. 本次实际环境

客户端：

```text
Ubuntu 编译服务器
```

目标设备：

```text
嵌入式 Linux
OpenSSH_9.6
```

设备 IP：

```text
192.168.10.200
```

用户：

```text
admin
```

---

# 13. 我们的目标

实现：

```text
编译服务器自动上传程序到设备
```

即：

```bash
scp/sftp 自动上传
```

不想每次输入密码。

---

# 14. 初始现象

执行：

```bash
ssh admin@192.168.10.200
```

仍然要求输入密码。

即：

```text
免密失败
```

---

# 15. 首先确认 SSH 是否支持公钥认证

设备查看：

```bash
cat /etc/ssh/sshd_config | grep PubkeyAuthentication
```

结果：

```text
PubkeyAuthentication yes
```

说明：

```text
设备支持 SSH 公钥认证
```

---

# 16. 使用 ssh-copy-id 上传公钥

客户端执行：

```bash
ssh-copy-id admin@192.168.10.200
```

看起来成功：

```text
Number of key(s) added: 1
```

但：

```text
实际仍然需要密码
```

---

# 17. 开始深入排查

使用：

```bash
ssh -vvv admin@192.168.10.200
```

查看详细日志。

---

# 18. 关键日志

日志中：

```text
Offering public key
```

说明：

```text
客户端已经正确发送了公钥
```

但随后：

```text
Authentications that can continue: publickey,password
```

说明：

```text
服务端拒绝了公钥
```

即：

```text
问题在服务端
```

---

# 19. 检查 authorized_keys

最开始我们检查：

```bash
/root/.ssh/authorized_keys
```

内容也正确。

权限也正确：

```bash
chmod 600
```

目录权限：

```bash
chmod 700
```

但：

```text
依旧失败
```

---

# 20. 关键突破

查看 sshd_config：

```bash
grep AuthorizedKeysFile /etc/ssh/sshd_config
```

结果：

```text
AuthorizedKeysFile /oem/data/ssh/authorized_keys
```

这意味着：

```text
SSH 根本不读取 /root/.ssh/authorized_keys
```

真正读取的是：

```text
/oem/data/ssh/authorized_keys
```

---

# 21. 真正原因

问题本质：

```text
公钥放错位置了
```

不是：

- key错误
- 算法错误
- 权限错误

而是：

```text
sshd_config 自定义了 authorized_keys 路径
```

---

# 22. 为什么嵌入式 Linux 会这样设计

嵌入式设备经常：

```text
/root
```

是：

- ramfs
- tmpfs
- overlayfs

重启可能丢失。

因此厂家通常会：

```text
把 SSH key 放持久化分区
```

例如：

```text
/oem/data
```

---

# 23. 最终解决方案

将公钥复制到：

```bash
/oem/data/ssh/authorized_keys
```

---

# 24. 实际修复步骤

设备执行：

```bash
mkdir -p /oem/data/ssh
```

写入公钥：

```bash
vi /oem/data/ssh/authorized_keys
```

内容：

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMp2Sh9VHFlT+USEoKEY840ujcjnSyeO0XmD+IgUUUMu jfxu@xssw-ubuntu
```

---

# 25. 设置权限

```bash
chmod 600 /oem/data/ssh/authorized_keys
```

---

# 26. 重启 sshd

```bash
killall sshd
/bin/sshd -f /etc/ssh/sshd_config
```

或者：

```bash
/etc/init.d/sshd restart
```

---

# 27. 最终验证

客户端：

```bash
ssh admin@192.168.10.200
```

结果：

```text
直接登录
无需密码
```

说明：

```text
SSH 免密成功
```

---

# 28. 后续收益

现在：

- ssh
- sftp
- scp
- rsync
- 自动部署脚本

全部：

```text
无需密码
```

例如：

```bash
scp xs-nvs admin@192.168.10.200:/oem/app/
```

或者：

```bash
sftp admin@192.168.10.200
```

都可以直接使用。

---

# 29. SSH 免密登录的核心本质

核心其实只有一句话：

```text
服务端能用 authorized_keys 中的公钥验证客户端私钥签名
```

因此：

- 私钥在客户端
- 公钥在服务端
- 服务端必须能正确读取 authorized_keys

否则：

```text
一定失败
```

---

# 30. 本次问题总结

最终问题：

```text
AuthorizedKeysFile 被改成了 /oem/data/ssh/authorized_keys
```

导致：

```text
/root/.ssh/authorized_keys 完全没被使用
```

所以：

```text
ssh-copy-id 看似成功
实际无效
```

真正修复：

```text
将公钥写入 sshd_config 指定的 authorized_keys 文件
```

问题解决。