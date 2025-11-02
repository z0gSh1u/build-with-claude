# 04i - 使用多个工具

目前，我们已经成功集成了一个工具 `get_current_datetime`。根据先前的分析，我们总共需要为 Claude 集成三个工具，这一小节将介绍如何让 Claude 可以使用多个工具。

## 添加更多工具

只需要将其他工具的 Schema 加入 `tools` 参数中，即可允许 Claude 在对话期间使用所有工具：

```python
response = chat(messages, tools=[
    get_current_datetime_schema,
    add_duration_to_datetime_schema,
    set_reminder_schema
])
```

同样地，需要为新的工具的调用也提供实现逻辑：

```python
def run_tool(tool_name, tool_input):
    if tool_name == "get_current_datetime":
        return get_current_datetime(**tool_input)
    elif tool_name == "add_duration_to_datetime":
        return add_duration_to_datetime(**tool_input)
    elif tool_name == "set_reminder":
        return set_reminder(**tool_input)
```

要测试 Claude 能否正确使用多个工具，可以尝试一个需要多种工具来完成的请求：

```
为我的医院挂号预约设置提醒。时间是2050年1月1日后的第177天。
```

Claude 首先解释了它需要做什么，然后按顺序调用适当的工具来处理这个问题（`add_duration_to_datetime` 和 `set_reminder`）。

![img](./04i-multi-tools.assets/1.png)

同样地，对话历史中的消息结构将类似于：

- 用户消息
- 包含文本块和 Tool Use 块的的助手消息
- 工具调用结果消息
- 后续的助手消息

将工具调用收口到路由函数 `run_tool` 中，将需要提供给 Claude 的工具加入 `tools` 列表参数中，这种模块化的方法使得在不大规模重构现有代码的情况下，也能容易地扩展 AI 助手的功能。
