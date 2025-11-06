# 07i - 定义提示词

在 MCP 服务器中，提示词允许你定义预先构建的高质量指令，客户端可以使用这些指令，而无需从零开始编写自己的提示词。可以将它们视为精心制作的模板，其效果通常优于用户自行构思的结果。

## 为什么使用提示词

假设你想让 Claude 将文档格式化为 Markdown。用户可以直接输入“将 report.pdf 转换为 markdown”，这样也可以正常工作。但他们可能会通过一个经过充分测试的提示获得更好的结果，这个提示包含关于格式、结构和输出要求的特定说明。

![img](./07i-define-prompt.assets/1.jpg)

关键在于，虽然用户可以自行完成这些任务，但当使用由 MCP 服务器作者精心开发和测试的提示时，他们能够获得更一致和更高质量的结果。

## 提示词如何工作

![img](./07i-define-prompt.assets/2.jpg)

提示语定义了一组用户和助手消息，客户端可以直接使用。当客户端请求提示语时，您的服务器会返回一组可以直接发送给 Claude 的消息。基本结构如下：

- 使用 `@mcp.prompt()` 装饰器定义提示
- 为每个提示添加名称和描述
- 返回构成完整提示的消息列表
- 这些提示应该是高质量的、经过充分测试的，并且与您的 MCP 服务器的目的相关

## 构建格式命令

以下是实现文档格式化提示的方法。首先，您需要导入基本消息类型，然后定义你的提示函数：

```python
from mcp.server.fastmcp import base

@mcp.prompt(
    name="format",
    description="Rewrites the contents of the document in Markdown format."
)
def format_document(
    doc_id: str = Field(description="Id of the document to format")
) -> list[base.Message]:
    prompt = f"""
Your goal is to reformat a document to be written with markdown syntax.

The id of the document you need to reformat is:

{doc_id}


Add in headers, bullet points, tables, etc as necessary. Feel free to add in extra formatting.
Use the 'edit_document' tool to edit the document. After the document has been reformatted...
"""

    return [
        base.UserMessage(prompt)
    ]
```

## 测试你的提示

您可以使用 MCP 检查器来测试提示。导航到提示部分，选择您的提示，并提供任何所需的参数。检查器将显示将发送给 Claude 生成的消息。这能让你验证你的提示正确地插入了变量，并在实际应用中使用前生成了预期的消息结构。

![img](./07i-define-prompt.assets/3.jpg)

## 最佳实践

在为您的 MCP 服务器创建提示时：

- 专注于与您的服务器目的密切相关的任务
- 编写详细、具体的指令，而不是模糊的要求
- 用不同的输入彻底测试你的提示
- 包含清晰的描述，以便用户了解每个提示的作用
- 考虑提示将如何与您的服务器工具和资源配合使用

要记住，提示的目的是提供用户自己难以轻易获得的价值——它们应该代表你在 MCP 服务器所涵盖领域的专业知识。
