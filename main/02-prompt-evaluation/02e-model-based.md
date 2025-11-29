# 02e - 基于模型的打分

从前面的内容可以知道，提示词评估过程中，打分器提供了关键的客观信号。目前有三种主要的评估模型输出的方法：

| 类型             | 方法                       | 适合场景                                                                                            |
| ---------------- | -------------------------- | --------------------------------------------------------------------------------------------------- |
| 基于代码的打分器 | 使用编程逻辑评估输出       | 程序性检查，例如输出长度、是否包含特定单词、结构化语法验证、可读性评分等                            |
| 基于模型的打分器 | 使用另一个 AI 模型评估输出 | 提供了更大的灵活性，适合评估响应质量、指令遵循（Instruction Following）质量、完整性、实用性、安全性 |
| 人工打分器       | 人工手动审查和评估         | 提供最大的灵活性，但但耗时且繁琐，适合评估全面性、深度、简洁性、相关性                              |

本节将主要介绍基于模型的打分器。

## 定义评估标准

需要先确定评估标准，才能指导选择哪一种打分器。例如，对于一个代码生成任务，可能关注如下方面：

- 格式：例如仅返回 Python 代码，无需解释
- 语法有效性
- 任务遵循：响应是否直接针对用户任务并使用准确代码

前两个标准与基于代码的打分器配合良好，而任务遵循则更适合基于模型的打分器。

![img](./02e-model-based.assets/1.png)

## 实现基于模型的打分器

关键函数如下：

````python
def grade_by_model(test_case, output):
    task, solution = test_case, output
    # 写一个用于打分的 Prompt
    eval_prompt = f"""
    You are an expert code reviewer. Evaluate this AI-generated solution.

    Task: {task}
    Solution: {solution}

    Provide your evaluation as a structured JSON object with:
    - "strengths": An array of 1-3 key strengths
    - "weaknesses": An array of 1-3 key areas for improvement
    - "reasoning": A concise explanation of your assessment
    - "score": A number between 1-10
    """
    messages = []
    add_user_message(messages, eval_prompt)
    add_assistant_message(messages, "```json")
    eval_text = chat(messages, stop_sequences=["```"])
    return json.loads(eval_text)
````

关键是询问 solution 的强项、弱项，提供解释，并给出分数。不要要求打分模型直接给出分数，而不带任何前面的思考性或解释性信息，否则分数会趋于在中庸的 6 分左右徘徊。

随后即可将打分器集成到提示词评估的流程中了。这部分代码实现在 [02e.ipynb](https://github.com/z0gSh1u/build-with-claude/blob/master/main/01-accessing-claude/02e.ipynb)。
