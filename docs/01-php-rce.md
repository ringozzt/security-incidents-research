# 1. PHP 核心未认证 RCE（CVE-2026-109618 / 109619）

**来源：** [sorrycc 2026-10 梳理 §1](https://research.sorrycc.dev/@sorrycc/2026-10-security-incidents)  
**可信度：** 已确认（php.net、Cisco Talos、watchTowr、VulnCheck、Datadog Security Labs）

## 概况

PHP 核心两个漏洞，CVSS v4.0 均为 **9.2**，未认证远程代码执行；影响可追溯至 PHP 5.4。本期**唯一确认被利用**的软件漏洞。

## 关键事实

| 项 | 内容 |
| --- | --- |
| 修复版本 | 8.5.12 / 8.4.27 / 8.3.36 / 8.2.35（**10-21** 发布） |
| 部署风险 | php.net：`mod_php` 最高；已有可在应用代码运行前触发的演示 |
| 披露 | 细节约 **10-20 15:20 UTC** 在禁运结束前流出；补丁 **10-21 14:00 UTC** |
| FPM | 109619 上传路径在 mod_php 与 PHP-FPM 均已确认；FPM 还需应用启动后特定操作；php.net：所有 Web 部署（含 FPM）受影响 |
| EOL | 7.4 / 8.0 / 8.1 无官方发版，有最小化 backport 分支；8.2 将于 2026-12-31 EOL |
| WAF | Talos：临时控制无法提供完整防护 |
| 日本提醒 | 高市早苗 10-21 21:55 JST 发紧急提醒，建议无法及时打补丁的站点暂时下线 |

## 在野利用

- Talos：截至补丁发布时未观察到。
- watchTowr 蜜罐：补丁当天已有在野利用。
- VulnCheck：`php2shell-scan/109618` UA 扫描；一次经 session upload-progress 植入 webshell 的尝试（未成功）。
- Datadog：多家厂商报告活动；转引 AWS 称尝试最早 **10-17**（补丁前四天；AWS 原始通报原文未找到）。
- **应按「已被利用」处理。**

## 公开 PoC（截至 10-22 05:30 UTC，据 VulnCheck）

- GitHub 已出现数百个 PoC 仓库，绝大多数无效，部分可能夹带恶意代码。
- 合格公开 PoC：仅能在 **ARM64**（可暴力绕 ASLR）或 **关闭 ASLR 的 x86-64** 上稳 RCE；尚无公开 PoC 能在开启 ASLR 的 x86-64 上独立完成。
- VulnCheck 商业利用已能在 x86-64 + ASLR + `display_errors=off` 的 PHP 8.4.26 上 RCE → 门槛可突破。

## 处置（原文）

1. 升级到修复版本，`mod_php` 优先。  
2. 7.4–8.1：发行版/LTS backport，或官方 backport 分支自编译，并列入迁移。  
3. 无法立即升级：应用 php.net 缓解配置并重启全部 worker（`ini_set()` / `.user.ini` 无效）；仅临时止血。  
4. 核对容器镜像与托管面板中的实际 PHP 版本。  
5. 公开 PoC 只在隔离环境测试。

## 常见误读（原文纠正）

- 「补丁不完整」：无机构称官方补丁对已知利用无效；接近说法来自 Datadog 对残留哈希表代码模式的风险提示。  
- 「高市早苗本人的 PHP 网站已被入侵」：无来源；报道只涉及其对运营者的提醒。
