# 07l - 小测验

Question 1: Correct answer
问题 1：正确答案

You've created an MCP server and want to test your tools before connecting them to Claude. What's the best way to do this?
你已经创建了一个 MCP 服务器，希望在将其连接到 Claude 之前测试你的工具。最好的方法是什么？

 Testing isn't needed for tools, Claude can figure out how to use them
测试工具不需要，Claude 可以自行判断如何使用

 Connect to Claude immediately
立即连接到 Claude

 **Use the MCP Inspector in your browser
在浏览器中使用 MCP 检查器**

 Test in production 在生产环境中测试

Question 2: Correct answer
问题 2：正确答案

You're building a document system where users can type @document_name to reference files. What MCP feature is best for exposing the document contents?
你正在构建一个文档系统，用户可以输入@document_name 来引用文件。哪个 MCP 功能最适合展示文档内容？

 Tools 工具

 Clients 客户端

 Prompts 提示

 **Resources 资源**

Question 3: Correct answer
问题 3：正确答案

Your MCP server and client need to communicate. What's the most common way they connect during development?
您的 MCP 服务器和客户端需要通信。在开发过程中，它们最常用的连接方式是什么？

 Through a database 通过数据库

 Over the internet 通过互联网

 **Through standard input/output on the same machine
通过同一台机器的标准输入/输出**

 Using email 使用电子邮件

Question 4: Correct answer
问题 4：正确答案

You're building a chatbot that needs to access GitHub data. What is the main benefit of using MCP instead of writing your own GitHub integration?
你正在构建一个需要访问 GitHub 数据的聊天机器人。使用 MCP 而不是自己编写 GitHub 集成的主要优势是什么？

 MCP requires less memory
MCP 需要的内存更少

 **MCP handles the tool definitions and execution for you
MCP 为你处理工具定义和执行**

 MCP only works with GitHub
MCP 仅在 GitHub 上运行

 MCP makes your chatbot run faster
MCP 使您的聊天机器人运行更快

Question 5: Correct answer
问题 5：正确答案

You want to create a tool for your MCP server that reads document contents. Using the Python SDK, what's the easiest way to define this tool?
你想为你的 MCP 服务器创建一个读取文档内容的工具。使用 Python SDK，定义这个工具最容易的方法是什么？

 Write a complex JSON schema manually
手动编写复杂的 JSON 架构

 Send an HTTP request
发送 HTTP 请求

 **Use the @mcp.tool decorator on a function
在函数上使用 @mcp.tool 装饰器**

 Create a separate configuration file
创建一个单独的配置文件

Question 6: Correct answer
问题 6：正确答案

You want to provide users with a high-quality, pre-tested instruction for formatting documents. What MCP feature should you use?
你想为用户提供一个高质量、经过预测试的文档格式化说明。你应该使用 MCP 的哪个功能？

 Resources 资源

 Sessions 会话

 Tools 工具

 **Prompts 提示**
