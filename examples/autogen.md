# 把乾元接入 AutoGen（微软）

> 实测 2026-09-12 · `autogen-ext` **0.7.5** · → **8 tools**
> 官方 CONTRIBUTING 明确欢迎 "Documentation, examples and test cases"。
> 若需正式对外，可提 PR 到 `microsoft/autogen`（需签 CLA）。
> **本轮只做自家文档示例，不对外提交。**

## 安装

```bash
pip install "autogen-ext[mcp]"
```

## 接入

```python
import asyncio
from autogen_ext.tools.mcp import McpWorkbench, StreamableHttpServerParams

async def main():
    params = StreamableHttpServerParams(url="https://qianyuan.ltd/mcp")
    workbench = McpWorkbench(params)
    async with workbench:
        tools = await workbench.list_tools()
        print(len(tools))   # 8

asyncio.run(main())
```

`McpWorkbench` 还提供：`call_tool` / `list_resources` / `list_prompts` /
`list_resource_templates`，可直接作为 Team 的工具源。

## 验证结果（本地实测）

```
✅ McpWorkbench.list_tools() → 8 tools
     - assess_skill / share_pitfall / verify_claim / query_experience
     - query_trust / recommend_skill / leaderboard / get_rubric
```
（工具 schema 亦正确：`assess_skill` 必填 `skill_text` + `qy_token`）

## 兜底写法

若 `McpWorkbench` 不适用，`autogen_ext.tools.mcp` 还导出 `mcp_server_tools`：

```python
from autogen_ext.tools.mcp import mcp_server_tools
tools = await mcp_server_tools(params)
```
