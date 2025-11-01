# 04l - 细粒度的工具调用

当你将工具使用与 Claude 中的流式处理结合时，你会在 AI 生成工具参数时获得实时更新。这创造了更响应的用户体验，但在其背后工作原理上，有一些重要的细节需要了解。

## 工具流式处理

启用流式处理后，Claude 在处理你的请求时会发送不同类型的事件。你已经熟悉像 `ContentBlockDelta` 这样的常规文本生成事件。对于工具使用，你还需要处理一种新的称为 `InputJsonEvent` 的事件类型。

每个 `InputJsonEvent` 包含两个关键属性：

- partial_json - 一个表示工具参数部分的 JSON 片段
- snapshot - 从目前收到的所有数据块中累积构建的 JSON

![img](./04l-fine-grained-tool-call.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1752775508%2F06_-_011.1_-_Fine_Grained_Tool_Calling_02.1752775508676.png)

```python
for chunk in stream:
    if chunk.type == "input_json":
        # Process the partial JSON chunk
        print(chunk.partial_json)
        # Or use the complete snapshot so far
        current_args = chunk.snapshot
```

## JSON 验证如何工作

Anthropic API 并不会在 Claude 生成的同时立即发送每一个数据块。相反，它会先将数据块缓存起来，然后再进行验证。

API 会等待完整的顶层键值对后再发送任何内容。例如，如果您的工具期望这种结构：

```json
{
  "abstract": "This paper presents a novel...",
  "meta": {
    "word_count": 847,
    "review": "This paper introduces QuanNet..."
  }
}
```

API 将：

- 等待整个 `abstract` 值完成
- 根据你的模式验证键值对
- 一次性发送所有缓存的 `abstract` 数据块
- 对 `meta` 对象重复该过程

![img](./04l-fine-grained-tool-call.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1752775511%2F06_-_011.1_-_Fine_Grained_Tool_Calling_11.1752775511555.png)

这个验证过程解释了为什么即使启用了流式传输，你也会看到延迟后跟文本的突发。这些数据块被暂缓处理，直到一个完整的、有效的顶层键值对准备好为止。

## 细粒度工具调用

如果你需要更快、更细粒度的流式传输——也许是为了向用户展示即时更新或快速开始处理部分结果——你可以启用细粒度工具调用。

细粒度工具调用主要做一件事：它禁用了 API 端的 JSON 验证。这意味着：

- 你可以在 Claude 生成它们时立即获取这些片段
- 顶级键之间没有缓冲延迟
- 更传统的流式行为
- 关键：JSON 验证已禁用 - 你的代码必须处理无效的 JSON

```python
run_conversation(
    messages, 
    tools=[save_article_schema], 
    fine_grained=True
)
```

通过细粒度工具调用，你可能在流中更早地接收到 `word_count` 值，而无需等待整个 `meta` 对象完成。

