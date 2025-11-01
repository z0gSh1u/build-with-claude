# Summary

[前言](README.md)

# 访问 Claude

- [访问 Claude API](01-accessing-claude/01a-accessing-the-api.md)
- [获取 API Key](01-accessing-claude/01b-getting-an-api-key.md)
- [发起请求](01-accessing-claude/01c-making-a-request.md)
- [多轮对话](01-accessing-claude/01d-multi-turn.md)
- [练习聊天](01-accessing-claude/01e-chat-exercise.md)
- [系统提示词](01-accessing-claude/01f-system-prompt.md)
- [系统提示词练习](01-accessing-claude/01g-system-prompt-exercise.md)
- [温度](01-accessing-claude/01h-temperature.md)
- [课程满意度调查](01-accessing-claude/01i-survey.md)
- [流式响应](01-accessing-claude/01j-streaming.md)
- [控制模型输出](01-accessing-claude/01k-control-output.md)
- [结构化数据](01-accessing-claude/01l-structured-data.md)
- [结构化数据练习](01-accessing-claude/01m-structured-data-exercise.md)
- [小测验](01-accessing-claude/01n-quiz.md)

# 提示词评估

- [提示词评估介绍](02-prompt-evaluation/02a-prompt-evaluation.md)
- [典型的评估流程](02-prompt-evaluation/02b-typical-eval-workflow.md)
- [生成测试数据集](02-prompt-evaluation/02c-gen-test-set.md)
- [运行评测](02-prompt-evaluation/02d-run-eval.md)
- [基于模型的打分](02-prompt-evaluation/02e-model-based.md)
- [基于代码的打分](02-prompt-evaluation/02f-code-based.md)
- [提示词评估练习](02-prompt-evaluation/02g-exercise.md)
- [小测验](02-prompt-evaluation/02h-quiz.md)

# 提示词工程

- [提示词工程介绍](03-prompt-engineering/03a-prompt-engineering.md)
- [清晰且直白](03-prompt-engineering/03b-clear-and-direct.md)
- [具体并准确](03-prompt-engineering/03c-specific.md)
- [使用 XML 标签](03-prompt-engineering/03d-xml-tag.md)
- [提供示例](03-prompt-engineering/03e-few-shot-example.md)
- [提示词工程练习](03-prompt-engineering/03f-exercise.md)
- [小测验](03-prompt-engineering/03g-quiz.md)

# 工具使用（Tool Use）

- [Tool Use 介绍](04-tool-use/04a-introduce.md)
- [项目概述](04-tool-use/04b-overview.md)
- [工具函数](04-tool-use/04c-function.md)
- [工具 Schema](04-tool-use/04d-schema.md)
- [处理消息块](04-tool-use/04e-message-block.md)
- [发送工具调用结果](04-tool-use/04f-tool-result.md)
- [多轮工具调用](04-tool-use/04g-multi-turn-with-tool.md)
- [实现带工具的多轮对话](04-tool-use/04h-implement-multi-turn.md)
- [使用多个工具](04-tool-use/04i-multi-tools.md)
- [批处理工具](04-tool-use/04j-batch-tool.md)
- [用于结构化数据的工具](04-tool-use/04k-tool-for-structured-data.md)
- [细粒度的工具调用](04-tool-use/04l-fine-grained-tool-call.md)
- [文本编辑工具](04-tool-use/04m-text-edit.md)
- [网络搜索工具](04-tool-use/04n-web-search.md)
- [小测验](04-tool-use/04o-quiz.md)

# 检索增强生成（RAG）

- [检索增强生成简介](05-rag/05a-introduce.md)
- [文本分块](05-rag/05b-text-chunking.md)
- [文本嵌入](05-rag/05c-text-embedding.md)
- [完整的 RAG 流程](05-rag/05d-rag-flow.md)
- [实现 RAG 流程](05-rag/05e-implement-rag-flow.md)
- [BM25 语义搜索](05-rag/05f-bm25-lexical-search.md)
- [多索引 RAG](05-rag/05g-multi-index-rag.md)
- [重排序](05-rag/05h-rerank.md)
- [上下文检索](05-rag/05i-contextual-retrieval.md)
- [小测验](05-rag/05j-quiz.md)

# Claude 特性

- [扩展思考](06-claude-features/06a-extended-thinking.md)
- [图像理解](06-claude-features/06b-image.md)
- [PDF 理解](06-claude-features/06c-pdf.md)
- [引用](06-claude-features/06d-citation.md)
- [提示词缓存](06-claude-features/06e-prompt-cache.md)
- [提示词缓存的规则](06-claude-features/06f-rules-of-cache.md)
- [缓存实践](06-claude-features/06g-cache-in-action.md)
- [代码执行与文件 API](06-claude-features/06h-code-exec-and-files-api.md)
- [小测验](06-claude-features/06i-quiz.md)

# 模型上下文协议

- [模型上下文协议介绍](07-mcp/07a-introduce.md)
- [MCP 客户端](07-mcp/07b-client.md)
- [项目配置](07-mcp/07c-setup.md)
- [使用 MCP 定义工具](07-mcp/07d-tool-with-mcp.md)
- [MCP Inspector](07-mcp/07e-inspector.md)
- [实现 MCP 客户端](07-mcp/07f-implement-client.md)
- [定义资源](07-mcp/07g-define-resource.md)
- [访问资源](07-mcp/07h-access-resource.md)
- [定义提示词](07-mcp/07i-define-prompt.md)
- [客户端中的提示词](07-mcp/07j-prompt-in-client.md)
- [回顾](07-mcp/07k-review.md)
- [小测验](07-mcp/07l-quiz.md)

# Anthropic 的应用

- [Anthropic 的应用介绍](08-anthropic-apps/08a-anthropic-app.md)
- [Claude Code 配置](08-anthropic-apps/08b-cc-setup.md)
- [Claude Code 实践](08-anthropic-apps/08c-cc-in-action.md)
- [Claude Code 与 MCP](08-anthropic-apps/08d-cc-mcp.md)
- [Claude Code 并行](08-anthropic-apps/08e-cc-parallel.md)
- [Computer Use 自动调试](08-anthropic-apps/08f-cua-auto-debug.md)
- [Computer Use 简介](08-anthropic-apps/08g-cua.md)
- [Computer Use 原理](08-anthropic-apps/08h-cua-how.md)
- [小测验](08-anthropic-apps/08i-quiz.md)

# Agent 和工作流

- [Agent 和工作流介绍](09-agents-and-workflows/09a-agent-and-workflow.md)
- [并行式工作流](09-agents-and-workflows/09b-parallel-workflow.md)
- [链式工作流](09-agents-and-workflows/09c-chain-workflow.md)
- [路由式工作流](09-agents-and-workflows/09d-route-workflow.md)
- [Agent 和工具](09-agents-and-workflows/09e-agent-and-tool.md)
- [环境感知](09-agents-and-workflows/09f-env-inspect.md)
- [工作流还是 Agent](09-agents-and-workflows/09g-workflow-vs-agent.md)
- [小测验](09-agents-and-workflows/09h-quiz.md)
