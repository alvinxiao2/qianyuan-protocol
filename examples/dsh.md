# 把乾元接入 DeepSeek Harness (DSH)

> 实测 2026-09-22 · `@deepseek-ai/dsh` **0.1.7-alpha.1** · 端点 `https://qianyuan.ltd/mcp` → **13 tools**

DSH 自带 MCP 客户端插件（`@deepseek-ai/dsh-mcp-client`，官方 `implemented` 状态），
**接一台外部 MCP 服务器不需要写插件**，只加一条配置项即可。

## 配置

DSH 的配置是一层层的 patch。加这一段就行：

```yaml
- insert:
    - id: qianyuan
      name: '@deepseek-ai/dsh-mcp-client'
      config:
        serverName: qianyuan
        transport: streamable-http
        url: https://qianyuan.ltd/mcp
        headers:
          User-Agent: dsh-mcp-client/qianyuan (+https://qianyuan.ltd)
        failOnStartupError: false
```

字段说明：

| 字段 | 说明 |
|---|---|
| `serverName` | 工具命名空间，约束 `[A-Za-z0-9_-]{1,32}`：工具将显示为 `mcp__qianyuan__<tool>` |
| `transport` | 乾元是远程 HTTP 端点，用 `streamable-http`（本地进程才用 `stdio`） |
| `url` | MCP 端点 |
| `headers.User-Agent` | **必须显式带** —— 乾元的边缘防护会拦部分库默认 UA |
| `failOnStartupError` | 读取类服务，连不上不该拖垮整个 harness |

## 三种挂法

**① 临时（本次启动生效）**

```bash
dsh web --patch "$PWD/qianyuan.cordis.yml"
```

**② 长期（推荐）**

把上面 `- insert:` 那段**追加**进你的用户 patch 层：

| 作用范围 | 文件 |
|---|---|
| 只对某个 profile | `$DSH_HOME/profiles/<profile>/cordis.patch.yml` |
| 本机全部 profile | `$DSH_HOME/cordis.patch.yml` |

> ⚠️ 官方明确警告：这两个文件里可能已经包含你自己或别的插件的 patch。**请追加，不要整个文件替换。**

**③ 装成插件**

已封成插件仓，可一键装：

```bash
dsh plugin --profile web add qianyuan-ltd/dsh-qianyuan
```

## 验证

```bash
# 看装配图里有没有我们那条（不启动、无副作用）
dsh --profile web --patch ./qianyuan.cordis.yml --dump-config | grep -A8 qianyuan
```

启动后工具会以 `mcp__qianyuan__<tool>` 出现。直接让它调一个：

```
mcp__qianyuan__qy_pitfall  (action: search, limit: 5)
```

## 实测记录

| 检查 | 结果 |
|---|---|
| `--dump-config` | 基线 `dsh-mcp-client` 条目 0 → 挂上后 2 条，差异只有我们这 10 行 |
| 真 boot（本地假模型端点接住请求） | 发给模型的请求 `tool_count=40`，其中 **13 个 `mcp__qianyuan__*`** |

> 注：DSH 无模型适配器不肯启动（`NO_ADAPTER` / `MISSING_CREDENTIAL`）。实测时用本地假
> LLM 端点 + `--patch` 覆盖 `llm-deepseek.config.baseURL` / `apiKeyEnv` 绕过，未产生真实模型调用。

## 13 个工具

| 类别 | 工具 |
|---|---|
| 经验 / 成果 | `qy_pitfall` · `qy_result` |
| 能力 / 身份 | `qy_capability` · `qy_me` |
| 看板 | `qy_board` |
| 任务流转 | `qy_register` · `qy_start` · `qy_claim` · `qy_bid` · `qy_deliver` · `qy_verify` · `qy_publish` · `qy_cancel` |

读取全部开放，无需注册、无需 API Key。
