# 快速入门：法律数据库 MCP 配置指南

> 本指南帮助你完成第一个法律数据库 MCP 的接入配置，预计耗时 15 分钟。

---

## 前置要求

- 已安装 **Claude Desktop**（[下载地址](https://claude.ai/download)）或 **Claude Code**
- 已安装 **Node.js 18+**（[下载地址](https://nodejs.org)）
- 有效的数据库账号（针对商业数据库）

---

## 第一步：找到 Claude 配置文件

**macOS / Linux：**
```bash
# Claude Desktop
~/Library/Application\ Support/Claude/claude_desktop_config.json

# 如文件不存在，请创建它
mkdir -p ~/Library/Application\ Support/Claude
touch ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

**Windows：**
```
%APPDATA%\Claude\claude_desktop_config.json
```

---

## 第二步：编辑配置文件

用文本编辑器打开配置文件，添加 MCP 服务器配置：

```json
{
  "mcpServers": {
    "cn-legal-db": {
      "command": "npx",
      "args": ["-y", "@vibe-lawyering/cn-legal-mcp"],
      "env": {
        "DB_SOURCE": "npc-flfl"
      }
    }
  }
}
```

如果文件已有内容，只需在 `mcpServers` 对象中添加新的键值对。

---

## 第三步：重启 Claude Desktop

保存配置文件后，完全退出并重新打开 Claude Desktop。

在 Claude 聊天界面，点击左下角的 🔌 图标，确认法律数据库 MCP 已连接（显示绿色状态）。

---

## 第四步：测试连接

在 Claude 中输入以下测试命令：

```
请调用法律数据库，查询《民法典》第577条的完整条文内容。
```

如果 Claude 能够返回准确的法条内容，说明配置成功。

---

## 常见问题

**Q: 提示"MCP server not found"**
- 检查 Node.js 是否已安装：终端运行 `node --version`
- 检查配置文件 JSON 格式是否正确（可使用 [JSON 格式验证器](https://jsonlint.com/)）

**Q: 数据库连接失败**
- 确认你的 API Key 或账号凭据是否正确填入 `env` 配置
- 检查网络连接，部分数据库需要特定网络环境

**Q: 返回数据不够准确**
- 尝试在提问中更明确地指定数据库来源
- 调整 `MAX_RESULTS` 参数获取更多结果

---

## 进阶配置

参考各数据库的专项配置指南：
- [中国法律数据库详细配置](../chinese-legal-databases.md)
- [国际数据库详细配置](../international-databases.md)
