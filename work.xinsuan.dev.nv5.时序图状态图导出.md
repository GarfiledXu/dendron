---
id: od5suf20puibog42eadijti
title: 时序图状态图导出
desc: ''
updated: 1774834276644
created: 1774833860318
---

## 时序图

### 导出时序
![picture 3](images/c19d7828ee13eced753545263290077356a11d0ae0fd433e251391d729b8afc1.png)  

### 导入时序
![picture 4](images/77f1348e4b1e2ae1a5a15805db710fb447e1f7507389598e5e0ea114e9489a1f.png)  

### 导出时，上位机状态
![picture 2](images/6ac8b89d84a2ece2743d8dad463a362476eb8f2b4432b44c237f95998b3be3ce.png)  

### 导出时，下位机状态
![picture 1](images/ce7936a78ab98f5d9928573fcb8b3afacc49670c81d1d4406eacfe97f8d7d5f5.png)  

### 导入时，上位机状态
![picture 0](images/8428425ab9acae0a5025c3c8aeadbd4ae7e1c079a8f8d5eea7a741e4bdd35465.png)  

### 导入时，下位机状态
![alt text](image-7.png)

```bash
# 简概
+-------------------+
| File Header        |  <- 固定长度
|-------------------|
| Data Unit 1        |  <- UnitHeader + payload (主控/原图/缩略图等)
| Data Unit 2        |  
| Data Unit 3        |  
| ...                |
|-------------------|
| Index Table        |  <- 支持随机访问
+-------------------+
```
![alt text](work.xinsuan.dev.nv5.时序图状态图导出-1.png)

