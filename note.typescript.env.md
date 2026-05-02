---
id: 2zskf55ox853ffrbgg4xygk
title: Env
desc: ''
updated: 1777452099374
created: 1777362228374
---

## nvm npm pnpm node.js 的关联

* nvm: node version manager，这里的管理包括node.js的安装，切换，卸载， 与之相对的，就是直装官方的node.js exe
  * [doc](https://www.nvmnode.com/guide/introduction.html)
  * 需要用powershell，管理员方式打开，使用命令行进行配置
* node.js js的运行环境，nvm管理的实体单位
* npm: node package manager, 包即程序依赖库，是高度依赖于 运行环境版本的，相当于c++动态库依赖于编译器，安装和使用方式?
  * npm默认在nvm选择了node.js后就支持，但存在命令不存在的情况

    ```bash
    <!-- PowerShell 的执行策略（Execution Policy）把 .ps1 脚本拦住了。 -->
    <!-- npm 在 Windows 下其实是个 npm.ps1 -->
        PS C:\Windows\system32> nvm current
        v24.15.0
        PS C:\Windows\system32> npm -v
        npm : 无法加载文件 C:\nvm4w\nodejs\npm.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fw
        link/?LinkID=135170 中的 about_Execution_Policies。
        所在位置 行:1 字符: 1
        + npm -v
        + ~~~
            + CategoryInfo          : SecurityError: (:) []，PSSecurityException
            + FullyQualifiedErrorId : UnauthorizedAccess
        PS C:\Windows\system32> Set-ExecutionPolicy -Scope CurrentUser RemoteSigned

        执行策略更改
        执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 https:/go.microsoft.com/fwlink/?LinkID=135170
        中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
        [Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“N”): y
        PS C:\Windows\system32> npm -v
        11.12.1
    
    ```
  
* pnpm: 加强版本的npm
  * 安装方式：`PS C:\Windows\system32> npm install -g pnpm`
  * 验证指令: `pnpm -v`

## 工程创建