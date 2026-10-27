# 2026-10 安全事件研究语料

基于陈成（[sorrycc](https://github.com/sorrycc)）于 2026-10-22 发布的梳理文章，整理成可检索、可增量补充的中文研究仓库。

**来源：** [2026 年 10 月中下旬安全事件梳理](https://research.sorrycc.dev/@sorrycc/2026-10-security-incidents)  
**覆盖窗口：** 2026-10-08 至 2026-10-22  
**整理原则：** 不编造事实；可信度与处置建议以原文为准；后续日报见 `digests/`。

## 一句话结论

本期真正造成损失的多半是**凭据泄露**（Zeabur / LiteLLM / OpenCode / TiDB Cloud），不是更抢眼的 RCE；唯一确认在野利用的软件漏洞是 **PHP 核心 RCE**（CVE-2026-109618 / 109619）。

## 十条事件总览

| # | 事件 | 类型 | 严重性 | 在野 / 损失 | 状态 | 文档 |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | PHP 核心 RCE（CVE-2026-109618 / 109619） | 软件漏洞 | CVSS v4 9.2 | 已有在野利用 | 已修复 | [01](docs/01-php-rce.md) |
| 2 | LiteLLM 1.107.0 投毒 | 供应链 | 严重 | 恶意版本已上线约 3h+ | 已下架 | [02](docs/02-litellm-supply-chain.md) |
| 3 | Zeabur 平台入侵 | 云平台入侵 | 严重 | 已确认大规模数据读取 | 调查中 / 停运 | [03](docs/03-zeabur-breach.md) |
| 4 | eMudhra 中间 CA 私钥可推算 | PKI 信任失效 | 高 | 未见在野滥用；公开工具已支持 | 各层封锁进度不一 | [04](docs/04-emudhra-ca.md) |
| 5 | Node.js 10 月更新 / Bun Brotli | 软件漏洞 | 4 High（含 RCE） | 未见报告 | Node 10-21；Bun 1.4.4（10-22） | [05](docs/05-nodejs-bun-brotli.md) |
| 6 | GitLab CVE-2026-110264 | 软件漏洞 | CVSS 10.0 | 未见报告 | 已修复 | [06](docs/06-gitlab-cve.md) |
| 7 | OpenCode Console API Key 可能暴露 | 凭据泄露 | 中 | 用户自报盗刷，未核实 | 已通知用户 | [07](docs/07-opencode-console.md) |
| 8 | Linux 内核 PwnEscape（CVE-2026-99999） | 虚拟化逃逸 | CVSS 3.1 9.3 | 未见报告 | 上游已修；发行版进度不一 | [08](docs/08-pwnescape.md) |
| 9 | IDCF Cloud 勒索 | 勒索 | 严重 | 495 家机构受影响 | 恢复受限 | [09](docs/09-idcf-ransomware.md) |
| 10 | PingCAP / TiDB Cloud 数据泄露 | 凭据 / 数据泄露 | 高 | 已确认数据被复制；攻击方规模未证实 | 凭据已作废 | [10](docs/10-tidb-cloud.md) |

附：漏洞发现侧 → [Anthropic OSS Scanner](docs/11-anthropic-oss-scanner.md)  
处置优先级与趋势 → [priority-and-trends.md](docs/priority-and-trends.md)

## 文档目录

| 路径 | 说明 |
| --- | --- |
| [GAP-ANALYSIS.md](GAP-ANALYSIS.md) | 源文覆盖了什么、本语料怎么拆 |
| [docs/00-source-article.md](docs/00-source-article.md) | 源文摘要与链接 |
| [docs/01-php-rce.md](docs/01-php-rce.md) … [10-tidb-cloud.md](docs/10-tidb-cloud.md) | 十条事件专题 |
| [docs/11-anthropic-oss-scanner.md](docs/11-anthropic-oss-scanner.md) | OSS Scanner |
| [docs/priority-and-trends.md](docs/priority-and-trends.md) | P0/P1 表与趋势判断 |
| [digests/](digests/) | 每日 GitHub Trending 安全相关摘要（`YYYY-MM-DD.md`） |

## digests/ 说明

`digests/` 存放**每日**从 GitHub Trending 抓取的安全相关事故、漏洞、工具信息摘要。  
格式约定见 [digests/README.md](digests/README.md)；空模板见 [digests/_TEMPLATE.md](digests/_TEMPLATE.md)。

**最新日报：** [2026-10-28](digests/2026-10-28.md)

## 可信度约定（沿用原文）

- **已确认**：一手公告或多家独立机构交叉证实  
- **二手**：仅媒体或第三方转述  
- **未证实**：仅当事方或攻击方单方说法  
