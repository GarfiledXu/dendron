---
id: qzezxqhcfckl5zg0rx4wzui
title: Command
desc: ''
updated: 1741837677365
created: 1734586790735
---

#### su

```bash
#第一次切换为root用户，需要设置root密码
sudo passwd root
su
#切换为xjf1127用户
su -- xjf1127
```

#### 利用find命令搜索文件，并且罗列文件权限和属性

```bash
## 查找所有包含librga的文件或者路径 并且罗列权限
sudo find ./target_folder -name "*librga*" -exec ls -l {} +

## 查找所有包含librga的文件或者路径 并且罗列属性
sudo find ./target_folder -name "*librga*" -exec file {} +

## 忽略大小写, name参数前添加一个i
sudo find ./ -iname "*rtsp*.a*" -exec ls -l {} +

```

#### 利用du命令，查看一个目录层级下所有子目录的占用空间

```bash
sudo du -sh ./target_folder/*
```

#### 多个子文件夹处理时压缩命令

```bash
#target 命令参数解释
-c：创建新的压缩包。
-z：使用 gzip 压缩，获得文件后缀 .tar.gz
-j: 使用bzip2 压缩，获得文件后缀 .tar.bz2
-v：显示压缩过程。
-f：指定输出文件名。
-C: 大C相当于切换目录为当前目录
## 将多个子文件夹添加到一个压缩包中
tar -czvf output.tar.gz -C /path/to/parent dir1 dir2 dir3
tar -cjvf output.tar.gz -C /path/to/parent dir1 dir2 dir3
## -C 是切换父目录，默认命令的最后部分是需要压缩的文件夹 list

## 将多个子文件夹分别进行压缩，添加到与自己名称对应的压缩包中
## 需要指定文件名称，一个一个来
```

#### 如何定期执行命令: 使用watch 命令来持续刷新观察数据

```bash
## 当系统在执行压缩命令时，通过另一个终端使用wtach命令来观察文件压缩大小的变化
watch -n 1 ls -l
## -n 1 表示1秒定隔时间
## ls -l 则为要执行的命令

```

#### 查看可执行文件对应的库依赖

```bash
ldd ./<exe>
```

#### grep时如何忽略大小写

```bash
grep -i "heyword"
```

#### 查找shell环境变量

```bash
env
```

#### 关于shell的环境变量

- 变量类型
    1. 用户全局变量
    2. 系统全局变量
    3. shell环境变量
    4. 普通shell变量
    5. 局部变量
    6. shell预定义变量
    7. 定义时使用了 `readonly`修饰的变量
**本质上都是 `shell` 变量，不同类型决定了变量的`可见性和传递性`，环境变量意味着 可通过父子关系传递给子shell，而普通变量不会传递，用户全局则该用户下所有shell进程可见，全局环境变量则系统下所有shell进程可见**
- 设置规则
  1. 设置临时环境变量

        ```bash
        export QT_PLUGIN_PATH="/usr/lib/qt/plugins"
        ```

  2. 设置持久化环境变量

        ```bash
        写入 `~/.bashrc` 或 `~/.profile` | `echo 'export MY_VAR="hello"' >> ~/.bashrc`
        ```

- 加载规则: 所有类型变量的加载顺序和覆盖规则
Shell 启动时自动设置的变量，具有最高优先级，无法手动覆盖

    ```bash
    1. 只读变量（Read-only Variables）
    **`$SHELL`**: 当前 Shell 的路径。
    **`$0`**: 当前脚本或 Shell 的名称。
    **`$$`**: 当前 Shell 的进程 ID (PID)。
    **`$PPID`**: 父进程的进程 ID。
    **`$UID`**: 当前用户的用户 ID。
    **`$EUID`**: 当前用户的有效用户 ID。
    **`$RANDOM`**: 每次引用时生成一个随机数。
    **`$SECONDS`**: 脚本或 Shell 运行的秒数。

    2. 环境变量（Environment Variables）
    **`$PATH`**: 命令搜索路径。
    **`$HOME`**: 当前用户的主目录。
    **`$PWD`**: 当前工作目录。
    **`$OLDPWD`**: 上一个工作目录。
    **`$IFS`**: 内部字段分隔符，用于分词。
    **`$PS1`**: 主提示符字符串。
    **`$PS2`**: 次提示符字符串。

    3. 特殊变量（Special Variables）
    **`$?`**: 上一个命令的退出状态。
    **`$!`**: 最后一个后台进程的 PID。
    **`$_`**: 上一个命令的最后一个参数。
    **`$#`**: 传递给脚本或函数的参数个数。
    **`$*`**: 所有传递给脚本或函数的参数。
    **`$@`**: 所有传递给脚本或函数的参数，每个参数作为独立的字符串。

    4. Shell 内置变量（Shell Built-in Variables）
    **`$BASH`**: Bash Shell 的路径。
    **`$BASH_VERSION`**: Bash 的版本信息。
    **`$SHLVL`**: Shell 嵌套层级。
    **`$LINENO`**: 当前脚本中的行号。

    5. 系统级环境变量（System-level Environment Variables）
    **`$LANG`**: 当前的语言环境。
    **`$TERM`**: 终端类型。
    **`$DISPLAY`**: X11 显示设置。
    **`$TZ`**: 时区设置。

    **只读变量** 和 **特殊变量** 通常无法被覆盖。
    **环境变量** 和 **Shell 内置变量** 虽然可以被修改，但在某些情况下会被 Shell 或系统重新设置。
    **系统级环境变量** 通常由系统管理，用户无法轻易覆盖。
    ```


    ```bash
    变量加载顺序：系统全局 (/etc/environment) < 用户级 (~/.profile, ~/.bashrc) < Shell 变量 < 运行时变量。
    变量覆盖规则：后加载的变量会覆盖前面的变量，export 变量会影响子进程。
    Shell 变量 vs 环境变量：普通变量不会继承给子进程，环境变量 (export) 可继承。
    source vs bash：
    source 不会创建子进程，变量影响当前 Shell。
    bash 会创建子进程，变量仅在子进程内生效。
    ```

- 什么行为创建的shell进程会是归属为子进程?哪些行为创建的shell进程属于独立进程?
        fork exec后的子进程
        system
        shell执行外部命令创建的进程
- 常见shell执行命令方式
        command 普通执行，首先解析命令内容，区分命令类型，`内建命令`和`外部命令`，`内建命令`即shell当前进程解析后直接输入输出，`外部命令`则是执行外部的exe程序，先fork进程，然后exec替换进程内存数据，主shell等待子进程返回
        (command) 进入shell前就会创建子进程再执行命令
        $(command) 常用于命令替换，先执行括号内命令以后返回结果再与当前所在位置组合为命令,至于是否会创建子进程来执行要看具体shell解释器
        bash script.sh 会创建子进程执行
        source script.sh 不会创建子进程执行，这个命令通常用来给当前shell环境加载变量
- 变量信息查询的方式

```bash

```


- other
  1. 普通变量和环境变量以及持久化的变量以及全局持久化变量的区别
  2. shell以及其子shell的应用场景，什么操作会创建shell进程，并且哪些操作shell进程间会存在继承关系
  3. shell 用户环境和全局环境，包括持久化环境变量的区别
  4. env，printenv命令
  5. `source`运行脚本和用`bash`或者`sh`运行脚本有什么区别？普通变量和环境变量能否同名，引用时方式是否有不同? 当一个shell进程被创建时，按什么规则依次加载变量?

    **1. 普通变量、环境变量、持久化变量、全局持久化变量的区别**

    | **变量类型**      | **是否继承到子进程** | **作用范围**              | **持久化方式**                      | **示例**                        |
    |-----------------|------------------|----------------------|------------------------------|-------------------------------|
    | **普通变量**     | ❌ **不会继承**    | 仅当前 shell 会话       | 关闭 shell 后失效              | `MY_VAR="hello"`               |
    | **环境变量**     | ✅ **会继承**      | 当前 shell 及其子进程    | 关闭 shell 后失效              | `export MY_VAR="hello"`        |
    | **持久化变量**   | ✅ **会继承**      | 仅当前用户的 shell 进程  | 写入 `~/.bashrc` 或 `~/.profile` | `echo 'export MY_VAR="hello"' >> ~/.bashrc` |
    | **全局持久化变量** | ✅ **所有用户继承**  | 所有用户的 shell 进程    | 写入 `/etc/environment`       | `echo 'MY_VAR="hello"' | sudo tee -a /etc/environment` |

    ---

    **2. Shell 及其子 Shell 的应用场景**
    **🔹 什么操作会创建 Shell 进程？**

    | **操作**                        | **是否创建新 Shell 进程？** |
    |--------------------------------|----------------------|
    | 打开新的终端窗口 (`gnome-terminal`, `konsole`, `xterm` 等) | ✅ **是** |
    | 在终端输入 `bash` 或 `sh`        | ✅ **是** |
    | 运行 `script.sh` 但未使用 `source` | ✅ **是** |
    | 使用 `source script.sh` 加载脚本 | ❌ **否** |
    | 在 `bash` 里运行 `python` 或 `vim` | ❌ **否**（不会创建新的 shell，只是运行程序） |
    | 执行 `ssh user@server` 远程登录   | ✅ **是** |
    | 在终端输入 `sudo -i` 或 `su -`     | ✅ **是** |

    ---

    **3. Shell 进程之间的继承关系**
    **子 Shell 会继承父 Shell 的环境变量，但不会继承普通变量。**
    **普通变量** 在当前 Shell 进程有效，但不会传递到子进程。
    **环境变量**（通过 `export` 设置）可以传递给子进程。

    ```sh
    VAR1="local"   # 普通变量，仅当前 shell 有效
    export VAR2="global"  # 环境变量，可继承到子 shell

    bash           # 进入子 shell
    echo $VAR1     # 无输出，VAR1 不继承
    echo $VAR2     # 输出 global，VAR2 被继承
    exit           # 退出子 shell
    ```

    **️4. `source` 运行脚本 vs `bash/sh` 运行脚本**

    | **方式**     | **是否创建新 Shell 进程？** | **作用范围** | **变量继承** | **示例** |
    |------------|------------------|----------|----------|--------|
    | `source script.sh` | ❌ **不会创建新 Shell** | 影响当前 Shell | **直接修改当前 Shell 的变量** | `source myscript.sh` |
    | `bash script.sh`   | ✅ **会创建新的 Shell** | 仅限于新 Shell | **不会修改当前 Shell 的变量** | `bash myscript.sh` |
    | `sh script.sh`     | ✅ **会创建新的 Shell** | 仅限于新 Shell | **不会修改当前 Shell 的变量** | `sh myscript.sh` |

     **📌 关键区别**
    1. **`source` 运行脚本时**
     不创建新的 Shell 进程，而是 **在当前 Shell 里执行**，所有定义的变量会影响当前 Shell。
     **适用于加载环境变量**，例如 `.bashrc` 或 `.profile`。
     **示例**

        ```sh
        MY_VAR="before"
        source myscript.sh
        echo $MY_VAR   # 可能被 myscript.sh 修改
        ```

    2. **`bash` / `sh` 运行脚本时**
     **创建一个新的子 Shell** 进程，并在该 Shell 里执行脚本。
     **父 Shell 变量不会影响子 Shell**，脚本修改的变量 **不会影响父 Shell**。
     **示例**

        ```sh
        MY_VAR="before"
        bash myscript.sh
        echo $MY_VAR   # 仍然是 "before"，不会受 myscript.sh 影响
        ```

    3. **️普通变量和环境变量是否能同名？引用方式是否有不同？**
     **✅ 普通变量和环境变量可以同名**
     **同名变量时**，普通变量的作用范围仅限当前 Shell，环境变量则可以传递给子进程。
     **引用方式相同**，都是用 `$变量名`。

     **🔹 示例**

    ```sh
    MY_VAR="local"   # 普通变量
    export MY_VAR="global"   # 变成环境变量
    echo $MY_VAR   # 输出 global


### 通过strace来观察程序行为

**eg:**

  ```bash
  ### 调试查找 qtchooser的硬编码路径
  strace -e openat qtchooser -print-env
  openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
  openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libstdc++.so.6", O_RDONLY|O_CLOEXEC) = 3
  openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libgcc_s.so.1", O_RDONLY|O_CLOEXEC) = 3
  openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
  openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libm.so.6", O_RDONLY|O_CLOEXEC) = 3
  openat(AT_FDCWD, "/home/xjf1127/.config/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/etc/xdg/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = -1 ENOENT (No such file or directory)
  openat(AT_FDCWD, "/usr/share/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
  openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
  openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/qt-default/qtchooser/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
  openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/qt-default/qtchooser//default.conf", O_RDONLY) = 4
  QT_SELECT="default"
  QTTOOLDIR="/usr/lib/qt5/bin"
  QTLIBDIR="/usr/lib/x86_64-linux-gnu"
  +++ exited with 0 +++
  ### 可以看出 qtchooser 的库加载流程，以及目录查找流程
  ```

### 打开一个终端和shell进程的关系
