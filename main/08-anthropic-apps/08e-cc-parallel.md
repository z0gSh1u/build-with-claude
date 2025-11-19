# 08e - Claude Code 并行

并行运行多个 Claude Code，为每个实例分配不同的任务并让它们同时工作，能为你提供一个由虚拟软件工程师组成的项目团队，带来很大的生产力提升。

![img](./08e-cc-parallel.assets/1.jpg)

## 核心挑战

并行的主要问题是文件冲突。当两个实例同时修改一个文件时，由于它们不了解彼此的修改，可能会写入冲突或无效的代码。

解决方案很简单：给每个实例分配一个独立的工作空间，在各自的项目副本上工作，在隔离状态下进行修改，然后将这些修改合并回主项目。

## Git Worktree

Git Worktree 是这个流程的完美工具。该机制允许在同一仓库中同时 checkout 多个分支，每个 Worktree 对应一个单独的分支；让 Claude Code 实例在不同分支上运行，完成工作后提交更改并将它们合并回主分支（就像一般的 Git 协作流程一样）。

![img](./08e-cc-parallel.assets/2.jpg)

相比起手动创建 Worktree，你可以让 Claude 处理整个流程，例如借助如下 Prompt 来创建 Worktree 并设置工作区：

![img](./08e-cc-parallel.assets/3.jpg)

这个提示词告诉 Claude：

- 检查是否已存在 Worktree
- 在 `.trees` 文件夹中创建新的 Git Worktree
- 将 `.venv` 文件夹 Symlink 到 Worktree 目录中（因为虚拟环境不被 Git 管理）
- 在该目录中启动新的 VSCode 实例

## 自定义斜杠命令

反复输入长提示词会很繁琐，Claude Code 支持自定义命令，可自动执行此过程：

- 在 `.claude/commands` 中添加一个 `.md` 文件
- 将提示词放在文件中
- 使用 `$ARGUMENTS` 作为动态值的占位符
- 使用 `/project:filename argument` 运行

![img](./08e-cc-parallel.assets/4.jpg)

## 并行开发的流程

![img](./08e-cc-parallel.assets/5.jpg)

以下是一个典型的并行开发工作方式：

- 为不同功能创建多个 Worktree
- 在每个工作空间中启动 Claude Code
- 为每个实例分配不同的任务
- 让它们并行工作
- 当每个任务完成时提交更改
- 将所有分支合并回主分支

对于最后一步“合并”的过程，也可以使用另一个自定义命令来自动化，即写一段提示词来：

- 切换到 Worktree 目录
- Checkout 到最新提交
- 切换回根目录
- 合并 Worktree 到主分支
- 自动处理合并冲突，根据对变更的理解解决冲突

![img](./08e-cc-parallel.assets/6.jpg)

这种工作方式的扩展上限取决于你能有效管理（协调多项任务、解决合并冲突）的并行实例数量。你不再需要按顺序开发功能，而是可以同时开发多个功能，从而大幅缩短复杂项目的开发时间。
