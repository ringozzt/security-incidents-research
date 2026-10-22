# 5. Node.js 10 月安全更新与 Bun Brotli

**来源：** [sorrycc 2026-10 梳理 §5](https://research.sorrycc.dev/@sorrycc/2026-10-security-incidents)  
**可信度：** 已确认（Node.js 官方公告、oven-sh/bun#46352、#46392）

## 概况

Node.js 于 **10-21** 发布 **22.23.4 / 24.22.1 / 26.12.1**，修 10 个 CVE（4 高、3 中、3 低）。22.x 受 7 个、24.x 受 8 个、26.x 受全部 10 个。**20.x**（2026-04-30 EOL）与 **25.x**（2026-06-01 EOL）不再提供补丁。同步更新了 c-ares、CA（含 eMudhra 封锁）、OpenSSL、SQLite、undici 等。

## CVE 表（原文）

| 等级 | CVE | 内容 | 影响分支 |
| --- | --- | --- | --- |
| High | CVE-2026-111497 | Brotli 解码越界写入，可致 RCE | 22 / 24 / 26 |
| High | CVE-2026-105482 | `dns.resolve` 遇 NUL 截断域名 | 22 / 24 / 26 |
| High | CVE-2026-105483 | 全角数字 IP 绕过证书主机名校验 | 24 / 26 |
| High | CVE-2026-106069 | 被宽松解析为 IP 的主机名绕过校验 | 22 / 24 / 26 |
| Medium | CVE-2026-105481 | keep-alive 处理了请求发出前写入的响应 | 22 / 24 / 26 |
| Medium | CVE-2026-106072 | 非规范 IPv4 使 DNS SAN 可认证 IP 端点 | 22 / 24 / 26 |
| Medium | CVE-2026-106074 | 启动快照 / SEA 保存随机数缓存，每次启动相同 | 22 / 24 / 26 |
| Low | CVE-2026-106070 | ZipFile 校验失败后返回攻击者可控前缀 | 26 |
| Low | CVE-2026-106071 | 权限模型通配符授权过宽 | 26 |
| Low | CVE-2026-106073 | 权限模型可经 `fs.realpathSync` 绕过 | 22 / 24 / 26 |

## Brotli RCE 暴露面

- **Bun：** 全部版本受影响；`fetch()`、`node:zlib`、`DecompressionStream("brotli")` 默认路径会解码（≤1.4.2 内置 brotli 1.1.0，1.4.3 内置 1.2.0，均未打补丁）。上游修复 `google/brotli@0ed4268a`；Bun **v1.4.4**（10-22 10:11 UTC）已修。
- Node `fetch`（undici）对 `Content-Encoding: br` 默认自动解压——原文标为**推断**，官方公告未说明触发条件。
- 风险最高：爬虫、AI Agent、MCP server、Webhook 回调、链接预览等「请求攻击者可控 URL」的程序。

## 处置（原文）

1. 核对服务 / CI / 容器 / Electron / SEA / `npx` MCP 的实际运行版本。  
2. 浮动 tag（如 `node:22`）需重新 pull 构建。  
3. 升级后重启 pm2 / systemd；只换二进制不影响已运行进程。  
4. Electron 内置独立 Node，需等应用发版。  
5. SEA：用新版 Node 重新打包。  
6. 优先外部抓取类服务；Bun → `bun upgrade` 至 1.4.4；`oven/bun` 镜像重建。  
7. 自链 brotli 的 C/C++/Rust/Go 项目需确认是否带上 `0ed4268a`（原文：据上游推断，本期未覆盖其他语言生态）。
