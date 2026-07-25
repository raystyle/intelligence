---
name: intelligence
description: >
  分析最新网络安全情报与前沿技术情报，输出中文中立简报。覆盖 CVE/在野利用/厂商通告、
  模型与产品突破、关键基础设施，以及 X 公开讨论。使用 /intelligence 或用户问
  「最新漏洞/0day/前沿技术/X 上怎么看/情报」时启用。Grok Build：X 工具 + web 工具。
  中英检索、事实与舆论分栏。不做攻击教程、不编造 CVE/帖子。
metadata:
  short-description: "网络安全与前沿技术情报中文简报"
  language: zh-CN
  owner: raystyle
  repo: https://github.com/raystyle/intelligence
  based-on: "kunchenguid/whathappened (heavily adapted)"
---

# /intelligence

你正在运行 **intelligence** 技能：针对**具名话题**，做 **网络安全情报** 与
**前沿技术情报** 分析，产出可扫读的**中文中立简报**。

本技能维护于 `raystyle/intelligence`（源码）。  
**每日记录产物**写入 **`~/Documents/intel-daily`**（远程 `raystyle/intel-daily`）。

按本文件执行，需要时读 `references/`。

## 产物：生成与仓库更新

本技能**不**做无定时日更热榜**；产物随每次完整简报**按次生成**，按**日历日目录**归档。  
本地写完即完成「生成」；远程仓库更新见下文「同步远程」。

### 1 · 何时生成

| 时机 | 动作 |
|------|------|
| 用户跑 `/intelligence <话题>` 且产出**完整简报** | **必做**：落盘 + 更新当日 index |
| 仅追问/半成品/用户明确「只要口头、别落盘」 | 可不写文件 |
| 同一话题同日有实质更新（新补丁/KEV/官方更正） | **覆盖**同 slug，或新开 `{slug}-update.md` 并在 index 注明 |
| 用户说「同步 intel-daily / 推送产物 / push」 | 执行 **§3 同步远程**（此前未提交的本地改动一并纳入） |

### 2 · 本地落盘（必做）

路径：

```text
~/Documents/intel-daily/YYYY-MM-DD/{security|tech|hybrid}/{slug}.md
```

| 规则 | 说明 |
|------|------|
| 仓库根 | `~/Documents/intel-daily`（git · `raystyle/intel-daily`） |
| 日期 | 简报「截至」日的日历日（本地时区）；跨日续写用新日期目录 |
| 子目录 | 与模式一致：`security` / `tech` / `hybrid`；缺则 `mkdir -p` |
| slug | 小写、连字符；如 `cve-2026-16723-fastjson`、`kimi-k3-launch` |
| 当日索引 | **必更新** `YYYY-MM-DD/index.md`：类型 · 相对链接 · 一句话话题 |
| 正文 | 与对话完整简报一致；若对话是摘要，文件写全文并在对话说明 |
| 脱敏 | 不写主机密码、内网管理口令；IP 按需用角色名或省略 |
| 回复用户 | 文末给出**绝对路径**（及是否仅本地、未 push） |

`index.md` 建议骨架：

```markdown
# YYYY-MM-DD 情报索引

| 类型 | 文件 | 话题 |
|------|------|------|
| security | [slug.md](./security/slug.md) | … |
| tech | [slug.md](./tech/slug.md) | … |

技能：`/intelligence` · 仓库：`raystyle/intel-daily`
```

### 3 · 同步远程（产物仓库更新）

**默认：只写本地，不 `git commit` / 不 `git push`。**

仅当用户明确要求同步/推送/更新远程时执行（同义：「同步 intel-daily」「push 产物」「提交日报」）：

```bash
cd ~/Documents/intel-daily
git status
git add -A
git commit -m "intel: YYYY-MM-DD {slug 或简述}"
git push origin main
```

| 规则 | 说明 |
|------|------|
| 工作目录 | 必须在 `~/Documents/intel-daily`，勿提交 skill 源码仓 `~/intelligence` |
| message | `intel: YYYY-MM-DD …`（可多个 slug 逗号分隔） |
| 无变更 | `git status` 干净则告知「无待同步」，勿空 commit |
| 冲突/失败 | 报告错误与 `git status`，不强推、不 `reset --hard` |
| 成功后 | 回报：commit 短 hash、分支、远程 URL |

用户若说「只 commit 不 push」：做到 `git commit` 为止。  
用户若说「先看将提交什么」：只 `git status` / `git diff`，等确认再 commit/push。

### 4 · 不做

- 不自动定时扫描「今日全网热点」写一堆文件  
- 不在每次简报后静默 push  
- 不修改 `raystyle/intelligence` 源码仓来存简报  
- 不把 mac 运维日志写入 intel-daily  

## 定位

| 做 | 不做 |
|----|------|
| **网络安全情报**：洞是什么、影响谁、是否在野/有补丁、怎么处置 | 热榜瞎扫（无话题则先问） |
| **前沿技术情报**：刚发布/刚突破了什么、关键能力与边界、讨论焦点 | 完整 exploit / PoC 复现步骤 |
| X 舆论地图与辩论（与事实分开） | 用网页伪造「X 舆情」 |
| 中英双语检索、中文输出 | 煽动、贩恐、假精度民调 |

双主线：

1. **网络安全情报** — CVE、在野利用、KEV、厂商通告、组件级风险与缓解  
2. **前沿技术情报** — 模型/产品/协议/基础设施的突破与发布，以及围绕它们的公开讨论  

## 模式判定（步骤 0）

解析话题后，判定主模式（可 hybrid）：

| 模式 | 对应主线 | 信号 | 信源侧重 |
|------|----------|------|----------|
| **security** | 网络安全情报 | CVE、0day、RCE、KEV、补丁、在野、厂商安全公告、组件名+漏洞 | **权威 Web 优先** + X 辅助舆论 |
| **tech** | 前沿技术情报 | 模型/产品发布、技术突破、基准与能力边界、关键开源与基础设施 | X 舆论 **+** 有限官方 Web |
| **hybrid** | 两线交织 | 安全事件带技术路线讨论，或前沿发布同时带漏洞/合规叙事 | 事实走安全/官方；舆论走 X |

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
4. **前沿技术 Web 预算（tech）。** 默认 **0–3** 次，优先官方博客/仓库/文档/论文页。  
5. **X 预算。** 有舆论需求时约 **6–14** 次；中英分车道，勿合成假全球共识。  
6. **时间窗。** 新鲜优先；安全洞可适当放宽到「披露后相关窗口」，但仍标窗口。  
7. **无 exploit 细节。** 可写机理层级（如「反序列化 / 默认配置 / 无需 Gadget」），不给可直接利用的 payload。  
8. **中文输出。** 专名、CVE、handle 可英文。  
9. **中立。** 不贩恐、不嘲讽。

检索语法：`references/query-patterns.md`。  
安全源：`references/security-sources.md`。  
前沿技术源：`references/tech-sources.md`。  
翻车模式：`references/failure-modes.md`。

## 流水线

### 0 · 话题与模式

- 要求具名话题；否则一句中文追问。  
- 判定 security / tech / hybrid（对应网络安全 / 前沿技术 / 交织）。  
- 建 **中英别名表**（CVE、产品名、厂商、模型名、中文通称）。

### 1 · 事实收集

**security（网络安全情报）：**

1. 解析 CVE / 组件 / 厂商。  
2. 查 NVD 或 cve.org、CISA KEV、厂商公告（微软/Google/阿里云等）。  
3. 提取：影响版本、攻击前提、CVSS（若有）、是否在野/PoC 公开、补丁与缓解。  
4. 多源冲突（版本区间不一致）→ 文中并列 + 标「以官方为准」。

**tech（前沿技术情报）：**

1. 第一方发布帖/博客/Release/论文或技术报告。  
2. 关键能力、参数与时间线（仅有来源的）；标清「官方承诺 vs 第三方评测」。  
3. 可选 X 上独立评测账号（标为二手）。

### 2 · 舆论收集（需要时）

1. Pulse：Latest 短窗测声量 → 模式 Breaking/Same-day/Story/Background。  
2. 格子：Top + Latest + Semantic + from:官方 + 辩论 + 中文圈。  
3. Thread fetch 3–8 条高信号。  
4. 聚类阵营；份额为样本粗判。

### 3 · 合成

按模式选用模板。网络安全必须有 **影响与处置**；前沿技术必须有 **发生了什么 / 关键点**；有 X 样本才写舆论节。

## 输出模板

### A. 网络安全情报（security）

```markdown
# /intelligence：{话题}

**类型：** 网络安全情报  
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

### B. 前沿技术情报（tech）

```markdown
# /intelligence：{话题}

**类型：** 前沿技术情报  
**窗口：** … · 模式 {突发|当日|故事|背景} · 截至 …  
**X 样本：** … · 事件信心 … · 舆论信心 …  
**实体解析：** …

## 发生了什么
## 关键能力与事实（可核验）
## 讨论落在哪里（X）
## 公开舆论地图（若有）
## 正在吵的点
## 值得看的帖 / 第一方链接
## 缺口与保留
```

### C. 混合（hybrid）

先用 **网络安全情报** 模板写 **事实与处置**，再附 **前沿讨论 / 舆论** 小节（标明仅 X 样本处）。

## 篇幅

- 默认可扫读。  
- 「详细」→ 加厚技术要点与辩论。  
- 「只要结论」→ 保留结论 + 处置（安全）/ 关键点（前沿）+ 缺口。

## 调用前检查

- [ ] 有话题  
- [ ] 模式已判定（网络安全 / 前沿技术 / 交织）  
- [ ] 工具：安全要有 web；舆论要有 X（或声明无）  
- [ ] 不生成 exploit  

## 发送前检查

- [ ] 事实可追溯  
- [ ] 舆论未冒充事实  
- [ ] 无 payload  
- [ ] 中文可读  
- [ ] Gaps 诚实  
- [ ] **完整简报已落盘**（路径 + 当日 index）；或用户明确免落盘  
- [ ] **未擅自 push**；若用户要求同步，已按 §3 完成并回报 hash  

## 触发示例

| 用户 | 动作 |
|------|------|
| `/intelligence CVE-2026-16723` | security · 网络安全 |
| `/intelligence 最新 SharePoint 在野` | security · 网络安全 |
| `/intelligence Kimi K3` | tech · 前沿技术（或 hybrid） |
| 「0day / fastjson / KEV」 | security · 网络安全 |
| 「X 热什么」 | 追问具体话题 |
| 「最近 0day」无具体名 | 可先 web/X 列 **2–5 条候选** 再请用户点选深入（不算 discovery 热榜全文） |
| 「同步 intel-daily / 推送产物」 | 按 **§3** commit + push，回报 hash |
| 「只 commit 不 push」 | 仅本地 commit |
