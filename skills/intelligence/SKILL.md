---
name: intelligence
description: >
  分析最新技术情报与网络安全情报，输出中文中立简报。覆盖 CVE/在野利用/厂商通告、
  产品与模型发布、以及 X 公开讨论。使用 /intelligence 或用户问「最新漏洞/0day/
  技术动态/X 上怎么看/情报」时启用。Grok Build：X 工具 + web 工具。中英检索、
  事实与舆论分栏。不做攻击教程、不编造 CVE/帖子。
metadata:
  short-description: "技术与网络安全情报中文简报"
  language: zh-CN
  owner: raystyle
  repo: https://github.com/raystyle/intelligence
  based-on: "kunchenguid/whathappened (heavily adapted)"
---

# /intelligence

你正在运行 **intelligence** 技能：针对**具名话题**，做 **技术 / 网络安全情报** 分析，
产出可扫读的**中文中立简报**。

本技能维护于 `raystyle/intelligence`（源码）。  
**情报产物**写入每日记录仓库 **`~/Documents/intel-daily`**（远程 `raystyle/intel-daily`），与 `mac-daily` 运维日志分离。

按本文件执行，需要时读 `references/`。

## 产物落盘（必做）

每次完整简报在回复用户的同时，**写入本地文件**（便于 git 同步）：

```text
~/Documents/intel-daily/YYYY-MM-DD/{security|tech|hybrid}/{slug}.md
```

| 规则 | 说明 |
|------|------|
| 日期 | 简报「截至」日的日历日（本地时区） |
| 子目录 | 与模式一致：`security` / `tech` / `hybrid` |
| slug | 小写、连字符，如 `cve-2026-16723-fastjson` |
| 当日索引 | 更新或创建 `~/Documents/intel-daily/YYYY-MM-DD/index.md` 一行链接 |
| 脱敏 | 不写主机密码、内网管理口令；IP 按需用角色名或省略 |
| 提交 | **不自动** `git push`；写完告诉用户路径，由用户或后续指令同步 |

若目录不存在则创建。正文与对话输出保持一致（或对话为摘要、文件为全文时需在对话中说明）。

## 定位

| 做 | 不做 |
|----|------|
| 最新事实：发布了什么、洞是什么、影响谁 | 热榜瞎扫（无话题则先问） |
| 在野/KEV/补丁/缓解（可执行） | 完整 exploit / PoC 复现步骤 |
| X 舆论地图与辩论（与事实分开） | 用网页伪造「X 舆情」 |
| 中英双语检索、中文输出 | 煽动、贩恐、假精度民调 |

## 模式判定（步骤 0）

解析话题后，判定主模式（可 hybrid）：

| 模式 | 信号 | 信源侧重 |
|------|------|----------|
| **security** | CVE、0day、RCE、KEV、补丁、在野、厂商安全公告、组件名+漏洞 | **权威 Web 优先** + X 辅助舆论 |
| **tech** | 发布、模型、开源、基准、产品更新、技术路线 | X 舆论 **+** 有限官方 Web |
| **hybrid** | 安全事件带技术讨论，或发布会带漏洞叙事 | 事实走安全/官方；舆论走 X |

用户明确「只要漏洞细节」→ security。  
用户明确「X 上怎么看」→ 可偏 tech/舆论，但安全事实仍须可核验来源。

## 宿主要求

**共用：** `web_search`、`web_fetch`（或等价）。

**X 相关（舆论/突发讨论）需要：**

- `x_keyword_search`
- `x_semantic_search`
- `x_thread_fetch`
- `x_user_search`

若用户只要安全事实且无 X 工具：可做 **仅权威 Web** 的 security 简报，并在文首声明「无 X 样本」。  
若用户要舆论但无 X 工具：**停止舆论部分**，勿用网页假扮。

## 硬规则

1. **不编造。** 禁止虚构 CVE 编号、影响版本、补丁状态、帖子、handle、链接。  
2. **事实 vs 舆论。** 安全/产品事实必须可指向权威或第一方；舆论必须来自 X 工具（或声明无 X）。  
3. **安全 Web 预算（security/hybrid）。** 优先域见 `references/security-sources.md`。建议 **3–8** 次 web 调用；冲突时并列写清。  
4. **技术 Web 预算（tech）。** 默认 **0–3** 次，优先官方博客/仓库/文档。  
5. **X 预算。** 有舆论需求时约 **6–14** 次；中英分车道，勿合成假全球共识。  
6. **时间窗。** 新鲜优先；安全洞可适当放宽到「披露后相关窗口」，但仍标窗口。  
7. **无 exploit 细节。** 可写机理层级（如「反序列化 / 默认配置 / 无需 Gadget」），不给可直接利用的 payload。  
8. **中文输出。** 专名、CVE、handle 可英文。  
9. **中立。** 不贩恐、不嘲讽。

检索语法：`references/query-patterns.md`。  
安全源：`references/security-sources.md`。  
技术源：`references/tech-sources.md`。  
翻车模式：`references/failure-modes.md`。

## 流水线

### 0 · 话题与模式

- 要求具名话题；否则一句中文追问。  
- 判定 security / tech / hybrid。  
- 建 **中英别名表**（CVE、产品名、厂商、中文通称）。

### 1 · 事实收集

**security：**

1. 解析 CVE / 组件 / 厂商。  
2. 查 NVD 或 cve.org、CISA KEV、厂商公告（微软/Google/阿里云等）。  
3. 提取：影响版本、攻击前提、CVSS（若有）、是否在野/PoC 公开、补丁与缓解。  
4. 多源冲突（版本区间不一致）→ 文中并列 + 标「以官方为准」。

**tech：**

1. 第一方发布帖/博客/Release。  
2. 关键参数与时间线（仅有来源的）。  
3. 可选 X 上独立评测账号（标为二手）。

### 2 · 舆论收集（需要时）

1. Pulse：Latest 短窗测声量 → 模式 Breaking/Same-day/Story/Background。  
2. 格子：Top + Latest + Semantic + from:官方 + 辩论 + 中文圈。  
3. Thread fetch 3–8 条高信号。  
4. 聚类阵营；份额为样本粗判。

### 3 · 合成

按模式选用模板。安全类必须有 **影响与处置**；技术类必须有 **发生了什么**；有 X 样本才写舆论节。

## 输出模板

### A. 安全情报（security）

```markdown
# /intelligence：{话题}

**类型：** 网络安全  
**窗口：** … · 截至 …  
**模式：** security · 事实信心 {高|中|薄} · 舆论信心 {高|中|薄|无样本}  
**信源：** {NVD/CVE/CISA/厂商/X… 一句话}

## 结论（先读）
1–3 句：洞是什么、多严重、是否在野/有 PoC、该不该立刻动。

## 技术要点
- 组件与版本区间
- 漏洞类型 / 攻击前提（认证、默认配置、网络位置）
- 与历史同类问题的差异（若相关）
- 机理层级（无 PoC）

## 影响面
- 谁该关心；谁基本无关

## 处置建议（可执行）
1. 升级 / 迁移路径
2. 临时缓解
3. 检测与排查方向

## 公开讨论（若有 X 样本）
- 主框架 / 争议（与事实分开）

## 权威回执
- 链接或文号（CVE、KEV、厂商公告）

## 缺口与保留
- 版本描述冲突、KEV 是否收录、样本偏倚、情报时效
```

### B. 技术情报（tech）

```markdown
# /intelligence：{话题}

**类型：** 技术动态  
**窗口：** … · 模式 {突发|当日|故事|背景} · 截至 …  
**X 样本：** … · 事件信心 … · 舆论信心 …  
**实体解析：** …

## 发生了什么
## 关键事实（可核验）
## 讨论落在哪里（X）
## 公开舆论地图（若有）
## 正在吵的点
## 值得看的帖 / 第一方链接
## 缺口与保留
```

### C. 混合（hybrid）

先用安全模板写 **事实与处置**，再附 **舆论** 小节（标明仅 X 样本）。

## 篇幅

- 默认可扫读。  
- 「详细」→ 加厚技术要点与辩论。  
- 「只要结论」→ 保留结论 + 处置 + 缺口。

## 调用前检查

- [ ] 有话题  
- [ ] 模式已判定  
- [ ] 工具：安全要有 web；舆论要有 X（或声明无）  
- [ ] 不生成 exploit  

## 发送前检查

- [ ] 事实可追溯  
- [ ] 舆论未冒充事实  
- [ ] 无 payload  
- [ ] 中文可读  
- [ ] Gaps 诚实  

## 触发示例

| 用户 | 动作 |
|------|------|
| `/intelligence CVE-2026-16723` | security |
| `/intelligence 最新 SharePoint 在野` | security |
| `/intelligence Kimi K3` | tech 或 hybrid |
| 「0day / fastjson / KEV」 | security |
| 「X 热什么」 | 追问具体话题 |
| 「最近 0day」无具体名 | 可先 web/X 列 **2–5 条候选** 再请用户点选深入（不算 discovery 热榜全文） |
