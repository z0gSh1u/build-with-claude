# 07d - 使用 MCP 定义工具

使用官方 Python SDK 构建 MCP 服务器会变得简单得多。你无需手动编写复杂的 JSON 工具架构，SDK 通过装饰器和类型提示为你处理所有这些复杂性。

在这个示例中，我们正在创建一个管理内存中存储文档的 MCP 服务器。该服务器将提供两个基本工具：一个用于读取文档内容，另一个通过查找替换操作来更新它们。

## 设置 MCP 服务器

Python MCP SDK 使得服务器创建变得极其简单。只需一行代码即可初始化一个完整的 MCP 服务器：

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("DocumentMCP", log_level="ERROR")
```

在这个实现中，文档存储在一个简单的 Python 字典中，其中键是文档 ID，值包含文档内容：

```
docs = {
    "deposition.md": "This deposition covers the testimony of Angela Smith, P.E.",
    "report.pdf": "The report details the state of a 20m condenser tower.",
    "financials.docx": "These financials outline the project's budget and expenditure",
    "outlook.pdf": "This document presents the projected future performance of the",
    "plan.md": "The plan outlines the steps for the project's implementation.",
    "spec.txt": "These specifications define the technical requirements for the equipment"
}
```

## 使用装饰器定义工具

SDK 将工具创建从冗长的过程转变为简洁易读的形式。您不再需要编写冗长的 JSON 架构，而是使用 Python 装饰器和类型提示。

### 创建文档阅读工具

第一个工具允许 Claude 通过 ID 读取任何文档。这是完整的实现：

```python
@mcp.tool(
    name="read_doc_contents",
    description="Read the contents of a document and return it as a string."
)
def read_document(
    doc_id: str = Field(description="Id of the document to read")
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    
    return docs[doc_id]
```

`@mcp.tool` 装饰器会自动生成 Claude 所需的 JSON 模式。Pydantic 中的 `Field` 类提供了参数描述，帮助 Claude 理解每个参数的期望值。

### 构建文档编辑工具

第二个工具对文档执行简单的查找和替换操作：

```python
@mcp.tool(
    name="edit_document",
    description="Edit a document by replacing a string in the documents content with a new string."
)
def edit_document(
    doc_id: str = Field(description="Id of the document that will be edited"),
    old_str: str = Field(description="The text to replace. Must match exactly, including whitespace."),
    new_str: str = Field(description="The new text to insert in place of the old text.")
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    
    docs[doc_id] = docs[doc_id].replace(old_str, new_str)
```

这个工具接受三个参数：文档 ID、要查找的文本和替换文本。实现部分使用 Python 的内置字符串方法 `replace()` ，以简化操作。

## 错误处理

这两种工具都包含基本的错误处理机制，用于管理 Claude 请求不存在的文档的情况。当提供无效的文档 ID 时，工具会引发一个 `ValueError` 错误，附带一条 Claude 能够理解并可能据此采取行动的描述性消息。

## 采用 SDK 方法的主要优势

- 从 Python 类型提示自动生成 JSON 模式
- 简洁、易读且易于维护的代码
- 通过 Pydantic 实现内置参数验证
- 与手动编写模式相比减少了样板代码
- 类型安全性和开发中的 IDE 支持

MCP Python SDK 将原本复杂的工具定义编写过程转变为对 Python 开发者来说自然的事情。你可以专注于业务逻辑，而 SDK 处理协议细节。
