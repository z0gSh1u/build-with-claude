# 07j - 使用提示词

## 列出提示词

第一步是在你的 MCP 客户端中实现 `list_prompts` 方法，从服务器检索所有可用的提示词：

```python
async def list_prompts(self) -> list[types.Prompt]:
    result = await self.session().list_prompts() # 调用 session 上的 list_prompts
    return result.prompts # 返回提示词数组
```

## 获取特定提示词

`get_prompt` 方法用于获取一个特定的提示词，并将参数插入其中。当你请求一个提示词时，需要提供用于模版化的参数：

```python
async def get_prompt(self, prompt_name, args: dict[str, str]):
    result = await self.session().get_prompt(prompt_name, args) # 获取插入好参数后的提示词
    return result.messages # 构成一组聊天记录，可以直接输入到 Claude 中
```

## 提示词的最佳实践

在为 MCP 服务器创建提示词时：

- 提示词应与 MCP 服务器的目的相关
- 在部署前彻底测试它们
- 使用清晰、具体的指令
- 与你可用的工具配合
- 考虑用户需要提供哪些参数

提示词在预定义的功能和动态的用户需求之间架起桥梁，为复杂的任务提供了一个起点，并通过参数化保持灵活性。
