# 04m - 文本编辑工具

Claude 自带了一个无需从头创建的工具：文本编辑器工具（Text Edit Tool）。这使 Claude 能够像在文本编辑器中一样处理文件和目录。

## 文本编辑工具的用途

文本编辑工具为 Claude 提供了一套全面的文件操作支持：

- 查看文件或目录内容
- 查看文件中特定行范围的内容
- 在文件中替换文本
- 创建新文件
- 在文件特定行插入文本
- 撤销最近对文件的编辑

这极大地扩展了 Claude 的功能，并从本质上赋予了它作为软件工程师的能力。

## 使用文本编辑工具

文本编辑工具的 Schema 已经集成到 Claude 中，即 Claude 知道如何请求文件操作，但实际执行这些操作的代码需要编写，即由开发者提供具体的实现。

另外，虽然完整的 Schema 已经集成到 Claude 中，在发起请求时仍需包含一个简短的 Schema Stub，它会在 Claude 侧根据模型展开为完整的 Schema。具体的 Stub 写法取决于选用的 Claude 模型，例如：

```python
def get_text_edit_schema(model):
    if model.startswith("claude-3-7-sonnet"):
        return {
            "type": "text_editor_20250124",
            "name": "str_replace_editor",
        }
    elif ...
```

> 注：每个模型的 Schema Stub 的 type 可在 https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/text-editor-tool 找到。

## 一个例子

例如，如果让 Claude 执行“打开 ./main.py 文件并总结其内容”，Claude 将：

- 使用文本编辑工具查看该文件
- 阅读内容
- 提供摘要

再例如：“打开 ./main.py 文件，并写出计算圆周率到第五位的函数，然后创建一个 ./test.py 文件来测试你的实现。”，Claude 将：

- 查看现有的 main.py 文件
- 用计算 π 的函数替换其内容
- 创建一个包含单元测试的 test.py 文件

你可能觉得，许多现代代码编辑器已经内置了 AI 助手，再有文本编辑工具存在的意义是什么。实际上，文本编辑工具在以下场景中具有明显的价值：

- 你正在构建的 AI 应用需要编程式地编辑文件
- 你的代码编辑器不支持 AI 助手
- 你想将文件编辑功能集成到你的 AI 应用中

本质上，文本编辑工具让开发者可以在应用中复刻那些 AI 驱动的代码编辑器的功能，精细地控制 Claude 与用户的文件系统交互的方式。
