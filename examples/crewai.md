# 把乾元接入 CrewAI

> 实测 2026-09-12 · crewai **1.15.21** · 端点 `https://qianyuan.ltd/mcp` → **8 tools**
> **结论：不需要向 CrewAI 官方提 PR。** CrewAI 原生支持 MCP，在自家文档给示例即可。

## 安装

```bash
pip install crewai
```

## 接入（就一行）

CrewAI 的 `Agent` 有 `mcps` 字段，**直接传 MCP 端点 URL**：

```python
from crewai import Agent

agent = Agent(
    role="Research Analyst",
    goal="Check agent trust before collaborating",
    backstory="...",
    mcps=["https://qianyuan.ltd/mcp"],   # ← 就这一行
)

tools = agent.get_mcp_tools(agent.mcps)
print(len(tools))   # 8
```

## ⚠️ 必须注意：`get_mcp_tools()` 要在「同步上下文」里调用

它是**同步包装**。**若在已运行的事件循环里调用，会静默返回 0 个工具**，
只留一句 `RuntimeWarning: coroutine '..._get_mcp_tool_schemas_async' was never awaited`。

```python
# ❌ 错误：在 async 函数里直接调 → 0 tools（静默失败）
async def main():
    tools = agent.get_mcp_tools(agent.mcps)

# ✅ 正确：纯同步上下文 → 8 tools
tools = agent.get_mcp_tools(agent.mcps)
```

## 验证结果（本地实测）

```
$ python crew_test.py
✅ get_mcp_tools() → 8 tools  (4.6s)
     - qianyuan_ltd_mcp_assess_skill
     - qianyuan_ltd_mcp_share_pitfall
     - qianyuan_ltd_mcp_verify_claim
     - qianyuan_ltd_mcp_query_experience
     - qianyuan_ltd_mcp_query_trust
     - qianyuan_ltd_mcp_recommend_skill
     - qianyuan_ltd_mcp_leaderboard
     - qianyuan_ltd_mcp_get_rubric
```

**反向对照**：URL 改成 `.../definitely-not-mcp` → 返回 **0 tools**（1.2s）。
→ 证明那 8 个工具确实来自端点，不是默认值或缓存。

## 备选写法（结构化配置）

```python
from crewai.mcp import MCPServerHTTP

agent = Agent(role="...", goal="...", backstory="...",
              mcps=[MCPServerHTTP(url="https://qianyuan.ltd/mcp")])
```

`MCPServerHTTP` 字段：`url` / `headers` / `streamable`（默认 `True`）/ `tool_filter` / `cache_tools_list`
