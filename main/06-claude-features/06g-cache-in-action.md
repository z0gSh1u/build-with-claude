# 06g - 缓存实践

## 提示词缓存的工作原理

当启用提示词缓存时，第一个请求会将内容写入一个生命周期为 1 小时的缓存中，后续请求可以读取此缓存，而不是再次处理相同内容。这在发送以下内容时很有价值：

- 大型的系统提示词
- 复杂的工具 Schema
- 重复的消息内容

缓存只有在反复发送相同内容时才有效。但在许多 AI 应用中，这种情况很常见。

### 缓存工具 Schema

需要在 `tools` 的最后一个工具中添加 `cache_control` 字段：

```python
if tools:
    tools_clone = tools.copy()
    last_tool = tools_clone[-1].copy()
    last_tool["cache_control"] = {"type": "ephemeral"}
    tools_clone[-1] = last_tool
    params["tools"] = tools_clone
```

### 缓存系统提示词

需要在文本 Block 中添加相关参数：

```python
if system:
    params["system"] = [
        {
            "type": "text",
            "text": system,
            "cache_control": {"type": "ephemeral"}
        }
    ]
```

## 缓存的行为

当请求启用缓存时，响应中会存在不同的用量格式：

- 首次请求：`cache_creation_input_tokens=1772`，表示 Claude 写入缓存
- 后续请求：`cache_read_input_tokens=1772`，表示 Claude 读取缓存
- 当发生内容变更时，新的 cache_creation_input_tokens 记录会出现

缓存非常敏感。即使工具或系统提示词中更改一个字符，也会使部分的整个缓存失效。

## 缓存排序和断点

你可以在单个请求中设置多个缓存断点，假设我们按照如下的结构断点：

- 工具（如果有）
- 系统提示词（如果有）
- 消息

如果更改系统提示词但工具不变，你会看到针对工具部分的缓存读取和针对新的系统提示词部分的缓存写入。这种细粒度的缓存策略意味着你只需为实际更改的部分付费。

当存在以下情况时，提示词缓存很有效：

- 跨请求的一致的工具 Schema
- 稳定的系统提示词
- 应用需要使用相似的上下文进行多次请求

需要记住的是，缓存仅持续 1 小时，因此它是为频繁使用 API 的应用而设计的。

