# 07d - 使用 MCP 定义工具

使用官方 Python SDK 构建 MCP 服务器，可以无需手动编写复杂的 JSON Schema，而是通过装饰器和类型标注自动处理。

在这个示例中，我们创建一个管理存储在内存中的文档的 MCP 服务器，提供两个工具：一个用于读取文档内容，另一个通过查找-替换操作更新它们。

## 设置 MCP 服务器

Python MCP SDK 使得服务器创建变得很简单，只需一行代码即可初始化一个完整的 MCP 服务器：

```python
from mcp.server.fastmcp import FastMCP 
```

在这个示例中，文档存储在一个 Python Dict 中，键是文档 ID，值包含文档内容：

```python
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

### 创建文档阅读工具

第一个工具允许 Claude 通过 ID 读取文档：

```python
@mcp.tool(
    name="read_doc_contents", # 工具名
    description="Read the contents of a document and return it as a string." # 描述
)
def read_document(
    doc_id: str = Field(description="Id of the document to read") # 参数、类型标注、描述
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found") # 描述性的错误提示
    return docs[doc_id] # 具体实现
```

`@mcp.tool` 装饰器会自动生成 Claude 所需的 JSON Schema，Pydantic 的 `Field` 类则提供了参数描述，帮助 Claude 理解每个参数的期望值。

### 创建文档编辑工具

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

这个工具接收三个参数：文档 ID、要查找的文本和替换的文本。

## 错误处理

这两个工具都包含基本的错误处理机制，用于处理 Claude 请求不存在的文档的情况。当提供无效的文档 ID 时，工具会抛出一个 `ValueError`，附带 Claude 能够理解并可据此采取行动的描述性消息。

## 采用 SDK 方法的主要优势

- 从 Python 类型自动生成 JSON Schema
- 代码简洁、易读且易于维护
- 通过 Pydantic 实现自动的参数校验
- 与手动编写 Schema 相比减少了样板代码
- 具备类型安全性，开发过程中 IDE 能提供类型提示支持

MCP 的 Python SDK 将原本复杂的工具定义编写过程转变为对 Python 开发者来说更自然的事情，从而可以专注于业务逻辑，由 SDK 负责处理协议细节。
