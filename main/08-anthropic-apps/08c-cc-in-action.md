# 08c - Claude Code 实践

Claude Code 不仅仅是一个编写代码的工具——它旨在贯穿软件项目的每个阶段与你协同工作。把它想象成你团队中的另一位工程师，能够处理从初始设置到部署和支持的各项工作。

## `/init` 指令

当你在一个项目中开始使用 Claude Code 时，你首先想要做的是运行 `/init` 命令。这会告诉 Claude 扫描你的整个代码库，理解你的项目结构、依赖关系、编码风格和架构。

Claude 将其所学的一切总结在一个名为 `CLAUDE.md` 的特殊文件中。该文件会自动作为上下文包含在所有未来的对话中，因此 Claude 会记住有关您项目的要点。

你可以为不同范围拥有多个 CLAUDE.md 文件：

- 项目 - 由所有参与该项目的工程师共享
- 本地 - 你个人的笔记，未提交到 git
- 用户 - 在所有你的项目中使用

你也可以使用 `#` 命令快速为你的 CLAUDE.md 文件添加笔记。例如，输入 `# Always use descriptive variable names` 会提示你将此指南添加到你的项目、本地或用户记忆中。

## 常见流程

当您将其视为一种效率倍增器时，Claude 表现最佳。您提供的上下文和结构越多，结果就越好。以下是最高效的工作流程：

![img](./08c-cc-in-action.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748542929%2F10_-_003_-_Claude_Code_in_Action_11.1748542928969.jpg)

### 步骤 1：将上下文输入 Claude

在让 Claude 构建某物之前，识别出你代码库中与你想创建的功能相关的文件。让 Claude 先阅读和分析这些文件。这为 Claude 提供了你的编码模式和它可在此基础上构建的现有功能的示例。

### 步骤 2：告诉 Claude 规划解决方案

不要直接跳到实现，让 Claude 思考问题并创建一个计划。明确告诉 Claude 不要现在编写任何代码——只需专注于所需的方法和步骤。

### 步骤 3：请 Claude 实现解决方案

当你有一个稳固的计划后，请 Claude 来实施它。Claude 将根据你们已经共同完成的背景和规划工作来编写代码。

## 测试驱动开发（TDD）

- 将上下文输入 Claude - 与之前相同，向 Claude 展示相关文件
- 让 Claude 思考测试用例 - 让 Claude 头脑风暴哪些测试可以验证你的新功能
- 让 Claude 实现这些测试 - 选择最相关的测试，让 Claude 编写它们
- 让 Claude 编写通过这些测试的代码 - Claude 将迭代实现，直到所有测试都通过

这种方法通常能生成更健壮的代码。

## 一个例子

假设你想在现有项目中添加一个文档转换工具：

```
// First, ask Claude to read relevant files
> Read the math.py and document.py files

// Then ask for planning (not implementation)
> Plan to implement document_path_to_markdown tool:
1. Create a function that:
   - Takes a file path parameter
   - Validates the file exists  
   - Determines file type from extension
   - Reads binary data from file
   - Leverages existing binary_document_to_markdown function
   - Returns markdown string
2. Add appropriate documentation
3. Register the tool with MCP server
4. Add tests

// Finally, ask for implementation
> Implement the plan
```

Claude 将创建该函数、更新必要的文件、编写测试，甚至运行测试套件来验证一切是否正常工作。

## 常用命令

- `/clear` - 清除对话历史并重置上下文
- `/init` - 扫描代码库并创建 CLAUDE.md 文档
- `#` - 向您的 CLAUDE.md 文件添加笔记

使用 Claude Code 的关键在于记住它被设计为协作伙伴，而不仅仅是代码生成器。你提供的上下文和结构越多，Claude 就能越有效地帮助你构建和维护项目。







