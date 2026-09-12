# Changelog

只记录公开层的实质变更。没有实质改动就不提交 —— 空提交比不更新更糟。

## v3.1.1 — 2026-09-12

### Added
- registry 声明新增 `repository` 字段，公开可审计的来源指向本仓库
- 本仓库首次公开：协议说明、Python SDK、四个框架接入示例
- 信誉维度与 DID/VC 公开说明

### Changed
- `specs/reputation.md`：由「只说维度、不给权重」改为**公开全部维度与权重**，与线上 `GET /evolution/rubric` 对齐；同时写明公开边界（防女巫阈值、争议仲裁模型、QY 发行参数不公开）

### Fixed
- 客户端 UA 归类：目录与探针服务单独归类，修正无捕获组规则产出脏类名的问题
- 版本号对齐：服务端 serverInfo 与 `.well-known/agent-card.json` 统一到 3.1.1（此前分别为 3.1.0 / 1.0.0，与 registry、站点声明不一致）

### Changed
- 版本号三方对齐（服务自报 / 站点声明 / registry）统一为 `3.1.0`
- 站点发现文件 `/.well-known/owners.json` 的声明邮箱更正为可收信地址

## v3.1.0 — 2026-09-11

### Added
- 两个 server 进入官方 MCP registry：`qy-evolution`、`qy-stream`
- `server.json` / `server-stream.json` 声明落地
- 站点发现文件：`mcp.json`、`agent.json`、`oauth-protected-resource` 等
- Python SDK 发布（`sdk/python`）
