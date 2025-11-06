# 08f - Computer Use 简介

《计算机使用》是一项强大的功能，它允许 Claude 直接与桌面环境交互，本质上赋予 AI 像人类一样控制计算机的能力。这项能力为自动化、测试和工作流程辅助开辟了全新的可能性。

## 电脑使用能做什么

与其仅仅生成代码或提供建议，Claude 实际上可以浏览网站、点击按钮、填写表单，并实时与应用程序进行交互。这使得它在执行诸如任务时非常实用：

- Web 应用程序的自动化 QA 测试
- 数据录入和表单填写
- 网站导航和信息收集
- UI 测试和验证
- 重复的桌面任务

## 实际案例：自动化质量保证测试

![img](./08g-cua.assets/1.jpg)

```
Your goal is to conduct QA testing on a React component hosted at https://test-mentioner.vercel.app/

Testing process:
1. Open a new browser tab
2. Navigate to https://test-mentioner.vercel.app/
3. Execute the test cases below one by one
4. After completing all tests, write a concise report

Test cases:
1. Typing 'Did you read @' should display autocomplete options
2. Typing 'Did you read @' then pressing enter should add '@document.pdf'
3. After adding '@document.pdf', pressing backspace should show autocomplete options directly below the text, not elsewhere on the page
```

## 工作原理

Computer Use 在受控环境中运行——通常是一个带有浏览器的 Docker 容器，该浏览器与您的系统完全隔离。您通过聊天界面与 Claude 互动，给它下达指令，然后观察它如何导航并与应用程序交互。

Claude 按照您的指示一步步操作，截取屏幕截图，点击元素，输入文本，并观察结果。在 QA 测试示例中，Claude 会：

- 导航到指定的网址
- 输入测试输入并观察自动完成行为
- 测试回车键功能
- 检查退格键的正确定位行为
- 生成详细报告，显示哪些测试通过，哪些失败

## 结果

在运行完所有测试用例后，Claude 提供了一份全面的报告。在我们的例子中，它可能发现前两个测试通过（自动补全显示正确，回车键正常工作），但第三个测试失败，因为按下退格键时自动补全下拉菜单出现在错误的位置。

这种自动化测试可以节省开发者大量时间，尤其是在重复的 QA 任务或需要快速测试多个场景时。你不必手动点击每个可能的交互，你可以描述你想测试的内容，让 Claude 来处理执行。

## 安全性与隔离

计算机使用在沙盒环境中运行以确保安全。浏览器和桌面环境在 Docker 容器内运行，完全与您的系统隔离。这意味着 Claude 可以与网络应用程序交互并测试接口，而不会对您的个人文件或系统安全造成任何风险。

这种隔离至关重要，因为它允许您授予 Claude 广泛的权限与应用程序交互，同时保持对其可以和不可以访问内容的完全控制。
