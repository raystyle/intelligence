# intelligence

Grok Build 技能：**网络安全情报**与**前沿技术情报**中文中立每日记录产物。

| | |
|--|--|
| 技能 | `/intelligence` |
| 源码 | https://github.com/raystyle/intelligence |
| 产物远程 | https://github.com/raystyle/intel-daily |
| 产物本机目录 | `~/Documents/intel-daily`（**本机自己设的** clone 路径，非通用安装目录） |
| 规范 | [Agent Skills](https://agentskills.io) · `skills/*/SKILL.md` |

## 安装（`gh skill`）

需 [GitHub CLI](https://cli.github.com/) 且已登录（`gh auth login`）。

### 发现

```bash
# 列出本仓库技能
gh skill install raystyle/intelligence

# 预览内容
gh skill preview raystyle/intelligence intelligence
```

### 装到 Grok Build（推荐 · 用户级）

当前 `gh skill` 尚无独立 `grok` agent 名，装到 Grok 原生目录用 `--dir`：

```bash
gh skill install raystyle/intelligence intelligence \
  --dir ~/.grok/skills \
  --force
```

装好后目录应为：

```text
~/.grok/skills/intelligence/SKILL.md
~/.grok/skills/intelligence/references/...
```

在 Grok 中：

```text
/intelligence CVE-2026-16723
/intelligence Kimi K3
```

### 其它宿主 / 作用域

```bash
# 项目级（写入当前仓库 .agents/skills 等，视 --agent 而定）
gh skill install raystyle/intelligence intelligence --scope project

# 指定 agent（示例）
gh skill install raystyle/intelligence intelligence --agent claude-code --scope user
gh skill install raystyle/intelligence intelligence --agent cursor --scope user

# 装本仓库全部技能（目前仅 intelligence）
gh skill install raystyle/intelligence --all --dir ~/.grok/skills --force
```

### 更新

```bash
gh skill update intelligence
# 或
gh skill install raystyle/intelligence intelligence --dir ~/.grok/skills --force
```

### 开发机：本地源码 symlink（改完即生效）

```bash
mkdir -p ~/.grok/skills
ln -sfn ~/intelligence/skills/intelligence ~/.grok/skills/intelligence
```

与 `gh skill install` 二选一；symlink 便于改契约，`gh skill` 便于干净安装与 `gh skill update`。

## 做什么

双主线：

1. **网络安全情报** — CVE / 在野 / KEV / 厂商通告 / 可执行处置  
2. **前沿技术情报** — 模型·产品突破、关键能力与边界、关键基础设施  

**有结论 → 必落盘 + 必同步** 到产物仓（见 skill 契约）。

## 产物约定

| | |
|--|--|
| 远程 | `raystyle/intel-daily`（每日记录产物真正同步到的 GitHub 仓） |
| 本机目录 | `~/Documents/intel-daily` — **本工作区本地设置**：把远程仓 clone 到这里，便于和 `mac-daily` 一样放在 `~/Documents` 下按日归档 |
| 可覆盖 | 环境变量 `INTEL_DAILY_ROOT` = 任意本地 clone 路径 |

目录形态：

```text
${INTEL_DAILY_ROOT:-~/Documents/intel-daily}/
  YYYY-MM-DD/
    security|tech|hybrid/{slug}.md
    index.md
```

本机首次准备（路径可自选，与 skill 默认一致即可）：

```bash
# 本机约定路径示例（可改成你喜欢的目录，并 export INTEL_DAILY_ROOT=...）
git clone https://github.com/raystyle/intel-daily.git ~/Documents/intel-daily
```
## 布局

```text
skills/intelligence/          # ← gh skill 发现的技能包
  SKILL.md
  references/
    query-patterns.md
    security-sources.md
    tech-sources.md
    failure-modes.md
docs/design.md                # 人读设计要点（非运行时）
examples/                     # 每日记录产物骨架示例
```

## 发布校验

```bash
cd ~/intelligence
gh skill publish --dry-run    # 校验 Agent Skills 规范
# 正式发版（打 tag + Release）：
# gh skill publish --tag v1.0.0
```

## 与上游

灵感来自 [kunchenguid/whathappened](https://github.com/kunchenguid/whathappened)，定位不同：覆盖网络安全事实 + 前沿技术；安全事实优先权威 Web。

## 许可

MIT（见 `LICENSE`）。
