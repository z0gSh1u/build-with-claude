# 01m - 结构化数据练习

对于如下用户输入及其目的：

```
Generate three different sample AWS CLI commands. Each should be very short.
```

默认地，Claude 可能会生成类似于以下的响应：

```
Here are three sample AWS CLI commands:
1. aws s3 ls
2. aws ec2 describe-instances
3. aws lambda list-functions
```

这种响应是可以接受的，但它并不是结构化的，不易于机器解析。为了更好地满足需求，请设法让 Claude 以按行形式列出命令，例如：

```
aws s3 ls
aws ec2 describe-instances
aws lambda list-functions
```

> 提示：使用 \`\`\`bash 和 \`\`\`。
