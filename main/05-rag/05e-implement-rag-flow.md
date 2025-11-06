# 05e - 实现 RAG 流程

我们已经从概念上理解了 RAG 流程，现在将通过一个完整的示例来展示如何分块、生成嵌入、存储到向量数据库中，并执行相似性搜索。

## 步骤 1：文本分块

首先，打开文档并将其分割成 Chunk：

```python
with open("./report.md", "r") as f:
    text = f.read()

chunks = chunk_by_section(text)
```

我们使用之前相同的 `chunk_by_section` 函数将文档分割成逻辑部分。

## 步骤 2：生成嵌入向量

接下来，一次性为所有的片段创建嵌入向量：

```python
embeddings = generate_embedding(chunks)
```

## 步骤 3：存储到向量数据库

现在我们创建向量存储，并存入嵌入及其相关的文本：

```python
store = VectorIndex()

for embedding, chunk in zip(embeddings, chunks):
    store.add_vector(embedding, { "content": chunk })
```

注意，我们既存储了嵌入向量，也存储了原始文本内容（或者至少是它的引用）。因为在后续搜索时，需要返回实际的文本内容，而不仅是嵌入向量。

## 步骤 4：处理用户查询

当用户提问时，为其查询也生成一个嵌入向量：

```python
user_embedding = generate_embedding("What did the software engineering dept do last year?")
```

## 步骤 5：查找相关内容

最后，搜索向量存储库以找到最相似的片段：

```python
results = store.search(user_embedding, 2)

for doc, distance in results:
    print(distance, "\n", doc["content"][0:200], "\n")
```

这个搜索返回了两个最相关的片段及其相似度分数（距离）。

## 更进一步

RAG 本质上是将文本转换为数字（嵌入向量），高效存储这些向量，然后使用数学工具（相似性）在用户提问时找到相关内容。目前这种实现方式适用于基础的情况，但在稍微复杂的情况下表现并不理想。接下来，我们将探讨改进措施，以使 RAG 系统更加稳健和准确。
