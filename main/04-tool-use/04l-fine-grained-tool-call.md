# 04l - 细粒度的工具调用

当将 Tool Use 与流式响应结合使用时，开发者能够在 Claude 生成工具调用参数的过程中实时处理，从而创造更响应式的用户体验。但对于背后的工作原理，有一些重要细节需要了解。

## 流式响应加工具调用

我们在前面介绍过开启流式响应后诸如 `ContentBlockDelta` 等的文本生成事件。对于工具调用，开发者还需要处理 `InputJsonEvent` 事件。

每个 `InputJsonEvent` 包含两个关键属性：

- partial_json：工具参数 JSON 的一个片段
- snapshot：到目前的所有片段所累积的 JSON 部分

![img](./04l-fine-grained-tool-call.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1752775508%2F06_-_011.1_-_Fine_Grained_Tool_Calling_02.1752775508676.png)

使用代码处理的逻辑类似于：

```python
for chunk in stream:
    if chunk.type == "input_json": # 判断是 InputJsonEvent
        print(chunk.partial_json) # 输出片段到 UI
        current_args = chunk.snapshot # 目前所累积的 JSON，在某一时刻可以被成功 parse
```

## JSON 验证

Anthropic API 并不会在 Claude 每次生成一小部分后立即发送数据块，而是会将数据块缓存，然后在特定时机进行局部验证。

更准确地说，API 会等待一个完整的、有效的顶层键值对准备好后再发送内容。例如，如果你的工具调用参数期望的结构为：

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

- 等待整个 `abstract` 完整
- 根据 Schema 验证目前已有的键值对是否是 Schema 的一部分
- 一次性发送所有缓存的与 `abstract` 相关的数据块
- 对 `meta` 对象重复该过程

![img](./04l-fine-grained-tool-call.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1752775511%2F06_-_011.1_-_Fine_Grained_Tool_Calling_11.1752775511555.png)

这意味着在涉及工具调用时，即使启用了流式传输，用户的体验仍类似于“一阵等待后跟着一串迸发出的文本”。

## 细粒度工具调用

如果你的场景非常延迟敏感（比如向用户展示实时更新，或要尽快开始处理结果），需要更快、更细粒度的流式传输，则可以启用细粒度工具调用：

```python
run_conversation(
    ...,
    fine_grained=True
)
```

启用后，例如上面的例子，开发者能在流中更早地接收到 `word_count`，而无需等待整个 `meta` 对象完整。

启用细粒度工具调用意味着：

- 可以在 Claude 生成时立即获取片段
- 顶层键之间没有 Buffer 带来的延迟
- 流式传输的行为将更传统、符合直觉
- 关键：但 JSON 校验将禁用，你的代码必须处理无效的 JSON

## 处理无效 JSON

在使用细粒度工具调用时，Claude 可能会生成无效的 JSON，如 `"word_count": undefined` ，而不是一个正确的数字。您的应用程序需要妥善处理这些情况：

```python
try:
    parsed_args = json.loads(chunk.snapshot)
except json.JSONDecodeError:
    # Handle invalid JSON appropriately
    print("Received invalid JSON, continuing...")
```

如果没有细粒度工具调用，API 的验证会捕获这个错误，并可能将有问题的值用字符串包裹，这可能不符合你的预期模式。

## 何时使用

- 您需要向用户展示工具参数生成的实时进度。
- 您希望尽可能快地开始处理部分工具结果。
- 缓冲延迟会负面影响您的用户体验。
- 您能够舒适地实现健壮的JSON错误处理。

对于大多数应用程序，带有验证的默认行为完全足够。但当您需要额外的响应能力时，细粒度工具调用为您提供控制，以便尽可能快地获取由Claude生成的块。
