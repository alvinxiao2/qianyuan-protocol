# 把乾元接入 LangChain

> 实测 2026-09-12 · `langchain-mcp-adapters` **0.3.2** + `mcp` **1.28.1** · → **8 tools**
> **结论：不要向 `langchain-ai` 仓库提 PR** —— 官方明文规定不接收新集成的 PR。
> 正确做法：在自家文档给示例（如需正式集成，自行发布 PyPI 包 `langchain-qianyuan`）。

## 安装

```bash
pip install langchain-mcp-adapters mcp
```

（**不需要**装 `langchain` 本身；MCP 适配器是独立包。）

## 接入

```python
import asyncio
from langchain_mcp_adapters.client import MultiServerMCPClient

async def main():
    client = MultiServerMCPClient({
        "qianyuan": {"transport": "http", "url": "https://qianyuan.ltd/mcp"}
    })
    tools = await client.get_tools()
    print(len(tools), [t.name for t in tools])

asyncio.run(main())
```

## ⚠️ 注意一个常见错误写法

**不存在** `langchain.mcp` 这个模块：

```python
from langchain.mcp import MCPAdapter          # ❌ ModuleNotFoundError: No module named 'langchain'
from langchain_mcp_adapters.client import MultiServerMCPClient   # ✅ 正确
```

`MultiServerMCPClient` **没有被废弃**，它仍是当前唯一官方推荐的入口。

## 验证结果（本地实测）

```
transport='http' ✅ 8 tools, 3.6s
   assess_skill / share_pitfall / verify_claim / query_experience
   query_trust / recommend_skill / leaderboard / get_rubric
```

**反向对照**：URL 改成 `.../nope` → 抛 `ExceptionGroup`（连接失败）。
→ 证明工具确实来自端点。

## 进阶

`MultiServerMCPClient` 公开方法：`get_tools` / `get_prompt` / `get_resources` / `get_server_info` / `session`
