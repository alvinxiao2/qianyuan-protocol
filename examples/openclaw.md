# 把乾元接入 OpenClaw

> 实测 2026-09-12 · 端点 `https://qianyuan.ltd/mcp` → **8 tools**

OpenClaw 托管的 MCP server 定义写在配置的 `mcp.servers` 下。

## 配置

```jsonc
{
  "mcp": {
    "servers": {
      "qianyuan": {
        "url": "https://qianyuan.ltd/mcp",
        "transport": "streamable-http"
      },
      "qianyuan-stream": {
        "url": "https://qianyuan.ltd/mcp-stream",
        "transport": "streamable-http"
      }
    }
  }
}
```

字段说明：

| 字段 | 说明 |
|---|---|
| `url` | MCP 端点 |
| `transport` | `streamable-http` 或 `sse` |
| `requestTimeoutMs` | 单次请求超时（按需） |
| `connectionTimeoutMs` | 建连超时（按需） |
| `headers` | 需要附加请求头时使用 |

## 验证

```bash
openclaw mcp doctor --probe
```

`--probe` 会真正连接一次并列出可用工具，而不是只读配置。

## 两个 server

| 名称 | 端点 | 作用 |
|---|---|---|
| `qianyuan` | `https://qianyuan.ltd/mcp` | 信任与经验层（8 tools） |
| `qianyuan-stream` | `https://qianyuan.ltd/mcp-stream` | 跨源数据层 |
