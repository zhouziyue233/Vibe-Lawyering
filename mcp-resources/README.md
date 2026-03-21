# 🗄️ MCP 资源库 | Legal Database MCP Resources

> 面向法律人的 Model Context Protocol (MCP) 工具集合，帮助你将主流法律数据库无缝接入 AI 助手。
> A collection of Model Context Protocol (MCP) tools for legal professionals, enabling seamless integration of major legal databases into AI assistants.

---

## 什么是 MCP？| What is MCP?

**Model Context Protocol (MCP)** 是一种开放协议，允许 AI 助手（如 Claude）安全地访问外部数据源、工具和服务。通过 MCP，你可以让 Claude 直接查询法律数据库，实现"检索 + 分析"的一体化工作流。

**MCP (Model Context Protocol)** is an open protocol that allows AI assistants (like Claude) to securely access external data sources, tools, and services. With MCP, you can let Claude directly query legal databases, enabling an integrated "search + analyze" workflow.

---

## 快速入门 | Quick Start

1. 安装 Claude Desktop 或 Claude Code
2. 按照对应数据库的配置指南进行设置
3. 在 Claude 中直接提问，AI 将自动调用数据库

详见 [setup-guides/getting-started.md](setup-guides/getting-started.md)

---

## 中国法律数据库 | Chinese Legal Databases

### 官方/免费数据库

| 数据库 | MCP 状态 | 覆盖范围 | 访问方式 | 说明 |
|--------|----------|----------|----------|------|
| **国家法律法规数据库** | ✅ 可用 | 法律、行政法规、司法解释 | 免费 | 全国人民代表大会官方数据库 |
| **中国裁判文书网** | ✅ 可用 | 全国法院裁判文书 | 免费 | 数据最新至2023年底 |
| **中国法院网** | 🟡 开发中 | 案例、法规、公告 | 免费 | — |
| **最高人民检察院案例库** | 🟡 开发中 | 指导性案例、典型案例 | 免费 | — |

### 商业数据库

| 数据库 | MCP 状态 | 覆盖范围 | 访问方式 | 说明 |
|--------|----------|----------|----------|------|
| **北大法宝** | 🟡 开发中 | 法规、案例、期刊、合同模板 | [付费] | 中国最全面的法律数据库 |
| **威科先行** | 🟡 开发中 | 法规、案例、实务分析 | [付费] | Wolters Kluwer 旗下 |
| **法信** | 🟡 开发中 | 最高院指导案例、审判参考 | [付费] | 官方背景 |
| **无讼** | 🔴 计划中 | 裁判文书、律所、律师信息 | [付费] | 面向律师的实务平台 |
| **聚法案例** | 🔴 计划中 | 裁判文书、法律法规 | [付费/部分免费] | — |

---

## 国际法律数据库 | International Legal Databases

| 数据库 | MCP 状态 | 覆盖范围 | 访问方式 | 地区 |
|--------|----------|----------|----------|------|
| **EUR-Lex** | ✅ 可用 | 欧盟法律、指令、条例 | 免费 | 欧盟 |
| **UN Treaty Collection** | ✅ 可用 | 国际条约、协定 | 免费 | 国际 |
| **HeinOnline** | 🟡 开发中 | 法律期刊、历史法规 | [付费] | 国际 |
| **Westlaw** | 🔴 计划中 | 美国法规、案例、二级文献 | [付费] | 美国/国际 |
| **LexisNexis** | 🔴 计划中 | 国际法律信息、新闻 | [付费] | 国际 |
| **vLex** | 🔴 计划中 | 多国法律数据库 | [付费] | 多国 |

---

## MCP 配置示例 | MCP Configuration Examples

### 国家法律法规数据库 MCP 配置

```json
{
  "mcpServers": {
    "cn-legal-db": {
      "command": "npx",
      "args": ["-y", "@vibe-lawyering/cn-legal-mcp"],
      "env": {
        "DB_SOURCE": "npc-flfl",
        "LANGUAGE": "zh-CN"
      }
    }
  }
}
```

### 裁判文书网 MCP 配置

```json
{
  "mcpServers": {
    "wenshu-mcp": {
      "command": "npx",
      "args": ["-y", "@vibe-lawyering/wenshu-mcp"],
      "env": {
        "MAX_RESULTS": "20",
        "DATE_RANGE": "last_3_years"
      }
    }
  }
}
```

> ⚠️ **注意：** 以上配置为示例格式，实际 MCP 包需在对应数据库授权范围内使用，请遵守各数据库的服务条款。

---

## 使用场景示例 | Usage Examples

### 场景一：合同纠纷案例检索

接入 MCP 后，你可以在 Claude 中这样提问：

```
请检索近3年内，上海法院关于"合同违约金过高调整"的裁判案例，
总结法院的主流裁判尺度，并给出支持调整和不支持调整的主要理由。
```

### 场景二：法规溯源

```
《个人信息保护法》第13条关于处理个人信息的合法性基础，
与之前的《网络安全法》相关规定有何传承和创新？
请梳理立法演进脉络。
```

### 场景三：跨库综合研究

```
请结合国内裁判文书和欧盟 EUR-Lex 数据库，
对比中欧数据泄露通知义务的法律规定和司法实践。
```

---

## 配置指南 | Setup Guides

- [快速入门：MCP 基础配置](setup-guides/getting-started.md)
- [Claude Desktop MCP 配置教程](setup-guides/claude-desktop.md)
- [中国法律数据库接入指南](chinese-legal-databases.md)
- [国际法律数据库接入指南](international-databases.md)

---

## 贡献 MCP 资源 | Contribute MCP Resources

如果你开发了一个法律数据库 MCP 工具，或发现了有效的配置方式，欢迎提交！

- 提交新的 MCP 工具：[提交 Issue](https://github.com/zhouziyue233/Vibe-Lawyering/issues/new?template=resource_submission.md)
- 贡献配置代码：参考 [CONTRIBUTING.md](../CONTRIBUTING.md)

---

## MCP 状态说明

| 图标 | 含义 |
|------|------|
| ✅ 可用 | MCP 工具已发布，可直接使用 |
| 🟡 开发中 | 正在开发，欢迎参与贡献 |
| 🔴 计划中 | 纳入路线图，尚未启动 |
