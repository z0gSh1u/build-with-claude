# 07i - 定义提示词

在 MCP 服务器中，提示词允许你定义预先准备的高质量指令；客户端可以使用这些指令，而无需从零开始自己编写提示词。可以将它们视为精心制作的模板，其效果通常优于用户自行构思的结果。

## 动机

假设你想让 Claude 将文档格式化为 Markdown。虽然用户可以直接输入 “将 report.pdf 转换为 Markdown” 作为提示词，大概率也可以正常工作，但一个经过充分测试的、包含格式、结构和输出要求的提示词往往能获得更好的结果。

![img](./07i-define-prompt.assets/1.jpg)

## 提示词如何工作

![img](./07i-define-prompt.assets/2.jpg)

当 MCP 客户端请求提示词时，MCP 服务器会返回一组可以直接发送给 Claude 的用户和助手消息，基本流程如下：

- 使用 `@mcp.prompt()` 装饰器定义提示词
- 为每个提示词添加名称和描述
- 返回构成完整提示词的消息列表
- 这些提示词应该是高质量的、经过充分测试的，且与 MCP 服务器的目的相关

## 一个例子

以上述为实现文档格式化的提示词为例，首先需要导入基本消息类型，然后定义提示词函数：

```python
from mcp.server.fastmcp import base

@mcp.prompt(
    name="format",
    description="以Markdown格式重写文档内容"
)
def format_document(
    doc_id: str = Field(description="要格式化的文档的ID")
) -> list[base.Message]:
    prompt = f"""
你的目标是重新格式化一个文档，使其使用 Markdown 语法编写。

你需要重新格式化的文档的 ID 是：

{doc_id}

根据需要添加标题、Bullet Point、表格等。可以自由添加额外的格式。
使用 'edit_document' 工具来编辑文档。在完成文档格式化后...
"""
    return [base.UserMessage(prompt)] # 返回一个消息列表，这里将其作为用户消息
```

## 测试提示词

可以使用 MCP Inspector 来测试。在 Prompts Tab 下，选择提示词，并提供所需的参数。Inspector 将显示将发送给 Claude 的消息。这让你能验证变量正确地插入了提示词，并生成了预期的消息结构。

![img](./07i-define-prompt.assets/3.jpg)

## 最佳实践

在为 MCP 服务器创建提示词时：

- 专注于与你的 MCP 服务器的目的密切相关的任务
- 编写详细、具体的指令，而不是模糊的需求
- 用不同的输入彻底测试你的提示词
- 包含清晰的描述，以便用户了解每个提示词的作用
- 考虑提示词如何与 MCP 服务器上的工具和资源配合使用

预定义提示词的目的是提供用户难以自行实现的价值，它们应该代表你的 MCP 服务器的目的领域的专业知识。
