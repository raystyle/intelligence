# intelligence

技术情报与网络安全情报分析技能（Grok Build Agent Skill）。

面向 **最新动态**：产品/模型发布、技术突破、CVE / 在野利用 / 厂商通告，以及 X 上的公开讨论。  
输出默认 **中文**，中英双语检索。

| | |
|--|--|
| 本地路径 | `~/intelligence` |
| 远程（暂不同步） | https://github.com/raystyle/intelligence |
| 技能名 | `/intelligence` |
| 运行时链接 | `~/.grok/skills/intelligence` → 本仓库 `skills/intelligence` |

## 做什么

```text
/intelligence <话题>
/intelligence CVE-2026-16723
/intelligence Kimi K3
```

或自然语言：「最新 SharePoint 在野利用」「X 上怎么看某某发布」。

产出中立简报：事实时间线、影响面、舆论/争议（若相关）、权威回执与缺口。

## 安装到 Grok（本机）

开发时用符号链接，改仓库即生效：

```bash
mkdir -p ~/.grok/skills
ln -sfn ~/intelligence/skills/intelligence ~/.grok/skills/intelligence
```

## 布局

```text
skills/intelligence/
  SKILL.md                 # 主契约
  references/
    query-patterns.md      # X 检索
    security-sources.md    # 安全情报权威源与优先级
    tech-sources.md        # 技术情报源与优先级
    failure-modes.md       # 失败模式
docs/
  design.md                # 设计说明（本地完善用）
examples/
  brief-security.md        # 安全类输出示例骨架
  brief-tech.md            # 技术类输出示例骨架
```

## 与其它仓库分工

| 仓库 | 本地 | 用途 | 同步 |
|------|------|------|------|
| **intelligence** | `~/intelligence` | 技能源码与契约（`/intelligence`） | 暂不同步 |
| **intel-daily** | `~/Documents/intel-daily` | 情报每日记录**产物** `YYYY-MM-DD/{security\|tech\|hybrid}/` | 与远程同步 |
| **mac-daily** | `~/Documents/mac-daily` | 本机运维与部署日志 | 与远程同步 |

产物约定与 grok-workspace / mac-daily 一样按**日期目录**落盘；技能写完简报后告知路径，**不自动** `git push`。

## 与上游关系

灵感来自 [kunchenguid/whathappened](https://github.com/kunchenguid/whathappened)，但定位不同：

- 不止 X 舆论，还覆盖 **技术与网络安全事实情报**
- 安全事实允许受控使用 **权威网页源**（CISA/NVD/厂商公告等）
- 中文输出、中英检索、双信心（事实/舆论）

## 开发

```bash
cd ~/intelligence
$EDITOR skills/intelligence/SKILL.md
# 完善满意后再:
# git add -A && git commit -m "..."
# git push -u origin main   # 你说先不同步
```

## 许可

MIT（见 `LICENSE`）。
