# 05e - 实现 RAG 流程

现在我们已经从概念上理解了 RAG 流程，让我们逐步实现它。我们将通过一个完整的示例来演示如何分块文本、生成嵌入、将它们存储在向量数据库中，以及执行相似性搜索。

## 步骤 1：文本分块

首先，我们加载文档并将其分割成可管理的部分：

```python
with open("./report.md", "r") as f:
    text = f.read()

chunks = chunk_by_section(text)
chunks[2]  # Test to see the table of contents
```

我们使用之前相同的 `chunk_by_section` 函数将文档分割成逻辑部分。

## 步骤 2：生成嵌入

接下来，我们一次性为所有我们的片段创建嵌入：

```python
embeddings = generate_embedding(chunks)
```

嵌入函数已更新，现在可以处理单个字符串和字符串列表，从而提高了批量处理的效率。

## 步骤 3：存储在向量数据库中

现在我们创建我们的向量存储，并用嵌入及其相关文本填充它：

```python
store = VectorIndex()

for embedding, chunk in zip(embeddings, chunks):
    store.add_vector(embedding, {"content": chunk})
```

请注意，我们既存储了嵌入向量，也存储了原始文本内容。这是至关重要的，因为在后续搜索时，我们需要返回实际的文本内容，而不仅仅是数值型的嵌入向量。

当我们查询我们的向量数据库时，仅仅返回嵌入数字是没有用的。我们需要实际用于生成这些嵌入的文本。这就是为什么我们在数据库中的每个嵌入旁边都包含原始文本片段（或者至少是它的引用）。

## 步骤 4：处理用户查询

当用户提问时，我们会为其查询生成一个嵌入：

```python
user_embedding = generate_embedding("What did the software engineering dept do last year?")
```

## 步骤 5：查找相关内容

最后，我们搜索我们的向量存储库以找到最相似的片段：

```python
results = store.search(user_embedding, 2)

for doc, distance in results:
    print(distance, "\n", doc["content"][0:200], "\n")
```

这个搜索返回了两个最相关的片段及其相似度分数（余弦距离）。搜索结果向我们展示了哪些文档部分与用户的问题最相关，并附有相似度分数。

## 更进一步

这种实现方式适用于基本案例，但在某些情况下表现并不理想。在接下来的部分中，我们将探讨改进措施，以使我们的 RAG 系统更加稳健和准确。

关键要点在于，RAG 本质上是将文本转换为数字（嵌入），高效存储这些数字，然后使用数学相似性在用户提问时找到相关内容。
