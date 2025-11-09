# 05h - 重排序

我们构建的混合检索方法已经不错，但仍存在一些问题：当搜索“工程团队如何处理 INC-2023-Q4-011？”时，我们期望软件工程部分排名更高，因为它特别提到了“工程团队”和“事件”；然而当前系统仍首先返回网络安全部分。

重排序（Rerank）是一种通过增加另一个后处理步骤来提高检索准确性的技术，我们将使用它来解决这个问题。

## 如何工作

在运行我们的向量检索和 BM25 检索并将结果合并后，重排序添加了一个额外步骤：使用 Claude 智能地重新排序搜索结果。

![img](05h-rerank.assets/1.jpg)

具体过程是这样的：

- 提取混合检索的合并结果
- 将它们连同用户的原始问题一起发送给 Claude
- 要求 Claude 按相关性递减的顺序返回最相关的文档
- 将 Claude 重新排序后的列表作为最终结果

提示词结构很简单，就是向 Claude 提供用户问题以及所有可能相关的文档，然后要求完成一个任务：

```
你的任务是找到与用户问题最相关的文档。

<user_question>
What happened with INC-2023-Q4-011?
</user_question>

这里是一些可能相关的文件文档
<documents>
  <document>Section 10...</document>
  <document>Section 2...</document>
  <document>Section 7...</document>
  <document>Section 6...</document>
</documents>

返回按相关性降序排列的前3份最相关的文档。
```

## 效率

在实现重排序时，如果让 Claude 返回每个相关片段的完整文本，是慢而且浪费的。更好的方法是事先为每个文本片段分配一个 ID，然后让 Claude 按顺序返回这些 ID：

```
<documents>
  <document>
    <id>ab84</id>
    <content>Section 10...</content>
  </document>
  <document>
    <id>51n3</id>
    <content>Section 8...</content>
  </document>
</documents>
```

Claude 返回一个简单的列表，如 `["1p5g", "51n3", "ab83"]` ，而不是复制整个文本块。

## 实现

重排序函数在混合检索完成后由 `Retriever` 自动调用：

````python
def reranker_fn(docs, query_text, k):
    # 组装文档，分配 ID
    joined_docs = "\n".join([
        f"""
        <document>
          <document_id>{doc["id"]}</document_id>
          <document_content>{doc["content"]}</document_content>
        </document>
        """
        for doc in docs
    ])
    # 组装 Prompt
    prompt = f"""..."""
    messages = []
    add_user_message(messages, prompt)
    add_assistant_message(messages, """```json""")
    # 处理结果
    result = chat(messages, stop_sequences=["""```"""])
    
    return json.loads(result["text"])["document_ids"]
````

## 权衡

重排序的使用伴随着权衡：

- 延迟增加：需要等待额外的 Claude 调用
- 准确度提高：Claude 能够进一步理解上下文和相关性，这是向量相似性等检索方法不易做到的

对于大多数应用来说，增加的延迟比起提升的准确度来说是值得的，尤其是在处理复杂的查询时，语义理解比纯关键词匹配更重要。
