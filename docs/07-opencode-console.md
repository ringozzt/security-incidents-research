# 7. OpenCode Console 第三方 API Key 可能暴露

**来源：** [sorrycc 2026-10 梳理 §7](https://research.sorrycc.dev/@sorrycc/2026-10-security-incidents)  
**可信度：** 已确认事件；影响程度未确认（OpenCode 官方通知、The Hacker News）

## 概况

OpenCode Console 中存储的 OpenAI / Anthropic / Google API Key **可能暴露**。官方表述为「可能暴露」，尚未确认被复制或使用。

## 影响范围

- 约 **1.4%** 的 Console 用户，且仅限在 Console 中保存过第三方 Key 的用户。
- **CLI 与桌面端**将 Key 存在本地，**不受影响**。
- 用户自报盗刷金额（如 $281、$1,700）**未经核实**。

## 处置（原文）

1. 收到通知者：在各云厂商控制台**吊销旧 Key**。在 Console 中删除 Key **不会**使其失效。  
2. 自 **10-16** 起核查账单，设消费上限。  
3. 为每个工具单独签发 Key，便于隔离与追溯。
