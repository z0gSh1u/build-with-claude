# 02d - 运行评测

在完成了评测集的构建后，可以开始运行评测了。评测的流程可以抽象为三个阶段。

- run_prompt：接收一个测试用例，与提示词模板合并，送给 Claude，得到输出。
- run_test_case：运行单个测试用例，并打分
- run_eval：遍历所有测试用例，协调整个测试过程

本节的代码在 [02d.ipynb](./02d.ipynb)，其中我们先固定打分为 10 分来走通流程，后续会介绍打分器的具体设计。

![img](./02d-run-eval.assets/instructor%2Fa46l9irobhg0f5webscixp0bs%2Fpublic%2F1748623397%2F04_-_004_-_Running_the_Eval_18.1748623397308-1760977880853-3.png)

评测返回一个 JSON 数组，其中每个对象包含三部分关键信息：

- output：来自 Claude 的响应
- test_case：原始测试用例
- score：评估分数

这个流程代了大多数 AI 评测系统的基础，而复杂性则将体现在细节上，包括更好的提示词、复杂的评分机制、性能优化。
