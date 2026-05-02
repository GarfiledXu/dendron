---
id: uvvmgdfmjsfzjsk1188epko
title: 免密
desc: ''
updated: 1776307000755
created: 1776306801379
---

# VSCode 通过 SSH 免密登录操作指南

## 一、目标

在本地使用 VSCode 连接远程服务器时，实现 **无需输入密码自动登录**。

---

## 二、原理说明

SSH 免密登录基于「公钥认证」机制：

- 本地生成一对密钥：
  - 私钥（保存在本地）
  - 公钥（放到服务器）
- 登录时：
  - 本地用私钥证明身份
  - 服务器用公钥验证
- 验证成功后无需密码

---

## 三、操作步骤

### 1. 生成 SSH 密钥（本地执行）

```bash
ssh-keygen -t ed25519
```

说明：

- 一路回车即可
- 默认生成在：

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

---

### 2. 上传公钥到服务器

#### 方法一（推荐）

```bash
ssh-copy-id user@服务器IP
```

输入一次密码即可

---

#### 方法二（手动）

查看公钥：

```bash
cat ~/.ssh/id_ed25519.pub
```

复制输出内容，然后登录服务器执行：

```bash
mkdir -p ~/.ssh
vim ~/.ssh/authorized_keys
```

将公钥粘贴进去并保存

---

### 3. 设置服务器权限（必须）

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

### 4. 测试免密登录

```bash
ssh user@服务器IP
```

如果无需输入密码，则成功

---

## 四、VSCode 配置

### 1. 安装 Remote SSH 插件

在 VSCode 插件市场搜索：

- Remote - SSH

---

### 2. 配置 SSH

编辑本地文件：

```bash
~/.ssh/config
```

添加：

```text
Host myserver
    HostName 192.168.1.100
    User root
    IdentityFile ~/.ssh/id_ed25519
```

---

### 3. 连接服务器

在 VSCode：

- 左下角绿色按钮
- 选择：Connect to Host
- 选择：myserver

---

## 五、常见问题

### 1. 仍然要求输入密码

可能原因：

#### 权限错误

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

#### SSH 服务未开启公钥认证

检查服务器：

```bash
vim /etc/ssh/sshd_config
```

确保：

```text
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```

重启服务：

```bash
sudo systemctl restart ssh
```

---

### 2. VSCode 连接仍需密码

原因：

VSCode 没有使用指定私钥

解决：

```text
IdentityFile ~/.ssh/id_ed25519
```

必须配置

---

### 3. Windows 用户注意

路径写法：

```text
IdentityFile C:\Users\用户名\.ssh\id_ed25519
```

注意：

- 不要使用 ~
- 使用完整路径

---

### 4. 多个密钥冲突

解决：

```text
IdentitiesOnly yes
```

---

## 六、进阶优化

### 1. 使用 ssh-agent（可选）

避免私钥密码输入：

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

---

### 2. 保持连接稳定

```text
Host *
    ServerAliveInterval 60
    TCPKeepAlive yes
```

---

## 七、推荐实践

建议为不同服务器配置不同 Host：

```text
Host build-server
    HostName 192.168.1.10
    User user
    IdentityFile ~/.ssh/id_build

Host test-device
    HostName 192.168.1.20
    User root
    IdentityFile ~/.ssh/id_test
```

---

## 八、总结

免密登录核心步骤：

1. 本地生成密钥
2. 公钥上传到服务器
3. 配置 SSH
4. VSCode 使用私钥连接

完成后即可实现无密码登录