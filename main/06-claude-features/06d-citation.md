# 06d - 引用

当 AI 根据提供的文档回答问题时，用户可能有疑虑：答案真的是来源于所给文档吗？还是只是从训练数据中得到的？Claude 的引用（Citation）功能能够明确指出它从哪里找到了特定信息，从而增强回答的可信度和透明度。

## 如何使用

要启用引用功能，需要在文档块中新增两个字段，`title` 和 `citations`：

```python
{
    "type": "document",
    "source": {...},
    "title": "earth.pdf", # 为你的文档提供可读的名称
    "citations": { "enabled": True } # 指示 Claude 跟踪信息来源
}
```

当启用引用时，Claude 响应的结构会变得复杂。你得到的将不再是简单的文本，而是包含引用信息的结构化数据：

![img](./06d-citation.assets/1.jpg)

具体来说，每个引用包含如下关键信息：

- cited_text：能支持 Claude 的说法的引用文本
- document_index：引用的文档序号（在指定多个文档时有用）
- document_title：引用的文档的 title
- start_page_number：引用的起始页码
- end_page_number：引用的结束页码

引用不仅可以基于 PDF 文档，也可以使用纯文本，此时文档块结构类似：

```python
{
    "type": "document",
    "source": {
        "type": "text",
        "media_type": "text/plain",
        "data": article_text, # 这里是文本的内容
    },
    "title": "earth_article",
    "citations": { "enabled": True }
}
```

对应地，响应将包含字符位置而不是页码，从而精确地指出 Claude 在文本中找到的信息的具体位置。

## UI 设计

在涉及引用时，UI 方面，可以允许用户悬停在引用标记上了解信息的确切来源。这使得用户可以：

- 看到 Claude 的回答是基于实际材料
- 检查原始文档来验证信息
- 了解引用信息周围的上下文

## 适用场景

引用在以下情况特别有价值：

- 用户需要核实信息的准确性
- 处理用户所参考的权威文件
- 信源的透明度对你的应用很重要
- 用户想要探索某事实的更广泛背景知识

通过引用，Claude 从一个只提供答案的“黑箱”转变为一个透明的助手，从而建立用户信任，并让用户在需要时能深入资料来源。
