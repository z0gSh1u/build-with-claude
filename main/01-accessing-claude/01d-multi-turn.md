# 01d - 多轮对话

在与 Claude 交互时，有个有些反直觉的事情：Claude 不会存储任何对话历史（无状态），每次请求都是完全独立的。这意味着对话状态（也就是 messages 列表）需要我们自己处理，并且在每次请求时携带完整的消息历史，以保持对话上下文。

> 实际上，流行的 OpenAI Completion API 也是如此，不过他们推出了 Response API 来解决这个问题。

简单来说，一场多轮对话的有效流程是：

- 将第一条 User Message 发给 Claude 并添加到 messages
- 将 Claude 响应的 Assistant Message 添加到 messages
- 将下一条 User Message 继续添加到 messages
- 将 messages 整个发送给 Claude 获取下一条 Assistant Message，以此类推

我们在 [01d.ipynb](https://nbviewer.org/github/z0gSh1u/build-with-claude/blob/master/main/01-accessing-claude/01d.ipynb) 中实现了这个逻辑。你可以看到 Claude 明白我们的第二个用户消息“写另一句话”是指扩展量子计算的定义，因为你提供了完整的对话上下文。
