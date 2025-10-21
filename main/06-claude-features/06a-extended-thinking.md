# 06a - 扩展思考

扩展思考（Extended Thinking）是 Claude 的高级推理功能，它给模型更长时间来处理复杂问题，然后生成回复。你可以看到 Claude 的推理过程，有助提高透明度，并通常能产生更高质量的回复。

> 扩展思考与某些功能不兼容，特别是消息预填写和温度。

## 扩展思考的原理

当启用扩展思考时，Claude 的响应会变为包含两个部分的结构化数据，你将同时获得推理过程和最终答案。

![img](./06a-extended-thinking.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748542429%2F08_-_001_-_Extended_Thinking_04.1748542429342.jpg)

扩展思考的主要优势包括：

- 更强大的复杂任务推理能力
- 在难题上的更高准确性
- 能了解 Claude 的思维过程

但也需要一些权衡：

- 带来更高的成本（需要为 Thinking Token 付费）
- 更高的延迟（思考需要时间）
- 代码需要处理更复杂的响应

当你已经优化了提示词，但准确性仍然没有达到你的要求时，可以考虑启用扩展思考，把它作为当标准提示词无法让你达到目标时的一种工具。

## 扩展思考的实现

首先，我们了解一下扩展思考的响应结构：

- 扩展思考的响应 Block 的 `type` 值为 `"thinking"` 
- 该 Block 携带一个签名 `signature` 来防止开发者篡改 Claude 的推理过程
- 有时你收到的 Thinking Block 是被删节的，而不是可读文本，这是因为 Claude 的思维过程被内部安全系统标记加密；你仍然可以照常使用，不会失去上下文

为了在代码中实现扩展思考，需要为 `chat` 方法添加两个参数：

```python
def chat(
    messages,
    system=None,
    temperature=1.0,
    stop_sequences=[],
    tools=None,
    thinking=False, # 是否开启思考
    thinking_budget=1024 # 思考预算，表示可用于推理的最大 Token 数，介于 1024 和 max_tokens 间
):
  # 然后组装请求参数
  if thinking:
    params["thinking"] = {
        "type": "enabled",
        "budget": thinking_budget
    }
```

当需要 Claude 处理复杂的推理任务时，扩展思考是一个强大的功能，但鉴于成本和延迟的影响，通常应谨慎使用。关键是确保你从 Baseline 提示词开始迭代，并彻底地优化，然后仅在确实需要额外推理能力时添加思考。





