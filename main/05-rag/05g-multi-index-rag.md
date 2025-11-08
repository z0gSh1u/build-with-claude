# 05f - 多索引 RAG

我们已为语义搜索（使用嵌入）和词法搜索（使用 BM25）分别实现了方案。这一小节将演示如何将它们结合成统一的搜索流程。

## 多索引架构

我们的 `VectorIndex` 和 `BM25Index` 类具有几乎完全相同的 API —— `add_document` 和 `search` 方法，这种抽象使得将它们包装在一起形成新的 `Retriever` 类变得简单。Retriever 充当协调者，将用户查询转发给两个索引搜索方法，收集它们的结果，并使用“倒数排序融合”（Reciprocal Rank Fusion，RRF）技术将它们合并。

## RRF

不同搜索方法使用不同的评分体系，需要一种方法来公平地归一化它们的排名，才能合并它们的结果。

![img](05g-multi-index-rag.assets/1.jpg)

RRF 的工作原理如下，假设搜索关于“INC-2023-Q4-011”的信息，并得到以下结果：

- `VectorIndex` 返回：第 2 节（排名 1），第 7 节（排名 2），第 6 节（排名 3）
- `BM25Index` 返回：第 6 节（排名 1），第 2 节（排名 2），第 7 节（排名 3）

![img](05g-multi-index-rag.assets/2.jpg)

我们将这些结果合并到一个表格中，展示每个文本块在两个索引中的排名，然后应用 RRF 公式：

$$
{\rm RRF_{score}}(d) = \sum_i \frac{1}{k+{\rm rank}_i(d)}
$$

其中 $k$ 是一个常数（通常为 60，但为了演示我们使用 1），${\rm rank}_i(d)$是文档 d 在第 $i$ 种排名方式中的名次：

![img](05g-multi-index-rag.assets/3.jpg)

我们得到最终排序结果为：第 2 节（0.833），第 6 节（0.75），第 7 节（0.583）。这很符合直觉——第 2 节在两种索引中都表现良好，因此排在最前面。

## 实现细节

`Retriever` 类封装了多种搜索方法，并提供了一个统一的接口：

```python
class Retriever:
    def __init__(self, *indexes: SearchIndex):
        if len(indexes) == 0:
            raise ValueError("At least one index must be provided")
        self._indexes = list(indexes)

    def add_document(self, document: Dict[str, Any]):
        for index in self._indexes:
            index.add_document(document)

    def search(self, query_text: str, k: int = 1, k_rrf: int = 60):
        all_results = ... # 从所有索引方法种得到结果
        for idx, results in enumerate(all_results):
            for rank, (doc, _) in enumerate(results):
                ... # 进行 RRF 计算
        # 返回排序后的结果
```

通过在不同搜索实现中保持一致的 API，可以将它们轻松地、低耦合地组合起来。

## 测试混合方法

当搜索“INC-2023-Q4-011 发生了什么？”时，通过混合检索器，我们现在得到了更好的结果：

- 第 10 节：网络安全分析 - 事件响应报告（最相关）
- 第 2 节：软件工程 - 项目凤凰稳定性增强（第二相关）
- 第 5 节：法律发展（三）

即通过结合语义搜索和词汇搜索来克服单一方法的局限性。

## 可扩展性

![img](05g-multi-index-rag.assets/4.jpg)

由于所有索引方法都实现了相同的 SearchIndex 接口，包含 `add_document` 和 `search` 方法，因此可以轻松地添加新的搜索方法，例如基于关键词、基于图的搜索、领域特定的索引等。这种模块化方法使每种搜索实现保持专一且可测试，将它们的优势结合到最终的 RAG 系统中。
