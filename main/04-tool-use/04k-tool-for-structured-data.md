# 04k - 用于结构化数据的工具

当你需要从 Claude 获取结构化数据时，主要有两种方法：使用消息预填和停止序列的基于提示的技术，或使用工具的更健壮的方法。虽然基于提示的方法设置起来更简单，但工具提供了更可靠的输出，代价是增加了额外的复杂性。

## 结构化数据工具

基于工具的方法通过创建一个 JSON 模式来定义您想要提取数据的精确结构。您不再寄希望于 Claude 正确地格式化其响应，而是实际上在向 Claude 提供一个具有特定参数的函数来调用，这些参数与您期望的输出结构相匹配。这是流程的工作方式：

- 编写一个模式来描述你所寻找的数据结构
- 强制 Claude 使用带有 `tool_choice` 参数的工具
- 从工具使用响应中提取结构化数据
- 无需提供后续响应——一旦获得数据就完成了

![img](./04k-tool-for-structured-data.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748623754%2F06_-_011_-_Tools_for_Structured_Data_03.1748623754770.png)

例如，如果你想要从一份报表中提取财务余额和关键见解，你的模式会分别将它们定义为整数和字符串数组。

## 控制工具使用

这一技术的一个关键部分是确保 Claude 确实调用你的工具。你可以使用 `tool_choice` 参数控制这一行为：

- `{"type": "auto"}` - 模型决定是否需要使用工具（默认）
- `{"type": "any"}` - 模型必须使用一个工具，但可以选择使用哪一个
- `{"type": "tool", "name": "TOOL_NAME"}` - 模型必须使用指定的工具

对于结构化数据提取，你通常需要选择第三种选项来确保 Claude 调用你特定的模式工具。

## 一个例子

假设你想从一篇文章中提取标题、作者和关键见解。首先，你需要创建一个工具架构：

```python
article_summary_schema = {
    "name": "article_summary",
    "description": "Extracts structured data from articles",
    "input_schema": {
        "type": "object",
        "properties": {
            "title": {"type": "string"},
            "author": {"type": "string"}, 
            "key_insights": {
                "type": "array",
                "items": {"type": "string"}
            }
        }
    }
}
# 然后你将使用该工具调用 Claude，并强制其使用：
response = chat(
    messages,
    tools=[article_summary_schema],
    tool_choice={"type": "tool", "name": "article_summary"}
)
# 响应将包含一个工具使用块，其中包含您在 input 字段中的结构化数据。您可以直接访问它：
structured_data = response.content[0].input
```

当你需要快速简单的结构化输出时，选择基于提示的方法。当你需要保证可靠性和能够处理额外的设置复杂性时，使用工具。这两种技术都很有价值，具体取决于你的特定用例和需求。



