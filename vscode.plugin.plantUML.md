---
id: ko1odclmzykwj5t5cfyu8lq
title: plantUML
desc: ''
updated: 1740455285105
created: 1740452716868
---

### install issume

安装`PlantUML`插件之后,进行preview，并不能工作

### ref

- [stackflow](https://stackoverflow.com/questions/70039939/how-to-use-the-plantuml-preview-in-vscode)
- [blog](https://hittheroad.dev/using-plantuml-inside-vscode-and-wsl2-2a4e3b2b0898)
- [using tutorial](https://plantuml.com/zh/sequence-diagram)

### resolve

```bash
### linux
sudo apt install openjdk-17-jdk -y
sudo apt install graphviz
### reload windows

### windows
https://adoptium.net/zh-CN/download/
https://graphviz.org/download/
```

### 时序图基本功能

1. 声明 默认矩形参与者 `participant`
2. 声明 其他形状参与者

   ```bash
      @startuml
      participant Participant as Foo
      actor       Actor       as Foo1
      boundary    Boundary    as Foo2
      control     Control     as Foo3
      entity      Entity      as Foo4
      database    Database    as Foo5
      collections Collections as Foo6
      queue       Queue       as Foo7
      Foo -> Foo1 : To actor 
      Foo -> Foo2 : To boundary
      Foo -> Foo3 : To control
      Foo -> Foo4 : To entity
      Foo -> Foo5 : To database
      Foo -> Foo6 : To collections
      Foo -> Foo7: To queue
      @enduml
   ```

3. 

