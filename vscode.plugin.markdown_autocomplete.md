---
id: uktrqywr059qhg7vcljqn5d
title: Markdown_autocomplete
desc: ''
updated: 1744471329787
created: 1743912849910
---

### 当前环境已经安装了所有相关插件，但似乎还是没有markdown语法的自动补全机制

1. 机制: snippet suggest 在编辑时，输入关键字会触发提示词，选定提示词后会进行文本补全
2. 插件功能
   1. markdown all in one: 主要提供`shortcut`快捷操作指令，以及`enter` `space` 后编号生成，符号补全等，`auto-complation`功能靠快捷命令触发
   2. markdownlint: 提供语法检查
3. 那么当前需要的自动补全，那就是需要设置vscode的`suggest`和`snippet`补全了

#### refe

- [microsoft: Markdown and Visual Studio Code](https://code.visualstudio.com/docs/languages/markdown)
- [microsoft: Snippets in Visual Studio Code](https://code.visualstudio.com/docs/editing/userdefinedsnippets)
- [VSCode设置Markdown自定义补全](https://juejin.cn/post/6844904089680085006)
- [microsoft: Intellisence](https://code.visualstudio.com/docs/editing/intellisense)

#### concept

1. outline
   outline 是vscod中侧边栏的一个面板，位于资源管理器`explorer`中的一个可折叠面板`collapsible panel`
2. snippet
   代码片段，通常指通过某些行为触发vscode自动生成对应代码并插入
   触发方式:
   1. `prefix`定义关键词触发 + `tab` 触发
   2. `key`定义快捷键触发
   3. 能否通过 `enter` 进行触发?
3. suggest
   1. suggest trigger?
      1. 使用 `ctrl` + `space` 并没有效果，不能触发建议列表
      2. 使用控制面板命令，直接输入`trigger suggest`可以在关键词边上触发建议列表
      3. 问题: 如何输入关键词后，出现建议列表?
4. auto-close
   1. 符号自动补全/闭合
   2. 如何触发，为什么现在没有了?
   3. 实现方式，插件 or builtin-setting

#### 实验记录

1. 设置了markdown代码片段以后似乎再编辑，并不能触发suggest，使用`tab`可以进行补全
2. 问题之前使用时，是提供了自动补全的部分功能的，为什么现在没有了?

![alt text](assets/image-20250412_170151-6e7bacd2.png)

