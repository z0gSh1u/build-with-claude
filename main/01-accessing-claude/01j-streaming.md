# 01j - 流式响应

对于一条用户输入，Claude 等大模型通常需要几秒至几十秒时间来生成完整的响应，导致用户长时间等待。为了优化用户体验，可以用流式响应的方式，允许用户逐块看到响应。

## 流式响应的原理

开启 streaming 后，Claude 会先返回一个初始响应，表明已收到请求并开始生成文本，随后会收到一系列事件，每一个事件包含完整响应的一部分。

![img](./01j-streaming.assets/1.png)

Claude 发送回的几类事件包括：

- MessageStart：消息开始
- ContentBlockStart：新的文本块或工具使用块或其他块的开始
- ContentBlockDelta：包含实际生成文本的块，通常用于展示给用户
- ContentBlockStop：当前内容块完成
- MessageDelta：当前消息完成
- MessageStop：当前消息结束

![img](./01j-streaming.assets/2.png)

## 使用流式响应

在 `create` 函数中添加 `stream=True` 参数即可，此时返回变为 stream 而非 message。我们在 [01j.ipynb](https://github.com/z0gSh1u/build-with-claude/blob/master/main/01-accessing-claude/01j.ipynb) 中实现了这部分逻辑。

```python
# 请求
stream = client.messages.create(
    model=model,
    max_tokens=1000,
    messages=messages,
    stream=True
)
# 消费流
for event in stream:
    print(event)
```

event 中包含许多额外的信息，如果只需要文本，可以换用 `stream` 函数，只消费 text_stream：

```python
# 请求
with client.messages.stream(
    model=model,
    max_tokens=1000,
    messages=messages,
) as stream:
    # 消费
    for text in stream.text_stream:
        print(text, end="")
```

使用 `stream` 函数时，在流完成后，可以调用 `stream.get_final_message` 来获取组装好的最终消息：

```python
final_message = stream.get_final_message()
```

这允许你对完整消息进行存储或者其他处理，而流式的 chunk 用于即时反馈的 UI 展示。
