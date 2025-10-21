# 06c - PDF 理解

Claude 可以直接读取和分析 PDF 文件，这意味着它是强大的文档处理工具。Claude 能分析和理解：

- 文本内容
- 嵌入 PDF 中的图片和图表
- 表格及其数据关系
- 文档结构和格式

这使 Claude 能处理摘要、数据分析、特定内容提取等各种任务。

## 如何使用

与图像类似地，需要一个 `type` 为 `document` 的 Block：

```python
# 将 PDF 读取为 Base64
with open("earth.pdf", "rb") as f:
    file_bytes = base64.standard_b64encode(f.read()).decode("utf-8")

add_user_message(
    messages,
    [
        {
            "type": "document", # 注意这里的类型与图像不同
            "source": {
                "type": "base64",
                "media_type": "application/pdf", # 注意这里的类型也与图像不同
                "data": file_bytes,
            },
        },
        {"type": "text", "text": "Summarize the document in one sentence"},
    ],
)
```

本节的代码在 [06c.ipynb](./06c.ipynb)，使用 Claude 处理了一个保存自维基百科的 PDF，用一句话总结了复杂的文档内容。

![img](./06c-pdf.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748542484%2F08_-_003_-_PDF_Support_02.1748542484779.jpg)

