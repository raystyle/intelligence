# intelligence 设计要点

人读摘要。**运行契约以 `skills/intelligence/SKILL.md` 为准。**

## 定位

| | |
|--|--|
| 名称 | intelligence · `/intelligence` |
| 源码 | `raystyle/intelligence` |
| 产物远程 | `raystyle/intel-daily` |
| 产物本机 | `~/Documents/intel-daily`（本地 clone 约定，可改） |
| 宿主 | Grok Build（web + X 工具） |

双主线：**网络安全情报**、**前沿技术情报**（可 hybrid）。

不是：热榜发现器、攻击教程、payload 投喂、未验证 0day 合集搬运。  
允许：分析公开 PoC 真伪/水分，并给出处置信度（不附可复制利用材料）。

## 模式

| 模式 | 主线 | 信源 |
|------|------|------|
| security | 网络安全 | 权威 Web 优先 + X 舆论 |
| tech | 前沿技术 | 官方 Web 有限 + X 舆论 |
| hybrid | 交织 | 事实走安全/官方；舆论走 X |

## 产物与同步

有结论的交互结束 → **必产物 + 必同步**（同一轮）：

1. 在本机产物目录写 `YYYY-MM-DD/{security\|tech\|hybrid}/{slug}.md`  
2. 更新当日 `index.md`  
3. 在该目录 `commit` + `push` 到 `raystyle/intel-daily`  
4. 回复报告路径与 commit hash  

本机目录默认 `~/Documents/intel-daily`（**本机设置**的远程仓 clone，可用 `INTEL_DAILY_ROOT` 覆盖）。  
无结论（仅追问）或用户明确免落盘/免 push → 跳过。

## 安全边界

- 不编造 CVE / 帖子 / 链接  
- 事实与舆论分栏  
- 不交付完整 exploit / 可复制 payload；可做 PoC 真伪与处置信度分析  
- 缺口诚实（版本冲突、KEV 滞后、样本偏倚）  

## 仓库边界

| 仓库 | 内容 |
|------|------|
| intelligence | 技能源码与契约（本仓） |
| intel-daily | 每日记录产物（远程）；本机 clone 路径自定 |

每日记录产物只进产物仓，不进本仓。
