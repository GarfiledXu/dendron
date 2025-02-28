---
id: e0i5z2yoql27vfzm9sgb36g
title: Qt
desc: ''
updated: 1740385726100
created: 1740381610168
---

#### linux install
```bash
# donwlaod link
https://download.qt.io/new_archive/qt/5.9/5.9.0/
# download file
qt-opensource-linux-x64-5.9.0.run
# example path
/usr/lib/x86_64-linux-gnu/qt5/examples
```


#### 相关目录
```bash
/usr/lib/x86_64-linux-gnu
/usr/bin/qmake
## vscode cpp
{
  "configurations": [
    {
      "name": "linux-gcc-x64",
      "includePath": [
        "${workspaceFolder}/**",
        "/usr/include/**",
        "/usr/include/x86_64-linux-gnu/qt5/**"
      ],
      "defines": [
          "QT_CORE_LIB",
          "QT_GUI_LIB",
          "QT_WIDGETS_LIB"
      ],
      "compilerPath": "/usr/bin/gcc",
      "cStandard": "${default}",
      "cppStandard": "${default}",
      "intelliSenseMode": "linux-gcc-x64",
      "compilerArgs": [
        ""
      ]
    }
  ],
  "version": 4
}
```