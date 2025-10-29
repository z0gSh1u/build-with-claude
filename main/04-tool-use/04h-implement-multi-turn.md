# 04h - 实现带工具的多轮对话

本节将具体介绍如何实现带工具调用的多轮对话，即上一节提到的对话循环模式。简单来说，就是不断调用 Claude 直到它停止请求工具调用。

## 实现对话循环

### 检测工具调用请求

知道 Claude 是否需要使用工具的关键在于响应消息的 `stop_reason` 字段。当 Claude 决定需要调用工具时，该字段会被设置为 `"tool_use"`，因此停止循环的条件是：

```python
if response.stop_reason != "tool_use":
    break
```

### 实现细节

可以表示为如下代码：

```python
def run_conversation(messages):
    while True:
        response = chat(messages, tools=[get_current_datetime_schema]) # 获得 Claude 响应
        add_assistant_message(messages, response) # 将助手消息添加到消息记录
        print(text_from_message(response)) # 输出响应的纯文本部分
        # 如果不再需要工具调用，则退出循环
        if response.stop_reason != "tool_use":
            break
        tool_results = run_tools(response) # 实际执行工具
        add_user_message(messages, tool_results) # 将工具结果添加到消息记录
        # 然后持续执行，直到不再需要工具调用
    return messages
```

考虑到单个响应中可以请求若干个工具调用，我们使用如下函数来筛选出所有工具调用块并逐个处理：

```python
def run_tools(message):
    tool_requests = [b for b in message.content if b.type == "tool_use"] # 所有工具调用块
    tool_result_blocks = [] # 所有工具结果块
    for tool_request in tool_requests:
        tool_output = ... # 根据 name 调用工具，参数为 input，可以实现一个可扩展的工具路由函数
        # 构造工具调用结果块
        tool_result_block = {
            "type": "tool_result",
            "tool_use_id": tool_request.id, # 注意 ID 需要与工具调用块相匹配
            # 当工具调用成功时
            "content": json.dumps(tool_output), # 序列化为字符串
            "is_error": False
            # 当工具调用失败时，也需要提供结果块
            "content": "Error: ...", # 提供有意义的错误信息
            "is_error": True
        }
        tool_result_blocks.append(tool_result_block) # 添加结果块
```

本节的代码实现在 [04h.ipynb](./04h.ipynb)。

总的来说，完整的带工具调用的多轮对话流程如下：

- 将带 `tools` 的用户消息发送给 Claude
- Claude 响应文本和/或工具调用请求
- 开发者执行所有工具调用请求并创建结果块
- 开发者将工具调用结果作为用户消息，将完整对话历史发送回 Claude
- 重复直到 Claude 提供最终答案

这让 Claude 可以在多次来回中使用多个工具来全面回答用户问题。对话历史保留了完整上下文，允许 Claude 基于前序工具调用结果继续组织响应，以提供更全面的回答。

