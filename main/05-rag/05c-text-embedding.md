# 05c - 文本嵌入

在将文档分割成块之后，RAG 流程的下一步是找出哪些块与用户的提问最相关。这本质上是一个搜索问题——你需要浏览所有文本块，并识别出与用户所问内容相关的块。

![img](./05c-text-embedding.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748542211%2F07_-_003_-_Text_Embeddings_03.1748542211434.jpg)

## 语义搜索

查找相关块最常用的方法是语义搜索。与基于关键词的搜索（寻找精确的单词匹配）不同，语义搜索使用文本嵌入来理解用户提问和每个文本块的含义和上下文。

## 文本嵌入

文本嵌入是文本中所包含意义的数值表示。可以将其视为将词语和句子转换为计算机可以数学处理格式的过程。

![img](./05c-text-embedding.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748542212%2F07_-_003_-_Text_Embeddings_07.1748542212576.jpg)

这是该过程的工作原理：

- 您将文本输入嵌入模型
- 模型输出一个长数字列表（嵌入），每个数字的范围在-1 到+1 之间
- 这些数字代表输入文本的不同品质或特征

嵌入中的每个数字本质上是对输入文本某种质量的"评分"。然而，这里有一个重要的注意事项：我们不知道每个数字具体代表什么。虽然想象一个数字可能代表“文本有多快乐”或“文本谈论海洋的程度”很有帮助，但这些只是概念性例子。每个维度的实际含义是模型在训练过程中学习的，人类无法直接理解。

## 使用 VoyageAI 实现嵌入

由于 Anthropic 目前不提供嵌入生成服务，推荐使用 VoyageAI。

```sh
VOYAGE_API_KEY="your_key_here"
```

代码：

```python
from dotenv import load_dotenv
import voyageai

load_dotenv()
client = voyageai.Client()

def generate_embedding(text, model="voyage-3-large", input_type="query"):
    result = client.embed([text], model=model, input_type=input_type)
    return result.embeddings[0]
```

当你在这个函数上运行一个文本块时，你会得到一个浮点数列表，代表嵌入。这个过程快速且简单——真正的挑战在于理解如何有效地将这些嵌入用于你的 RAG 管道，以找到最相关的内容。

![img](./05c-text-embedding.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748542214%2F07_-_003_-_Text_Embeddings_19.1748542214605.jpg)

下一步是学习如何比较嵌入来确定哪些片段与用户的提问最相似，这是语义搜索过程的核心。
