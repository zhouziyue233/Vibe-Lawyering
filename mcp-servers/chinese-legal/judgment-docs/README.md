# 中国裁判文书网 MCP Server

基于中国裁判文书网公开数据的 MCP Server，支持在 Claude 等 AI 工具中直接搜索和读取裁判文书。

## 功能

- 🔍 **全文检索**：按案件名称、当事人、关键词搜索裁判文书
- 📄 **文书读取**：获取裁判文书全文及结构化摘要
- 🏛️ **法院筛选**：按法院层级、地区筛选
- 📅 **时间筛选**：按裁判日期范围检索
- ⚖️ **案件类型**：支持民事、刑事、行政、执行等案件类型

## 数据来源

[中国裁判文书网](https://wenshu.court.gov.cn/)（最高人民法院官方平台，公开数据）

## 快速开始

### 安装依赖

```bash
pip install -r requirements.txt
```

### 配置

复制配置模板：

```bash
cp config.example.py config.py
```

### 接入 Claude Desktop

macOS 配置路径：`~/Library/Application Support/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "judgment-docs": {
      "command": "python",
      "args": ["/绝对路径/mcp-servers/chinese-legal/judgment-docs/server.py"]
    }
  }
}
```

## 可用工具（MCP Tools）

| 工具名 | 描述 | 参数 |
|--------|------|------|
| `search_judgments` | 搜索裁判文书 | `keyword`, `court`, `date_from`, `date_to`, `case_type` |
| `get_judgment` | 获取文书全文 | `doc_id` |
| `get_case_summary` | 获取案件摘要 | `doc_id` |
| `list_courts` | 获取法院列表 | `province`, `level` |

## 使用示例

> "帮我搜索2023年北京市关于股权转让纠纷的裁判文书，整理争议焦点"

> "查找最高人民法院近五年关于数据权利的典型案例"

## 注意事项

- 本 Server 仅使用裁判文书网公开数据，请遵守平台使用条款
- 建议设置合理的请求频率，避免对服务器造成压力
