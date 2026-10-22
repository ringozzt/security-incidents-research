# 8. Linux 内核 PwnEscape（CVE-2026-99999）

**来源：** [sorrycc 2026-10 梳理 §8](https://research.sorrycc.dev/@sorrycc/2026-10-security-incidents)  
**可信度：** 已确认（Linux 内核 CNA CVE 记录；NVD 状态为 Received，尚未完成分析）

## 概况

内核 IPv4 栈中 `skb_checksum_help` **未校验 `checksum_start`**。恶意客户机经 **virtio-net** 提交伪造校验和元数据，经宿主机 TAP 进入 IPv4 分片路径，篡改已校验 IPv4 头，进而读宿主机内存、使宿主机崩溃、或以管理员权限执行命令。

## 关键事实

| 项 | 内容 |
| --- | --- |
| CVE 发布 | Linux 内核 CNA，**10-15**；标题 ipv4: validate checksum_start before completing checksum |
| CVSS 3.1 | **9.3**（`AV:L/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H`） |
| 攻击前提 | 只需能从分配给自己的 virtio-net 发包；无需宿主机凭据、无竞态 |
| 攻击面 | **客户机→宿主机虚拟设备路径**，不是从互联网收包的路径 |
| 上游修复 | 提交 `6785011f8b16`；已回合多个 stable（CVE 列 8 个）；第三方列出版本含 6.18.57、6.12.113、6.6.159、6.1.190、5.15.223、5.10.272 等 |
| 发现者 | Paulos；引用 pwn.ai 文章与 Vercel Sandbox 挑战赛报告 |
| 说明 | 修复称「纵深防御」，更完整入口校验仍在开发 |

## 影响范围（原文）

- AWS：托管服务不受影响。  
- 日本 Sakura Internet：10-16 暂停新 VPS 开通。  
- 发行版进度不一（Debian 12/13、Amazon Linux 2023 已发公告；Proxmox 部分系列尚未说明）。  
- 「波及各家主流虚拟机厂商」夸大；风险集中在跑**不可信客户机**的自管 KVM / Firecracker 宿主机。

## 处置

1. 多租户 VPS / 沙箱 / CI / Agent 沙箱等宿主机优先升到含 `6785011f8b16` 的内核并重启。  
2. 以发行版安全公告为准，不要只看上游版本号。  
3. 关注后续更完整入口校验补丁。
