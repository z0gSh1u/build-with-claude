# 01k - 控制模型输出

除了优化提示词之外，控制 Claude 的输出还可以通过如下两种方式：

- 预填写 Assistant Message
- 定义停止序列（Stop Sequence）

前者能帮助控制 Claude 如何回应，后者能控制何时停止生成文本。

## 预填写助手消息

通常助手消息应该是 Claude 自己生成的，不过如果我们提供助手消息的开头部分，则可以让 Claude 以为自己已经开始回答问题，从而引导它基于此继续生成内容。

例如下面的例子，我们预设了“咖啡更好，因为…”的开头，则 Claude 将转为解释咖啡更好的原因，而不是纠结茶和咖啡作为早餐的各自优劣。这在某种意义上限制了 Claude 的生成空间，从而能朝特定方向前进，输出更受控的内容。

![img](./01k-control-output.assets/1.png)

用代码可以表示为：

```python
messages = []
add_user_message(messages, "Is tea or coffee better at breakfast?")
add_assistant_message(messages, "Coffee is better because")
answer = chat(messages)
```

## 定义停止序列

停止序列能迫使 Claude 在生成特定字符串时立即结束响应，这适用于控制回复的长度或者终点。停止序列本身不会出现在最终响应中。实现这一点，只需要在 `create` 函数中提供 `stop_sequences` 参数即可：

```python
messages = []
add_user_message(messages, "Count from 1 to 10")
answer = chat(messages, stop_sequences=["5"])
# answer 将类似于
1, 2, 3, 4,
```

## 实际应用

这两个技巧特别适合于协助构建可靠的 AI 应用，因为它们可以：

- 统一回复格式：让回复始终以特定结构开始
- 控制回复长度
- 让回复采取特定立场
- 结构化输出：结合这两种技术来生成符合特定模板的回应

其中结构化输出是非常重要的能力，我们将在后面的内容中讨论。本节的内容在 [01k.ipynb](https://nbviewer.org/github/z0gSh1u/build-with-claude/blob/master/main/01-accessing-claude/01k.ipynb) 中实现。
