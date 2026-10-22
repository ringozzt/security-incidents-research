# 每日安全摘要（digests）

本目录存放**每日**从 GitHub Trending（及同类公开来源）抓取的安全相关事故、漏洞、工具信息摘要。

## 文件命名

```
YYYY-MM-DD.md
```

示例：`2026-10-23.md`。时区按 **Asia/Shanghai（UTC+8）** 的日历日归档。

## 每篇结构

请复制 [_TEMPLATE.md](_TEMPLATE.md) 填写，至少包含：

1. **元信息**：日期、抓取窗口、来源列表  
2. **今日要点**：3–8 条 bullet  
3. **表格**：仓库 / CVE / 事件名 | 类型 | 简述 | 链接 | stars（若有）  
4. **与本仓库主线交叉**：若与 PHP / LiteLLM / Zeabur / eMudhra / Node·Bun / GitLab / OpenCode / PwnEscape / IDCF / TiDB / OSS Scanner 相关，注明并链到 `docs/`  
5. **未核实项**：单方说法单独列出，勿写入「已确认」

## 范围提示

纳入：CVE、RCE、供应链投毒、凭据泄露、云平台入侵、CA/PKI、勒索、虚拟化逃逸、安全扫描/加固工具。  
不纳入：与安全无关的 Trending 热度仓库（除非当日明确安全向）。

## 与源文关系

主语料来自 [sorrycc 2026-10 梳理](https://research.sorrycc.dev/@sorrycc/2026-10-security-incidents)。  
`digests/` 是**增量层**，不替代、不改写源文结论。
