# 07f - 实现 MCP 客户端

我们的 MCP 服务器已经可以运行，接下来该构建客户端了。客户端是让我们的应用与 MCP 服务器通信的关键。

## 客户端架构

在多数实际项目中，开发者只需要实现 MCP 服务器或客户端中的一个，但在这个示例项目中我们同时构建二者，以看到它们是如何协同工作的。

MCP 客户端包含两个主要概念：

- MCPClient 类：一个用于简化会话使用和清理的类
- 客户端会话：实际到服务器的连接，也是 MCP Python SDK 的一部分

![img](./07f-implement-client.assets/1.jpg)

## 如何集成客户端进应用程序

AI 应用常常需要完成下面两件事情：

- 获取发送给 Claude 的工具列表

- 当 Claude 请求时，执行工具

我们的应用可以通过简单的方法调用，让 MCP 客户端协助实现这些功能。

![img](./07f-implement-client.assets/2.jpg)

### 实现核心方法

我们的客户端需要实现两个关键方法： `list_tools` 和 `call_tool` 。

- List Tools 方法从 MCP 服务器获取所有可用的工具：

```python
async def list_tools(self) -> list[types.Tool]:
    result = await self.session().list_tools() # 访问与 MCP 服务器连接的会话，调用内置的函数
    return result.tools # 从结果中返回工具
```

- Call Tool 方法在服务器上执行特定工具：

```python
async def call_tool(
    self, tool_name: str, tool_input: dict
) -> types.CallToolResult | None:
    # 将 Claude 提供的工具名称和输入参数传递给 MCP 服务器，并返回结果
    return await self.session().call_tool(tool_name, tool_input)
```

### 测试客户端

要测试我们的实现，可以直接运行客户端，连接到 MCP 服务器并调用相应方法：

```python
async with MCPClient(
    command="uv", args=["run", "mcp_server.py"]
) as client:
    result = await client.list_tools()
    print(result) # 看看是否打印出工具的定义，包括之前创建的 read_doc_contents 和 edit_document
```

### 整合

至此，我们可以测试完整流程。当运行应用程序并询问 Claude 关于某文档的问题时：

- 应用的代码使用 MCP 客户端来获取可用工具
- 这些工具连同用户的问题一起发送给 Claude
- Claude 决定使用 `read_doc_contents` 工具
- 应用的代码使用 MCP 客户端来执行该工具
- 结果被发送回给 Claude，然后 Claude 回复用户

MCP 客户端充当了我们的应用逻辑与 MCP 服务器之间的桥梁，使得访问 MCP 服务器上的功能变得容易，无需担心底层的连接细节。
