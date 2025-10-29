# 06h - 代码执行与文件 API

Anthropic API 提供了两个强大且协同工作的功能：Files API 和 Code Execution。虽然它们最初看起来是独立的，但将它们结合使用为将复杂任务委托给 Claude 开辟了一些非常有趣的可能性。

## 文件 API

Files API 提供了一种处理文件上传的替代方式。您不必直接在消息中将图像或 PDF 编码为 base64 数据，而是可以提前上传文件并在之后引用它们。

![img](./06h-code-exec-and-files-api.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748542606%2F08_-_008_-_Code_Execution_and_the_Files_API_02.1748542606050.jpg)

- 将你的文件（图像、PDF、文本等）通过单独的 API 调用上传到 Claude
- 接收一个包含唯一文件 ID 的文件元数据对象
- 在未来的消息中引用该文件 ID，而不是包含原始文件数据

这种方法在你需要多次引用同一文件，或处理较大文件（这些文件若包含在每次请求中会显得繁琐）时特别有用。

## 代码执行工具

代码执行是一个基于服务器的工具，它不需要你提供实现。你只需在请求中包含一个预定义的工具模式，Claude 就可以选择性地在一个隔离的 Docker 容器中执行 Python 代码。

![img](./06h-code-exec-and-files-api.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748542607%2F08_-_008_-_Code_Execution_and_the_Files_API_04.1748542607054.jpg)

- 在隔离的 Docker 容器中运行

- 无网络访问（无法进行外部 API 调用）
- Claude 可以在一次对话中多次执行代码
- 结果由 Claude 捕获并解释，以生成最终回复

## 组合文件 API 和代码执行工具

由于 Docker 容器没有网络访问权限，Files API 成为获取数据进出执行环境的主要方式。这是一个典型的流程：

- 使用 Files API 上传您的数据文件（如 CSV 文件）
- 在您的消息中包含一个容器上传块，并附上文件 ID
- 请让 Claude 分析数据
- Claude 编写并执行代码来处理您的文件
- Claude 可以生成您可以下载的输出（如图表）

## 实际例子

让我们以流媒体服务数据为例来看一个实际场景。CSV 文件包含用户信息，包括订阅等级、观看习惯以及他们是否已流失（取消了订阅）。

```python
# 首先，使用辅助函数上传文件：
file_metadata = upload('streaming.csv')
# 然后创建一个包含上传文件和分析请求的消息：
messages = []
add_user_message(
    messages,
    [
        {
            "type": "text",
            "text": """Run a detailed analysis to determine major drivers of churn.
            Your final output should include at least one detailed plot summarizing your findings."""
        },
        {"type": "container_upload", "file_id": file_metadata.id},
    ],
)

chat(
    messages,
    tools=[{"type": "code_execution_20250522", "name": "code_execution"}]
)
```

## 理解响应

当 Claude 使用代码执行时，响应包含多种类型的块：

- 文本块 - Claude 的分析和解释
- 服务器工具使用块 - Claude 决定实际运行的代码
- 代码执行工具结果块 - 运行代码的输出

![img](./06h-code-exec-and-files-api.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748542608%2F08_-_008_-_Code_Execution_and_the_Files_API_13.1748542608585.jpg)

Claude 可能会在单个响应中多次执行代码，迭代地构建其分析。每次执行周期都包括代码及其结果。

## 下载产物

Claude 最强大的功能之一是能够生成文件（如图表或报告），并提供下载。当 Claude 创建可视化内容时，它会存储在容器中，您可以使用 Files API 下载它。

在响应中查找带有 `type: "code_execution_output"` 的代码块——这些代码块包含生成内容的文件 ID：

```python
download_file("file_id_from_response")
```

## 更多用途

数据分析是自然而然的选择，但 Files API 与代码执行的结合开辟了许多可能性：

- 图像处理和操作
- 文档解析和转换
- 数学计算和建模
- 使用自定义格式生成报告

关键在于你可以将复杂的计算任务委托给 Claude，同时通过 Files API 保持对输入和输出的控制。这创建了一个强大的工作流程，Claude 成为你的编程助手，能够实际执行和迭代解决方案。
