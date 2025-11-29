# 08f - Computer Use 原理

在 Claude 中，Computer Use 与常规的 Tool Use 基本相同。

## Tool Use 复习

在深入了解 Computer Use 之前，让我们快速回顾一下 Claude 中常规的 Tool Use 流程：

- 向 Claude 发送一条用户消息，并附带工具 Schema
- Claude 决定使用一个工具来回答问题
- Claude 发送一个包含工具名称和输入参数的 Tool Use 请求
- 服务器实际运行函数并返回结果
- 将结果通过 Tool Use Result 消息发送回 Claude

## Computer Use：相同流程，不同工具

Computer Use 遵循完全相同的模式，只不过它是一个可以与桌面环境交互的特殊的工具。

![img](08h-cua-how.assets/1.jpg)

- 向 Claude 提供 Computer Use 功能的工具 Schema
- Claude 决定使用 Computer Use 工具
- 开发者负责在 Computer 环境中执行所请求的操作
- 将结果（如截图）发送回 Claude

## Computer Use 工具 Schema

与代码执行的工具 Schema 类似，Computer Use 的 Schema 也是预置的，只需要发送一个“索引元信息”，Anthropic 会将其自动展开为完整的 Schema：

![img](08h-cua-how.assets/2.jpg)

例如：

```json
{
  "type": "computer_20250124",
  "name": "computer",
  "display_width_px": 1024,
  "display_height_px": 768,
  "display_number": 1
}
```

幕后，会被展开为完整的 Schema，告诉 Claude 可以执行以下操作：

- `key` - 按下键盘按键
- `mouse_move` - 移动光标
- `left_click` - 在特定坐标处点击
- `screenshot` - 捕获屏幕截图
- `scroll` - 滚动屏幕

## 开始使用

Computer Use 需要一个实际的计算环境，支持以编程方式执行键盘和鼠标操作。最常见的方法是使用带有 GUI 桌面环境的 Docker 容器。开发者无需从头开始构建计算环境，Anthropic 提供了一种参考实现，处理了所有细节。

![img](08h-cua-how.assets/3.jpg)

```sh
export ANTHROPIC_API_KEY="your_api_key" # 配置你的 API Key
docker run \
  -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \
  -v $HOME/.anthropic:/home/computeruse/.anthropic \
  -p 5900:5900 -p 8501:8501 -p 6080:6080 -p 8080:8080 \
  -it ghcr.io/anthropics/anthropic-quickstarts:computer-use-demo-latest
```

将容器运行起来后，将可以看到一个聊天界面，用于测试 Claude 的 Computer Use 能力。完整的设置指南可在 [github.com/anthropics/anthropic-quickstarts](https://github.com/anthropics/anthropic-quickstarts) 找到。

![img](08h-cua-how.assets/4.jpg)

Computer Use 将常规的 Tool Use 系统应用于桌面自动化。Claude 并不直接控制计算机，而是在受控的环境中请求工具调用，具体的处理由你的代码来满足，这提供了更大的安全性。
