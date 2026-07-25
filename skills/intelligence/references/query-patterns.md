# 检索配方（X + 安全关键词）

## X 核心算子

| 算子 | 用途 |
|------|------|
| `"精确短语"` | CVE、产品全名 |
| `OR` | 别名（大写 OR） |
| `from:user` | 第一方 |
| `since:YYYY-MM-DD` | 窗口 |
| `min_faves:N` | 降噪 |
| `filter:quote` | 辩论面 |

## 安全话题 Pulse

```text
"CVE-2026-16723" OR CVE-2026-16723 since:{近期}
"fastjson" (RCE OR 0day OR 在野 OR KEV OR PoC) since:{近期}
```

中文：

```text
fastjson (漏洞 OR 远程代码 OR 在野 OR 通告)
```

## 厂商 / 组件

```text
from:msftsecurity SharePoint (exploited OR zero-day OR KEV)
"SharePoint" (CVE-2026 OR RCE) min_faves:5
```

## 技术发布

```text
"{Product}" (launch OR released OR 发布 OR 开源) since:{今天}
from:{official} ("{Product}" OR announce OR 发布)
```

## 辩论

```text
("{Entity}") (scam OR overhyped OR mid OR 翻车 OR 真香 OR 不能用)
```

## 实体卫生

- 始终带主实体（CVE 或产品名）  
- 别名 1–3 个足够  
- 撞名时加厂商前缀：`Microsoft SharePoint`、`Alibaba fastjson`  
