# 网络安全情报信源优先级

事实类结论按下列优先级采信；冲突时并列并标注来源。

## P0 · 权威与官方

| 源 | 用途 |
|----|------|
| https://www.cve.org | CVE 记录 |
| https://nvd.nist.gov | 描述、引用、部分指标 |
| https://www.cisa.gov/known-exploited-vulnerabilities-catalog | **在野利用**官方清单（KEV） |
| https://www.cisa.gov/news-events/alerts | KEV 增补与专项警报 |
| 厂商 PSIRT / Security Update Guide | 微软 MSRC、Google、Apple、Cisco、Fortinet、Adobe… |
| 组件官方仓库 Security Advisory / Wiki | 如 GitHub Security Advisories、项目 wiki |

## P1 · 一线应急与云厂商通报（尤其中文环境）

| 源 | 用途 |
|----|------|
| 国家漏洞库 / CNNVD 通报 | 国内通报口径 |
| 阿里云漏洞库 AVD、腾讯云安全公告、华为云等 | 影响说明与缓解 |
| 微步、奇安信、绿盟、NSFOCUS 等 CERT 通告 | 在野/PoC/复现状态（二手，需交叉） |

## P2 · 专业媒体（辅助，不单源定论）

The Hacker News、Dark Reading、SecurityWeek、BleepingComputer、SOC Prime 等。  
用于时间线与上下文；**版本区间与补丁以 P0 为准**。

## P3 · X / 社交媒体

- 可用于：最早披露线索、研究员线程、利用观测传闻。  
- **不可单独**作为「已在野」「影响版本」的最终依据。  
- 与事实节分开写。

## 采集清单（security 模式建议）

1. CVE 页（cve.org 或 NVD）  
2. 厂商公告 / GHSA  
3. CISA KEV 是否收录（搜 CVE 编号）  
4. 1–2 家应急通告（缓解步骤）  
5. 可选：X 上 `CVE-…` / 组件名 Latest（舆论与最早传播）

## 写入每日记录产物时的固定字段

- 影响版本（多源不一致则并列）  
- 攻击前提（认证、默认配置、出网、用户交互）  
- 利用状态：未证实 / PoC 公开 / 多家称在野 / 已入 KEV  
- 修复：已发补丁版本 / 仅缓解 / 迁移路径  
- 对本环境相关性（有无该组件）

## 禁止

- 完整 exploit 代码、可复制 payload  
- 未验证的「全版本通杀」夸张标题当事实  
- 仅凭自媒体标题写 CVSS  
