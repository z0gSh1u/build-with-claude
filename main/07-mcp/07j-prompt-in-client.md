# 07j - 客户端中的提示词

MCP 中的提示定义了一组用户和助手消息，客户端可以使用这些消息。这些提示应该是高质量的、经过充分测试的，并且与 MCP 服务器的整体目的相关。

## list_prompts

第一步是在你的 MCP 客户端中实现 `list_prompts` 方法。这个方法从服务器检索所有可用的提示：

```python
async def list_prompts(self) -> list[types.Prompt]:
    result = await self.session().list_prompts()
    return result.prompts
```

这个简单的实现调用了会话的 `list_prompts` 方法，并从结果中返回提示数组。

## get_prompt

`get_prompt` 方法会获取一个特定的提示，并将参数插入其中。当你请求一个提示时，你会提供参数，这些参数会作为关键字参数传递给提示函数：

```python
async def get_prompt(self, prompt_name, args: dict[str, str]):
    result = await self.session().get_prompt(prompt_name, args)
    return result.messages
```

该方法返回结果中的消息，这些消息构成一个对话，可以直接输入到 Claude 中。

## 参数如何工作

当你在服务器端定义一个提示函数时，它可以接受参数。例如，一个文档格式化提示可能会期望一个 `doc_id` 参数：

```python
def format_document(doc_id: str):
    # The doc_id gets interpolated into the prompt
```

当客户端调用 `get_prompt` 时，参数字典应包含预期的键。MCP 服务器将这些键作为关键字参数传递给提示函数，允许将动态内容插入到提示模板中。

## 在 CLI 中测试提示

一旦实施，您可以通过命令行界面测试提示。当您输入斜杠时，可用的提示会作为命令出现。选择一个提示可能会提示您从可用选项（如文档 ID）中进行选择，然后完整的提示会被发送给 Claude。工作流程如下：

- 用户选择一个提示（例如"格式化"）
- 系统提示所需的参数（例如要格式化的文档）
- 提示会发送给 Claude，其中包含插值后的值
- Claude 然后可以使用工具获取更多数据并完成任务

## 提示词最佳实践

在为您的 MCP 服务器创建提示词时：

- 让他们与您服务器的目的相关
- 在部署前彻底测试它们
- 使用清晰、具体的指令
- 设计它们以与您可用的工具良好配合
- 考虑用户需要提供哪些参数

提示词在预定义功能和动态用户需求之间架起桥梁，为 Claude 提供结构化的复杂任务起点，同时通过参数化保持灵活性。
