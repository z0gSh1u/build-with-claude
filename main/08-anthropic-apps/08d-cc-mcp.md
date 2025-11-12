# 08d - Claude Code 与 MCP

Claude Code 内置了 MCP 客户端，使你可以连接 MCP 服务器来大幅扩展 Claude 的功能。每个 MCP 服务器可以向 Claude 暴露不同类型的功能：工具（用于执行操作）、提示词（提供模板）和资源（用于访问数据）。

![img](./08d-cc-mcp.assets/1.jpg)

## 设置 MCP 服务器

使用如下命令来注册 MCP 服务器：

```shell
claude mcp add [server-name] [command-to-start-server]
claude mcp add documents uv run main.py # 例如
```

注册后，Claude Code 在启动时会自动尝试连接到该服务器。

## 一个例子

考虑让 Claude 能够读取 PDF 和 Word 文档这一需求。通过一个具有 `document_path_to_markdown` 工具的 MCP 服务器，可以让 Claude 将文档内容转换为 Markdown 格式。在添加该 MCP 服务器后，当你要求 Claude 将 `tests/fixtures/mcp_docs.docx` 转换为 Markdown 时，它会自动使用所自定义工具来读取该文档并返回转换后的内容。

![img](./08d-cc-mcp.assets/2.jpg)

## 流行的 MCP 服务器

MCP 生态中，有许多常见开发工具和服务的 MCP 服务器：

- sentry-mcp：自动发现并修复 Sentry 中记录的错误
- playwright-mcp：为 Claude 提供浏览器自动化功能，用于测试和排障

- figma-context-mcp：将 Figma 设计稿提供给 Claude
- mcp-atlassian：允许 Claude 访问 Confluence 和 Jira
- firecrawl-mcp-server：为 Claude 添加网络抓取功能
- slack-mcp：允许 Claude 在 Slack 发布消息或回复特定 Thread

根据你的开发流程结合多个合适的 MCP 服务器，可以实现强大的智能化，例如：

- Sentry MCP 获取生产环境错误详情
- Jira MCP 读取需求工单
- Slack MCP 在完成工作时通知团队
- 还可以开发用于内部工具和 API 的自定义 MCP 服务器

这样的环境让 Claude 可以无缝地与你使用的工具和服务协同工作，使其成为一个更强大的、定制化的软件开发助手。

