# intelligence

Grok Build 技能：**网络安全情报**与**前沿技术情报**中文中立简报。

| | |
|--|--|
| 技能 | `/intelligence` |
| 本地 | `~/intelligence` |
| 远程 | https://github.com/raystyle/intelligence |
| 运行时 | `~/.grok/skills/intelligence` → `skills/intelligence` |
| 产物 | https://github.com/raystyle/intel-daily · `~/Documents/intel-daily` |

## 做什么

```text
/intelligence <话题>
/intelligence CVE-2026-16723
/intelligence Kimi K3
```

或自然语言：「最新 SharePoint 在野」「某某模型刚发布 X 上怎么看」。

双主线：

1. **网络安全情报** — CVE / 在野 / KEV / 厂商通告 / 可执行处置  
2. **前沿技术情报** — 模型·产品突破、关键能力与边界、关键基础设施  

输出中文简报：结论、事实、舆论（若有 X 样本）、权威回执与缺口。  
**有结论 → 必落盘 + 必同步** 到 `intel-daily`（详见 skill 契约）。

## 安装（本机）

```bash
mkdir -p ~/.grok/skills
ln -sfn ~/intelligence/skills/intelligence ~/.grok/skills/intelligence
```

## 布局

```text
skills/intelligence/
  SKILL.md                 # 主契约（Agent 执行）
  references/
    query-patterns.md      # X / 关键词检索
    security-sources.md    # 网络安全权威源
    tech-sources.md        # 前沿技术信源
    failure-modes.md       # 失败模式
docs/
  design.md                # 设计要点（人读，非运行时）
examples/
  brief-security.md        # 网络安全简报骨架
  brief-tech.md            # 前沿技术简报骨架
```

## 产物约定

```text
~/Documents/intel-daily/YYYY-MM-DD/{security|tech|hybrid}/{slug}.md
~/Documents/intel-daily/YYYY-MM-DD/index.md
```

| 规则 | 说明 |
|------|------|
| 触发 | 一次情报交互**形成结论**时 |
| 动作 | 写文件 → 更新 index → `git commit` + `git push` intel-daily |
| 例外 | 仅追问无结论；或用户明确免落盘/免 push |

技能源码仓（本仓库）与产物仓分离：简报**不**写入 `raystyle/intelligence`。

## 与上游

灵感来自 [kunchenguid/whathappened](https://github.com/kunchenguid/whathappened)，定位不同：

- 覆盖网络安全事实 + 前沿技术，不止 X 舆论  
- 安全事实优先权威 Web（CISA / NVD / 厂商）  
- 中文输出、中英检索、事实与舆论分栏  

## 开发

```bash
cd ~/intelligence
$EDITOR skills/intelligence/SKILL.md
git add -A && git status
git commit -m "..."
git push origin main
```

## 许可

MIT（见 `LICENSE`）。
