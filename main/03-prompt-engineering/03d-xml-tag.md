# 03d - 使用 XML 标签

当你在构建包含大量内容的提示时，Claude 有时会难以理解哪些文本片段应该放在一起，或者不同的部分应该代表什么。XML 标签提供了一种简单的方法来为你的提示添加结构和清晰度，尤其是在你插入大量数据时。

## 为什么需要组织结构

考虑一个需要分析 20 页销售记录的提示。如果没有清晰的界限，Claude 可能会难以区分你的指令和实际想要分析的数据。上面的例子展示了不清晰的边界如何使 Claude 难以解析您的意图。通过将销售记录包裹在 `<sales_records>` 和 `</sales_records>` 等 XML 标签中，您创建了清晰的分隔符，这有助于 Claude 理解您的提示结构。

![img](./03d-xml-tag.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748623569%2F05_-_004_-_Structure_with_XML_Tags_06.1748623569098.png)

## 一个例子

这是一个更戏剧性的例子，说明了 XML 标签的重要性。如果你让 Claude 使用提供的文档来调试代码，将所有内容混在一起会造成混淆：

"不太好用"版本几乎无法区分代码和文档。"更好"版本使用 `<my_code>` 和 `<docs>` 标签来创建清晰的界限。

![img](./03d-xml-tag.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748623570%2F05_-_004_-_Structure_with_XML_Tags_10.1748623569882.png)

## 如何使用

你不需要使用官方的 XML 标签。为你的内容创建描述性的名称：

- `<sales_records>` is better than `<data>`
  `<sales_records>` 比 `<data>` 更好
- `<athlete_information>` clearly identifies user details
  `<athlete_information>` 清晰地标识用户详细信息
- `<my_code>` and `<docs>` separate different types of content
  `<my_code>` 和 `<docs>` 分隔不同类型的内容

你的标签名称越具体和描述性，Claude 就越能理解每个部分的目的。

XML 标签在以下情况最为有用：

- 包含大量上下文或数据
- 混合不同类型的内容（代码、文档、数据）
- 你想特别清楚地了解内容边界
- 处理包含多个变量插值的复杂提示

即使是较短的文本，XML 标签也可以作为分隔符，使你的提示结构对 Claude 更加清晰。

## 实际应用

```
<athlete_information>
- Height: 6'2"
- Weight: 180 lbs
- Goal: Build muscle
- Dietary restrictions: Vegetarian
</athlete_information>

Generate a meal plan based on the athlete information above.
```

这使得非常明确：身高、体重、目标和限制都是与运动员相关的数据，在生成饮食计划时应综合考虑。虽然简单的提示可能不会带来显著的改进，但随着提示变得越来越复杂并包含更多样化的内容，XML 标签的价值会越来越高。

