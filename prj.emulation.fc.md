---
id: 1hz1xwwhgbod5tjy6spypz3
title: Fc
desc: ''
updated: 1770625857488
created: 1766238702272
---

## route map

1. 简要了解计算机组成原理
   1. [[prj.emulation.fc.principles_of_computer_organization]]
2. 梳理fc所有的硬件模块，以及硬件架构
3. 掌握汇编基础，以及6502汇编的应用编写
4. 学习fc的软件架构
5. 整理模拟器实现要点
6. 阅读已实现的模拟器项目
7. 开始编程模拟器
8. 模拟器测试思路和方法整理
9. 项目闭环

## refe

- [nesdev wiki](https://www.nesdev.org/wiki/Nesdev_Wiki)
- [github: 6502芯片可视化模拟](https://github.com/trebonian/visual6502)
- [visual 6502: 在线可视化](https://visual6502.org/JSSim/expert.html)
- [easy 6502: 在线汇编](https://skilldrick.github.io/easy6502/)
- [FCEUX](https://fceux.com/web/home.html)
- [cc65: 包含6502的c编译器和汇编，可用于对照学习](https://cc65.github.io/)
- [godbolt: 在线编译器，用于汇编和高级语言的映射对照](https://godbolt.org/)
- [blog: 任天堂娱乐系统（NES）架构](https://www.copetti.org/zh-hans/writings/consoles/nes/)
- [pdf: 6502微处理器概述](https://d1.amobbs.com/bbs_upload782111/files_32/ourdev_575707.pdf)
- [blog: cpu是如何通过总线读取总线的](https://www.cnblogs.com/cdaniu/p/15605828.html)

## book

- 6502 Instruction Set — Quick Reference
- Programming the 6502（在线 PDF 版本很多）
- 6502 assembly language programming (Lance A. Leventhal) (Z-Library)
- 6502 Assembly Language Subroutines (Lance A. Leventhal, Winthrop Saville) (Z-Library)
- 6502 Software Design (Leo J. Scanlon) (Z-Library)
- 6502汇编语言 (霍元斌编著, 霍元斌编著, 霍元斌) (Z-Library)
- 6502微处理机及其应用 (荣树熙，张开敬编) (Z-Library)
- 6502微电脑游戏 (陈丁山译) (Z-Library)
- 6502系统程式 (北方电脑公司信息资料部编) (Z-Library)
- 6502组合语言与系统程式 (杨洪生，张祥庆等编译) (Z-Library)
- Assembly Language Step-by-Step Programming with Linux (Jeff Duntemann) (Z-Library)
- Assembly Programming and Computer Architecture for Software Engineers (Brian Hall, Kevin Slonka) (Z-Library)
- NES Architecture More Than a 6502 Machine (Rodrigo Copetti) (Z-Library)
  - [离线下载异常，替代的在线阅读版本](https://reader.z-library.sk/read/1114eac44b354193259e1baa2ce64de4b5a8e8561119099968e2080928ec9932/26586154/bbeec2/nes-architecture-more-than-a-6502-machine.html?client_key=1fFLi67gBrNRP1j1iPy1&extension=azw3&signature=50225b0c4de15d599b2196d44a3e807fe9825850f55eada2531603bde4257479&download_location=https%3A%2F%2Fz-library.sk%2Fdl%2F26586154%2Fe3e1cc)
- NESFamicom A Visual Compendium (Bitmap Books) (Z-Library)
- The Art of ARM Assembly, Volume 1 64-Bit ARM Machine Organization and Programming (Randall Hyde) (Z-Library)
- The Art of Assembly Language (Randall Hyde) (Z-Library)
- Understanding Digital Computers (Forrest M. Mims III) (Z-Library)
- 汇编程序设计与计算机体系结构 软件工程师教程 Assembly Programming and Computer Architecture for Software Engineers(Chinese Edition) (Brian R. Hall, Kevin J. Slonka) (Z-Library)
- 计算机系统解密 从理解计算机到编写高效代码（有助于提高整个系统质量的知识图谱，包括硬件、组合逻辑、时序逻辑、计算机体系结构与组成原理... (Z-Library)

## plan

[[plan.learn.roadmap.2025.12]]

## glossary

1. NTSC:
   1. 全称：National Television System Committee，国家电视系统委员会制式
   2. 定义了 帧率（≈60Hz） / 扫描线（262） / 颜色副载波
2. PAL:
   1. phase alternating line 逐行倒相制
   2. 欧洲电视制式（50Hz）
3. RF
   1. radio frequency 射频信号
4. CVBS
   1. composite video，blanking and sync 复合电视信号
5. FC
   1. family computer
   2. 日本名称
6. NES
   1. nintendo entertainment system 任天堂娱乐系统
   2. 北美地区名称
7. CPU: central process unit
8. PPU: picture process unit
9. APU: audio process unit
10. ROM: read-only memory
11. RAM: random access memory
12. Mapper: memory mapper 存储映射芯片
13. VRC: Virtual ROM Controller 虚拟rom控制器
14. FDS: family disk system 红白磁碟机
15. IRQ: interrupt request
16. DMA: direct memory access 直接内存访问
17. DPCM: 差分脉冲编码调制 作用：NES 中播放采样音效

## 北美日韩国家和中国英德国家红白机硬件的差异

本质源于电视制式的差异，前者NTSC后者PAL，电视制式决定了电视信号的时序和色编码要求，进而决定了主机本身需要采用的晶振频率。
主机晶振作为全系统时钟源，经由ppu分频生成cpu时钟，因此，凡是以来该时钟的器件，如cpu，ppu，apu，都需要采用对应制式的硬件版本

？是这些硬件元器件无法做到多版本时钟频率的兼容吗，还是说有其他东西要考虑
要做到一芯多频的兼容，从技术原理上可以做到，但从芯片本身的复杂度以及成本考量完全不需要，相比之下更换一整套芯片版本逻辑复杂度，验证测试风险，制作成本有更大优势

## 2A03（NTSC）与 2A07（PAL）

1. 共同点：cpu核心部分，指令集相同
2. 差异点：芯片主频差异（6502内部cpu和apu共享一套时钟），APU 内部定时常量 + 帧计数器 / DPCM 速率表

## cpu assembly

[[prj.emulation.fc.6502_assembly]]
