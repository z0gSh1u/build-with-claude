# 01c - 发起请求

我们将基于 Anthropic 的 Python SDK 来完成。你可以基于本项目根目录的 pyproject.toml 初始化虚拟环境，并将 Jupyter Notebook 执行时使用的 Python 环境指向所创建的 .venv 目录下的对应文件。以下是基于 uv 的示例命令。

```sh
# 在根目录初始化环境
uv sync
# 在根目录 .env 文件配置环境变量
ANTHROPIC_API_KEY="your-api-key-here"
BASE_URL="https://somewhere.com/v1"
# 在当前目录打开 Notebook
jupyter notebook .
```

请结合 [01c.ipynb](https://nbviewer.org/github/z0gSh1u/build-with-claude/blob/master/main/01-accessing-claude/01c.ipynb) 阅读后续的内容。

## `create` 函数

对于 Anthropic 标准的 API（这与 OpenAI API、Gemini API 略有不同），发起 API 请求的核心函数是 `client.messages.create`，包含三个关键参数：

- model：选用的模型
- max_tokens：对响应的最大 Token 数限制
- messages：对话历史

## 消息 Message

我们最经常打交道的是 message，用于表示用户和 Claude 之间的对话。有两类最基本的消息类型：

- 用户消息 User Message：人类编写的发送给 Claude 的内容
- 助手消息 Assistant Message：Claude 生成的回复

每条 message 是一个类似如下结构的 dict：

```json
{
    role: "user" | "assistant", // 消息类型
    content: string, // 实际的消息文本
}
```

后面我们会看到更多的消息类型和更复杂的消息结构，不过目前知道这些就够了。

## 发送请求并提取响应

只需要一个函数调用即可：

```python
message = client.messages.create(
    model=model,
    max_tokens=1000,
    messages=[
        {
            "role": "user",
            "content": "What is quantum computing? Answer in one sentence"
        }
    ]
)
```

响应如下，包含许多字段，例如 content（模型生成的文本）、stop_reason、usage（用量信息）。其中最常用的是 Claude 生成的文本，即 `message.content[0].text`。同时还告诉我们，输入占 16 个 Token，输出占 42 个 Token。

```
{'content': [{'text': 'Quantum computing is a revolutionary computing paradigm '
                      'that uses quantum mechanical phenomena like '
                      'superposition and entanglement to process information '
                      'in ways that can potentially solve certain problems '
                      'exponentially faster than classical computers.',
              'type': 'text'}],
 'id': 'msg_bdrk_0181VY7YSHPvWqB3b7ZX3q3E',
 'model': 'claude-sonnet-4-20250514',
 'role': 'assistant',
 'stop_reason': 'end_turn',
 'stop_sequence': None,
 'type': 'message',
 'usage': {'cache_creation_input_tokens': 0,
           'cache_read_input_tokens': 0,
           'input_tokens': 16,
           'output_tokens': 42}}
```

这些 Token 在当前模型下大概要花掉 0.005 RMB。而你也已经以这样简单的方式，正式进入了与 Claude 交互的魔法世界。
