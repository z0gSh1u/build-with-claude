# 05h - 重排序

我们构建的混合检索方法运行良好，但仍存在一些弱点。当搜索"工程团队如何处理 INC-2023-Q4-011？"时，我们可能会期望软件工程部分排名更高，因为它特别提到了工程团队和事件。然而，当前系统仍然首先返回网络安全部分。

这就是重排序发挥作用的地方——一种通过增加另一个后处理步骤来提高检索准确性的技术。

## 如何工作

重新排序在概念上很简单。在运行你的向量索引和 BM25 索引并将结果合并后，你添加了一个额外的步骤：一个使用 Claude 智能重新排序你搜索结果的重新排序器。

![img](05h-rerank.assets/1.jpg)

这个过程是这样的：

- 取自你混合搜索的合并结果
- 将它们连同用户原始问题一起发送给 Claude
- 要求 Claude 按相关性递减的顺序返回最相关的文档
- 使用 Claude 重新排序的列表作为你的最终结果

提示结构很简单。你向 Claude 提供用户的问题以及所有看似相关的文档，然后要求一个简单的任务：

```
You are tasked with finding the documents most relevant to a user's question.

<user_question>
What happened with INC-2023-Q4-011?
</user_question>

Here are documents that may be relevant:
<documents>
<document>Section 10...</document>
<document>Section 2...</document>
<document>Section 7...</document>
<document>Section 6...</document>
</documents>

Return the 3 most relevant docs, in order of decreasing relevance.
```

## 效率

在实现重排序时有一个重要的效率考虑。如果你让 Claude 返回每个相关片段的完整文本，你实际上是在要求它将大量文本复制回给你。这是浪费且慢的。更好的方法是事先为每个文本片段分配一个唯一 ID，然后让 Claude 只按正确顺序返回这些 ID：

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

Claude 然后可以返回一个简单的列表，如 `["1p5g", "51n3", "ab83"]` ，而不是复制整个文本块。

## 实现

重排序函数在初始混合搜索完成后由您的检索器自动调用。这是基本结构：

````python
def reranker_fn(docs, query_text, k):
    # Format documents with IDs
    joined_docs = "\n".join([
        f"""
        <document>
        <document_id>{doc["id"]}</document_id>
        <document_content>{doc["content"]}</document_content>
        </document>
        """
        for doc in docs
    ])

    # Create prompt and get Claude's response
    prompt = f"""..."""
    messages = []
    add_user_message(messages, prompt)
    add_assistant_message(messages, """```json""")

    result = chat(messages, stop_sequences=["""```"""])

    return json.loads(result["text"])["document_ids"]
````

## 结果

在用“what did the eng team do with INC-2023-Q4-011?”测试重新排序方法时，软件工程部分现在出现在结果的第一位。Claude 成功识别出用户的查询特别关注工程团队及其与事件的关系。

## 权衡

重新排序伴随着明显的权衡：

- 延迟增加：现在您必须等待额外的 API 调用以获取 Claude 的响应
- 准确度提高：Claude 能够以纯向量相似性无法做到的方式理解上下文和相关性

对于大多数应用来说，准确度的提升值得延迟的成本，尤其是在处理复杂查询时，语义理解比纯关键词匹配更重要。
