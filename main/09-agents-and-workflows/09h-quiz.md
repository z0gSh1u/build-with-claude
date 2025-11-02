# 09h - 小测验

Question 1: Correct answer
问题 1：正确答案

You're building an agent with tools. Which approach will give Claude the most flexibility to handle unexpected requests?
你正在构建一个带有工具的智能体。哪种方法将给 Claude 提供最大的灵活性来处理意外请求？

 Give Claude only one powerful tool
只给 Claude 提供一个强大的工具

 Provide very specific tools like "write_python_function" and "debug_code"
提供非常具体的工具，如“write_python_function”和“debug_code”

 **Provide abstract tools like "read_file", "write_file", and "run_command"
提供抽象的、原子化的工具，如“read_file”、“write_file”和“run_command”**

 Provide tools that only work for planned scenarios
提供仅适用于计划场景的工具

Question 2: Correct answer
问题 2：正确答案

You want Claude to write a report, then check if it's good enough, and improve it if needed. What pattern are you using?
您想让 Claude 写一份报告，然后检查它是否足够好，如果需要的话进行改进。您使用的是什么模式？

 Chaining workflow 工作流链式

 **Evaluator-Optimizer pattern
评估-优化模式**

 Parallelization workflow
并行化工作流

 Routing workflow 路由工作流

Question 3: Correct answer
问题 3：正确答案

Your app generates different types of social media content. Programming topics need educational scripts, while sports topics need entertainment-focused content. What pattern should you use?
您的应用生成不同类型的社会媒体内容。编程主题需要教育性脚本，而体育主题需要以娱乐为重点的内容。您应该使用什么模式？

 **Route requests to specialized processing pipelines
将请求路由到专门的加工管道**

 Use the same prompt for everything
对所有内容使用相同的提示

 Ask users to write their own content
让用户自己编写内容

 Always use the entertainment approach
始终采用娱乐方法

Question 4: Correct answer
问题 4：正确答案

Claude keeps ignoring some of your rules when you give it a long prompt with many requirements. What workflow approach would help?
Claude 在接收到一个包含许多要求的长时间提示时，会忽略一些你的规则。有什么工作流方法能有所帮助？

 Make the prompt even longer with more rules
将提示变得更长，加入更多规则

 Run everything in parallel
并行运行所有任务

 Use a routing workflow to categorize first
使用路由工作流先进行分类

 **Chain the task into focused sequential steps
将任务链成专注的顺序步骤**

Question 5: Correct answer
问题 5：正确答案

You need Claude to recommend the best material for a part by considering metal, plastic, ceramic, and wood options. Each material has different criteria. What's the best approach?
您需要 Claude 根据金属、塑料、陶瓷和木材选项推荐最适合某个部件的材料。每种材料都有不同的标准。最佳的方法是什么？

 Chain the evaluations one after another
依次连锁评估

 Ask Claude to pick randomly
让 Claude 随机选择

 **Send separate requests for each material type in parallel
并行发送针对每种材料类型的单独请求**

 Put all criteria in one big prompt
将所有标准放入一个大的提示中

Question 6: Correct answer
问题 6：正确答案

You need to choose between a workflow and an agent for your app. Reliability and predictable results are most important to you. Which should you pick?
您需要为您的应用选择工作流或代理。可靠性和可预测的结果对您来说最为重要。您应该选择哪一个？

 Always use an agent for maximum flexibility
始终使用代理以获得最大灵活性

 **Use a workflow since it's more reliable and testable
使用工作流，因为它更可靠且可测试**

 Combine both approaches equally
两种方法同等结合

 Use whichever is easier to code
使用编写起来更简单的那个

Question 7: Correct answer
问题 7：正确答案

You're building an app where users upload photos of damaged car parts and always get repair cost estimates. You know exactly what steps are needed each time. What should you use?
您正在开发一个应用程序，用户上传损坏的汽车部件照片，并始终获得维修成本估算。您知道每次都需要哪些步骤。您应该使用什么？

 Multiple agents working together
多个协同工作的智能体

 A single complex prompt
一个单一的复杂提示

 An agent with many specialized tools
一个拥有许多专业工具的智能体

 **A workflow with predetermined steps
一个具有预定步骤的工作流程**

