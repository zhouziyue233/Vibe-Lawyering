# MCP Server 开发模板

用于快速构建新的法律数据库 MCP Server。复制本目录，按照说明替换占位符即可。

## 使用方法

```bash
cp -r templates/ my-new-server/
cd my-new-server/
pip install -r requirements.txt
cp config.example.py config.py
# 编辑 server.py，添加工具定义和实现
```

## 文件说明

| 文件 | 说明 |
|------|------|
| `server.py` | MCP Server 入口，包含工具注册骨架 |
| `config.example.py` | 配置模板，复制为 config.py 后填写 |
| `requirements.txt` | 基础依赖 |

## 开发要点

1. **定义工具**：在 `list_tools()` 中声明工具名称、描述和参数 Schema
2. **实现工具**：在 `call_tool()` 中路由到对应实现函数
3. **错误处理**：所有工具调用需有 try/except，返回友好错误信息
4. **中文支持**：JSON 序列化时使用 `ensure_ascii=False`

## 提交贡献

完成后欢迎提 PR！请确保：
- [ ] 有完整的 README.md（含使用示例）
- [ ] 有 `config.example.py`（不提交真实 API Key）
- [ ] 工具描述清晰，方便 AI 理解何时调用
