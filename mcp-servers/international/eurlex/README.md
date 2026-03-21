# EUR-Lex MCP Server

对接 [EUR-Lex](https://eur-lex.europa.eu/)（欧盟官方法律数据库）的 MCP Server，通过官方开放 API 提供欧盟法律法规检索能力。

## 功能

- 🔍 **全文检索**：搜索欧盟条约、法规、指令、决定等
- 📄 **文件获取**：按 CELEX 编号获取法律文件全文
- 🌐 **多语言**：支持欧盟所有官方语言（含中文检索）
- 🗂️ **类型筛选**：按文件类型、发布机构、年份筛选
- 🔗 **相关文件**：查找修正案、实施细则、相关判例

## 数据来源

[EUR-Lex Open Data](https://eur-lex.europa.eu/content/tools/webservices/WebServicesHandbook.pdf)（欧盟官方开放 API，**免费使用，无需 API Key**）

## 快速开始

```bash
pip install -r requirements.txt
cp config.example.py config.py
```

**Claude Desktop 配置：**

```json
{
  "mcpServers": {
    "eurlex": {
      "command": "python",
      "args": ["/绝对路径/mcp-servers/international/eurlex/server.py"]
    }
  }
}
```

## 可用工具（MCP Tools）

| 工具名 | 描述 | 核心参数 |
|--------|------|----------|
| `search_eu_law` | 搜索欧盟法律文件 | `query`, `doc_type`, `year`, `language` |
| `get_document` | 按 CELEX 编号获取文件 | `celex_id`, `language` |
| `get_gdpr_article` | 快速获取 GDPR 指定条款 | `article_number` |
| `search_by_topic` | 按 EuroVoc 主题词检索 | `topic`, `subtopic` |

## 使用示例

> "获取 GDPR 第 17 条（被遗忘权）的完整条文"

> "搜索欧盟 2023 年以后发布的关于人工智能的法规和指令"

> "查找《欧盟人工智能法案》（AI Act）的正式文本"

## CELEX 编号说明

- `32016R0679` → GDPR（2016 年第 679 号法规）
- `12012E/TXT` → 《欧盟运作条约》全文
