# 🛠️ MCP Servers

本目录收录面向法律数据库的 **Model Context Protocol (MCP) Server** 实现，可直接接入 Claude Desktop、Cursor 等支持 MCP 协议的 AI 工具。

> 与 [`mcp-resources/`](../mcp-resources/README.md)（数据库索引与接入指南）不同，本目录提供**可运行的 MCP Server 代码或配置文件**。

---

## 📁 目录结构

```
mcp-servers/
├── chinese-legal/          # 中国法律数据库 MCP Server
│   ├── judgment-docs/      # 中国裁判文书网（✅ 可用）
│   └── national-laws/      # 国家法律法规数据库（✅ 可用）
├── international/          # 国际法律数据库 MCP Server
│   └── eurlex/             # 欧盟 EUR-Lex（✅ 可用）
└── templates/              # 自定义 MCP Server 开发模板
```

---

## 🚦 当前状态

| Server | 数据库 | 状态 | 语言 |
|--------|--------|------|------|
| [judgment-docs](chinese-legal/judgment-docs/) | 中国裁判文书网 | ✅ 可用 | Python |
| [national-laws](chinese-legal/national-laws/) | 国家法律法规数据库 | ✅ 可用 | Python |
| [eurlex](international/eurlex/) | EUR-Lex 欧盟法律 | ✅ 可用 | Python |
| pkulaw | 北大法宝 | 🟡 开发中 | — |
| wkinfo | 威科先行 | 🟡 开发中 | — |
| lexisnexis | LexisNexis | 🔴 计划中 | — |

---

## ⚡ 快速接入（以裁判文书网为例）

**1. 安装依赖**

```bash
cd mcp-servers/chinese-legal/judgment-docs
pip install -r requirements.txt
```

**2. 配置 Claude Desktop**

将以下内容添加到 `~/Library/Application Support/Claude/claude_desktop_config.json`（macOS）：

```json
{
  "mcpServers": {
    "judgment-docs": {
      "command": "python",
      "args": ["/path/to/mcp-servers/chinese-legal/judgment-docs/server.py"]
    }
  }
}
```

**3. 重启 Claude Desktop，即可在对话中调用裁判文书搜索工具。**

---

## 🔧 自定义开发

参考 [`templates/`](templates/) 目录，按照模板快速构建新的法律数据库 MCP Server。

每个 Server 遵循统一结构：

```
<server-name>/
├── server.py          # MCP Server 入口
├── tools.py           # 工具函数定义
├── config.py          # 配置（API Key、端点等）
├── requirements.txt   # Python 依赖
└── README.md          # 使用说明
```

---

## 🤝 贡献新 Server

欢迎为更多法律数据库贡献 MCP Server！请参考 [CONTRIBUTING.md](../CONTRIBUTING.md)，并使用 `templates/` 中的基础模板。
