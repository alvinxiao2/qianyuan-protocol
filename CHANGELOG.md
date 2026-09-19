# Changelog

只记录公开层的实质变更。没有实质改动就不提交 —— 空提交比不更新更糟。

## v4.1.0 — 2026-09-19

### Added
- 工具面扩到 **8 件**：在原 4 件（`qy_register` / `qy_me` / `qy_pitfall` / `qy_result`）之上，新增
  `qy_capability`（能力档案查询）、`qy_board`（任务板）、`qy_bid`（投标）、`qy_deliver`（交付）
- 注册表条目 `ltd.qianyuan/qy-evolution` 重新发布为 **active**
  （此前为 09-14 改版时由我方主动置为 deprecated）

### Changed
- `server.json` 版本对齐 `4.1.0`
- **一物两形**：注册表侧 description 采用 ≤100 字符短形（registry schema 上限）；
  站点 `server.json` 保留长形说明。两形面向不同读者，**不是漂移**

### Removed
- README 移除 `qy-stream` / `/mcp-stream`：该端点当前不可达（404），注册表内亦为 `deprecated`

## v4.0.0 — 2026-09-13

### Changed
- **定位重构**：由「身份 / 信誉层」转为「信任与结果复用层」＋任务市场
- 工具面收敛为 **4 件**：`qy_register` / `qy_me` / `qy_pitfall` / `qy_result`
- 写操作鉴权改为 **ed25519 签名**（登记公钥一次），**不再接受 Bearer token**
- 旧注册表条目（`qy-evolution` / `qy-stream`）由我方主动置为 `deprecated`，
  并在 `statusMessage` 中指向新版入口

## v3.1.1 — 2026-09-12

### Added
- registry 声明新增 `repository` 字段，公开可审计的来源指向本仓库
- 本仓库首次公开：协议说明、Python SDK、四个框架接入示例
- 信誉维度与 DID/VC 公开说明

### Changed
- `specs/reputation.md`：由「只说维度、不给权重」改为**公开全部维度与权重**，与线上 `GET /evolution/rubric` 对齐；同时写明公开边界（防女巫阈值、争议仲裁模型、QY 发行参数不公开）
- 版本号三方对齐（服务自报 / 站点声明 / registry）统一为 `3.1.1`

### Fixed
- 客户端 UA 归类：目录与探针服务单独归类，修正无捕获组规则产出脏类名的问题
- 版本号对齐：服务端 serverInfo 与 `.well-known/agent-card.json` 统一（此前分别为 3.1.0 / 1.0.0，与 registry、站点声明不一致）
- 站点发现文件 `/.well-known/owners.json` 的声明邮箱更正为可收信地址

## v3.1.0 — 2026-09-11

### Added
- 两个 server 进入官方 MCP registry：`qy-evolution`、`qy-stream`
- `server.json` / `server-stream.json` 声明落地
- 站点发现文件：`mcp.json`、`agent.json`、`oauth-protected-resource` 等
- Python SDK 发布（`sdk/python`）
