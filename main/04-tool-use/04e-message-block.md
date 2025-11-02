# 04e - 处理消息块

在使用 Claude 的 Tool Use 功能时，需要介绍一种新的响应结构。Claude 不再仅仅返回单一的文本块，而是返回包含文本和工具使用信息的多块（Multi-Block）消息。

## 启用 Tool Use

要使 Claude 能够使用工具，需要在 API 调用中包含 `tools` 参数，传入 JSON Schema 数组：

```python
messages = []
messages.append({
    "role": "user",
    "content": "What is the exact time, formatted as HH:MM:SS?"
}
response = client.messages.create(...,
    tools=[get_current_datetime_schema],
)
```

## 多块消息的结构

当 Claude 决定使用工具时，会返回包含多块的消息，例如文本块 + 工具使用块：

![img](./04e-message-block.assets/1.png)

文本块包含人类可读文本，解释 Claude 正在做什么；工具使用块包含调用哪个工具以及使用什么参数的代码化的说明。更具体地，工具使用块的结构含：

- 一个 ID
- 要调用的函数名称
- dict 类型的调用参数
- 块类型标识符 `tool_use`

## 完整的 Tool Use 流程

Tool Use 遵循如下流程，每一步都需要仔细处理消息结构：

- 向 Claude 发送带工具 Schema 的消息
- 接收带文本块和工具使用块的助手消息
- 提取工具使用信息并执行实际功能：即 Claude 只会给出调用工具的必要信息（或者说“请求调用”），而“调用”操作本身需要开发者自己处理
- 将工具调用结果连同对话历史一起发送回 Claude：当在往对话历史中添加包含工具调用的助手消息时，需要附加完整的文本块和工具使用块，才能正确保持上下文

  ```python
  messages.append({
      "role": "assistant",
      "content": response.content # 需要是完整的 content，不能丢弃任何 Block
  })
  ```

- 从 Claude 接收最终响应

本节的代码在 [04e.ipynb](./04e.ipynb)，我们更新了其中的 `add_user_message` 和 `add_assistant_message`，来正确处理多块消息。
