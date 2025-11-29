# 08i - 小测验

> 在新的项目上使用 Claude Code，应首先运行什么来让 Claude 理解你的代码库？

- \# 添加备注
- ☑️ /init 扫描项目
- npm install 安装依赖
- /clear 重置所有

> “一个允许 Claude Code 连接到外部服务和工具以扩展其功能的系统”描述的是哪个术语？

- ☑️ 模型上下文协议（MCP）
- 计算机使用（Computer Use）
- 终端集成（Terminal integration）
- CLAUDE.md 文件

> 你希望用 Claude Code 开发一个新功能，最有效的方法是什么？

- 先自己开始编码
- 立刻让 Claude 编写代码
- ☑️ 提供背景信息，要求 Claude 制定计划，然后再实施
- 仅提供功能名称

> 你想在项目中让 Claude Code 读取 PDF 文件，如何添加这个功能？

- 切换到不同的 AI 助手
- 仅使用 Claude 的内置功能
- ☑️ 连接到一个带有文档工具的 MCP 服务器
- 安装一个新的文本编辑器

> 你想在同一项目上同时运行多个 Claude 实例，需要解决的主要问题是什么？

- 一次只能运行一个 Claude 实例
- Claude 实例运行速度会过慢
- Claude 实例会占用过多内存
- ☑️ Claude 实例在编辑相同文件时将发生冲突

> Computer Use 与 Claude 的工具系统是如何相关联的？

- 它只能通过连接到 Claude 服务器的专用硬件来工作
- 它是一个完全独立的系统，绕过了正常的 Tool Use 流程
- ☑️ 它被实现为一个特殊的工具，遵循与其他 Claude 工具相同的 Tool Use 模式
- 它需要直接访问 API，不能与标准的工具接口（Tool Interface）一起使用

> 你想让每个 Claude 实例使用自己的工作空间，应该使用哪个 Git 功能？

- 仅使用 Git branches
- Git commits
- ☑️ Git worktrees
- Git repositories
