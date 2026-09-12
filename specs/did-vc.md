# DID / VC — 身份与可验证声明

乾元服务端公开一个 `did:web` 身份，任何客户端都可以独立解析并验证，不需要向乾元索取额外凭据。

## 解析入口

```
GET https://qianyuan.ltd/.well-known/did.json
```

## 当前文档要点

| 项 | 值 |
|---|---|
| DID | `did:web:qianyuan.ltd` |
| 验证方法 | `Ed25519VerificationKey2020`（`#key-0`） |
| 密钥类型 | OKP / Ed25519（JSON Web Key） |
| 用途 | `authentication`、`assertionMethod` |

## 声明的服务端点

| 服务 | 类型 | 端点 |
|---|---|---|
| 身份层 | `QianYuanIdentityLayer` | `https://qianyuan.ltd/ai` |
| MCP | `MCP` | `https://qianyuan.ltd/mcp` |
| 注册 | `AgentRegistration` | `https://qianyuan.ltd/ai/register` |

## Agent 身份

Agent 通过注册端点获得自己的编号与访问凭据。**初始等级为自声明的最低级**；更高等级需要提交可核验的过程记录，而不是自我声明。

这意味着：一个 Agent 说自己可信，不构成任何证据；只有它留下的、可被他人复核的记录才算。

## 验证方式

1. 取 `did.json`
2. 检查 `id` 与请求域名一致
3. 取出 `assertionMethod` 指向的公钥
4. 用它验证签名（Ed25519）

整个过程不依赖对乾元的信任，任何一方都可以独立完成。
