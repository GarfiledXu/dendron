---
id: zpgmgtzb3luvr7rxa00wfiq
title: Scheme
desc: ''
updated: 1782219123296
created: 1781938835906
---

## 1. Agent

* **Antigravity**: 系统级 Agent，支持操作系统级别的命令执行与文件系统交互，适用于跨环境（如 Windows 宿主机与 Docker 容器）的自动化开发流。
* **Claude Code**: 终端级 (CLI) Agent，由 Anthropic 开发，侧重于在命令行环境中进行代码库检索、修改与重构任务。
* **Codex (Copilot)**: IDE 级 Agent，集成于编辑器内部，主要功能为上下文行级代码补全与侧边栏对话。

## 2. Agent Tool

* **Antigravity Manager**
  * 作用: Antigravity 的可视化管理界面与模型节点配置工具。
  * 链接: <https://github.com/Draculabo/AntigravityManager>
* Antigravity tool
  * 评价：一坨大辩
* claude code router
  * `npm install -g @musistudio/claude-code-router`
* **One-API**
  * 作用: API 聚合与路由网关。将异构大模型（如 Gemini、DeepSeek）的 API 统一转换为标准的 OpenAI/Anthropic 接口格式，用于突破特定 Agent 的模型端点限制。
* **Continue (.dev)**
  * 作用: 开源 IDE 插件。用于替代官方 Copilot 插件，支持在编辑器中自定义大模型 API 地址以接管代码补全工作。

## 3. Model

* **Gemini Pro**: 支持长上下文窗口 (Context Window)，适用于大规模代码库全局分析和长文档输入。
* **DeepSeek V4 Pro**: API 调用成本较低且推理能力稳定，适用于高并发、高频次的代码生成与测试任务。
* **GPT-5.5**: 综合指令遵循能力较强，适用于处理复杂的系统架构设计或核心逻辑重构。
* **GLM**: 国产大模型，支持本地私有化部署，中文技术文档处理能力较好。
* **Kimi**: 国产大模型，支持超长上下文，适用于处理大容量日志分析或 API 文档解析。

## 4. Scheme

* [x] Antigravity (Windows) + Antigravity Manager + Gemini Pro
* [ ] claude code 账号申请
  * [ ] Chrome (Windows)Current San Jose, California, US	Jun 21, 2026, 10:13 AM	Jun 21, 2026, 10:13 AM
  * [ ] JuYeNiaoXing
  * [ ] 7d33b3d2-a89e-41db-8931-0b61f40ced34
  * [博客园](https://www.cnblogs.com/youring2/p/19976124)
    * 修改配置文件，跳过登录
    * 配置三方模型
* [ ] Claude Code (Windows/CLI) + One-API + Gemini Pro
* [ ] Codex (通过 Continue 插件) + One-API + Gemini Pro
* [ ] Claude Code (Windows/CLI) + One-API + DeepSeek V4 Pro
* [ ] Codex (通过 Continue 插件) + One-API + DeepSeek V4 Pro

## gemini api Key

Gemini API Key
projects/980771772400
980771772400

查看api版本
curl https://generativelanguage.googleapis.com/v1beta/models?key=你的KEY