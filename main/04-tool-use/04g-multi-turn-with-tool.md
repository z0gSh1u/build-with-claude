# 04g - 多轮工具调用

在前面的小节，我们处理了单条助手消息携带一或多个工具调用块的情况。现在我们将讨论 Claude 在给出最终回答前，需要在一串助手消息中每一条都请求工具调用的情况。

## 一个例子

假设用户问“今天起的第 103 天是哪一天？”，Claude 需要先调用 `get_current_datetime`，再基于调用结果再调用 `add_duration_to_datetime`。而“决定调用后者”这一决策及工具输入依赖前者的输出，因此无法放在具有两个工具调用块的一条助手消息中，而需要两条助手消息，就形成了多轮对话模式。

![img](./04g-multi-turn-with-tool.assets/1.png)

更具体地，流程如下：

- 用户询问：今天起的第 103 天是哪一天？
- Claude 响应了一个工具调用块，请求 `get_current_datetime`

- 开发者调用该函数，并返回结果
- Claude 意识到在提供最终回答前，还需要更多信息，并请求 `add_duration_to_datetime`
- 开发者调用该函数，并返回结果
- Claude 现在有足够的信息来提供最终回答

## 对话循环

由于我们无法预先知道工具调用的次数，要处理这种模式需要一个对话循环，一直持续到 Claude 停止请求工具调用：

```python
def run_conversation(messages):
    while True:
        response = chat(messages)
        add_user_message(messages, response)
        if is_not_asking_for_tool_call(response): # 伪代码
            break
        tool_result_blocks = run_tools(response)
        add_user_message(tool_result_blocks)

    return messages
```

本节的代码在 [04g.ipynb](./04g.ipynb)，我们再次更新了其中的 `add_user_message` 和 `add_assistant_message`，来正确处理带工具调用和工具结果的消息。具体地，我们需要如下基建：

- 可以处理不同消息格式的辅助函数
- `chat` 函数可以接收和传递工具 Schema
- `chat` 函数返回保留所有 Block 的完整消息对象，而不仅仅是文本
- `text_from_message` 函数用于从复杂消息中提取可读文本

随后，我们就可以实现自动处理多个工具调用的对话循环，让 Claude 可以根据需要使用尽可能多的工具来回答用户的问题。
