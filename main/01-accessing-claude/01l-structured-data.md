# 01l - 结构化数据

如果 Claude 能够直接生成 JSON、Python 代码等结构化数据，将非常有助于我们消费它的输出。但通常，Claude 为了帮助你阅读和理解它的输出，会在内容周围添加解释性文本，或者 Markdown 代码块标记。

本节将介绍如何让它只输出符合预期的原始数据，不要添加其他东西。

## 一个例子

本节的例子是让 Claude 生成一个 AWS EventBridge Rule（一种 JSON）。如果不做任何处理，输出将类似：

````
```json
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": {
    "state": ["running"]
  }
}
```

This rule captures EC2 instance state changes when instances start running.
````

这导致下游需要进行手工提取，而 ad-hoc 编写的提取逻辑通常都不够可靠。解决这个问题的方式有很多，这里将提供一种基于预填写助手消息和定义停止序列的方案，用代码描述如下：

````python
messages = []
add_user_message(messages, "Generate a very short event bridge rule as json")
add_assistant_message(messages, "```json")
text = chat(messages, stop_sequences=["```"])
````

其原理很好理解：

- 用户消息告诉 Claude 要生成什么内容
- 预填写助手消息让 Claude 从一个 Markdown 的 JSON 代码块的主体内容写起
- 当 Claude 试图结束代码块时，立即停止生成

把结果 strip 后就得到干净的 JSON，没有额外的格式：

```json
{
  "source": ["aws.ec2"],
  "detail-type": ["EC2 Instance State-change Notification"],
  "detail": {
    "state": ["running"]
  }
}
```

这个例子实现在 [01l.ipynb](https://github.com/z0gSh1u/build-with-claude/blob/master/main/01-accessing-claude/01l.ipynb) 中。

## 举一反三

这种技术不仅限于 JSON 生成。当你需要结构化数据而不需要注释时，随时可以使用，例如：

- Python 代码片段
- 有序/无序列表
- CSV 数据

关键在于识别 Claude 默认地想要包裹内容的格式，然后基于该格式设计预填充和停止序列。
