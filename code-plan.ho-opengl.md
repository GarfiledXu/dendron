---
id: xc897ax604mgh65f4tn8bze
title: Ho Opengl
desc: ''
updated: 1691304773232
created: 1691301205837
---

## gl code plan
author: [^1]
[^1]: xjf2613~2023/08/06~
------

### windows 
-----
#### target
1. **understand gl machanism**
2. verify render effect
3. as a fast effect simulation platform

#### complete step
- [x] triangle render
- [ ] texture render 
- [ ] basic graphic drawing
- [ ] mutliple texture and graphic drawing
- [ ] yuv data color format convert
- [ ] basic image transform
- [ ] device screen env simulation

#### failure preparing
- GLSL basic grammer must be passed once
- basic yuv color formatting must be passed once
- image transform and device screen can be delay
  
### android
-----

#### tagert 
1. for mastering basic flow of embedded gl
2. native layer program
3. application layer directly program
4. native and application layer mix-programming

#### complete step
- [ ] application render program
- [ ] native render program
- [ ] mix program by jni

#### failure preparing
- time limit
- priority process application render program

### qnx700
-----
1. mater qnx native graphics interface
2. native opengl program

#### complete step
- [ ] native render program

### web gl
-------
1. fast shader program by online

### linux
-------
1. cross platform program

