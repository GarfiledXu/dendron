---
id: ljudrw7uf97zgtduogrzury
title: '20'
desc: ''
updated: 1716172395322
created: 1716167482650
---

#### must
- [x] 补充日报，周报
- [ ] 开始封装 重汽avm sdk
  - [ ] 检查原接口的复杂性
    - [ ] 减少结构体封装
  - [ ] java 层基类: 目的: 实现一个测试子类 来脱离实际sdk 进行测试
  - [ ] 封装 init param 部分的接口，改为 cfg + init，不将param 参数暴露到 java层，直接使用配置文件
  - [ ] 其余的暂时保持接口原特色, java 层使用包含long成员的对象 来存储c指针
- [ ] 志鹏 设备问题
  - [ ] 255
  - [ ] 142