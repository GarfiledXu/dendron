---
id: 7qwn6ge88oqenr2027g2eyn
title: Workspace_setting_file
desc: ''
updated: 1776329390505
created: 1776329253148
---

# VSCode 在“打开根目录(~/系统目录)”场景下，为目标项目创建 Workspace + C++ 配置流程

---

# 一、目标

当前状态：

- VSCode 已打开在：`~` 或更大目录
- 目标项目：`~/projectA`

需要实现：

- 仅让 `projectA` 作为工作区
- 配置独立 C++ IntelliSense

---

# 二、创建 Workspace（在根目录环境下）

---

## Step 1：保持当前 VSCode（仍在 ~）

---

## Step 2：创建 workspace 文件（放到目标项目）

路径：

```text
~/projectA/.vscode/project.code-workspace
```

如果 `.vscode` 不存在，先创建：

```bash
mkdir -p ~/projectA/.vscode
```

---

## Step 3：写入 workspace 内容

```json
{
  "folders": [
    {
      "path": ".."
    }
  ],
  "settings": {}
}
```

---

## Step 4：打开该 workspace

在 VSCode 中：

```text
File → Open Workspace → ~/projectA/.vscode/project.code-workspace
```

---

# 三、创建 C++ 配置

---

## Step 1：创建配置文件

路径：

```text
~/projectA/.vscode/c_cpp_properties.json
```

---

## Step 2：写入内容

```json
{
  "configurations": [
    {
      "name": "default",
      "includePath": [
        "${workspaceFolder}/include",
        "${workspaceFolder}/src",
        "/usr/include"
      ],
      "defines": [
        "LINUX"
      ],
      "compilerPath": "/usr/bin/g++",
      "cppStandard": "c++17",
      "intelliSenseMode": "linux-gcc-x64"
    }
  ],
  "version": 4
}
```

---

# 四、创建 settings.json（可选）

路径：

```text
~/projectA/.vscode/settings.json
```

---

内容：

```json
{
  "files.exclude": {
    "**/build": true,
    "**/.git": true
  },
  "search.exclude": {
    "**/build": true
  }
}
```

---

# 五、关键变量说明

---

## workspaceFolder

```json
${workspaceFolder}
```

解析结果：

```text
~/projectA
```

---

原因：

```json
"path": ".."
```

- workspace 文件在 `.vscode/`
- `..` 指向上级目录（projectA）

---

# 六、最终效果

---

- VSCode 只加载 `projectA`
- IntelliSense 只作用于该目录
- 不再扫描 `~`
- C++ 提示正常

---

# 七、完整文件汇总

---

## project.code-workspace

```json
{
  "folders": [
    {
      "path": ".."
    }
  ],
  "settings": {}
}
```

---

## settings.json

```json
{
  "files.exclude": {
    "**/build": true,
    "**/.git": true
  },
  "search.exclude": {
    "**/build": true
  }
}
```

---

## c_cpp_properties.json

```json
{
  "configurations": [
    {
      "name": "default",
      "includePath": [
        "${workspaceFolder}/include",
        "${workspaceFolder}/src",
        "/usr/include"
      ],
      "defines": [
        "LINUX"
      ],
      "compilerPath": "/usr/bin/g++",
      "cppStandard": "c++17",
      "intelliSenseMode": "linux-gcc-x64"
    }
  ],
  "version": 4
}
```

---

# 完成