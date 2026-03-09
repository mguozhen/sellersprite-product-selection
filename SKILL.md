---
name: sellersprite-product-research
description: "卖家精灵选品研究 Skill — 基于卖家精灵 API，输入关键词或品类，自动拉取市场数据并 AI 分析，输出选品机会报告：蓝海指数、竞争态势、推荐切入点、风险预警。Triggers: 选品, product research, 卖家精灵, sellersprite, 选产品, 竞品分析, 蓝海市场, 亚马逊选品, amazon product research, 市场分析"
allowed-tools: Bash
license: MIT
metadata:
  openclaw:
    homepage: https://github.com/mguozhen/sellersprite-product-research
---

# 卖家精灵选品助手

> 输入关键词 / 品类，AI 自动调用卖家精灵 API 抓取市场数据，输出双语选品分析报告。
> Input keyword / category, AI calls SellerSprite API to fetch market data and outputs bilingual product selection report.

## 前置要求 / Prerequisites

设置卖家精灵 API Key（在 [open.sellersprite.com](https://open.sellersprite.com) 获取）：

```bash
export SELLERSPRITE_SECRET_KEY="your-secret-key"
```

确保已安装 `openclaw` CLI（用于 AI 分析）：

```bash
# 检查 openclaw
which openclaw
```

## 快速使用 / Quick Start

```bash
# 基础选品分析（关键词）
bash ~/.claude/skills/sellersprite-product-research/selection.sh --keyword "wireless earbuds"

# 指定市场（默认 US）
bash ~/.claude/skills/sellersprite-product-research/selection.sh --keyword "yoga mat" --marketplace UK

# 指定月份
bash ~/.claude/skills/sellersprite-product-research/selection.sh --keyword "phone case" --month 202501

# 分析竞品（通过 ASIN）
bash ~/.claude/skills/sellersprite-product-research/selection.sh --asin B08N5WRWNW --marketplace US

# 保存报告
bash ~/.claude/skills/sellersprite-product-research/selection.sh --keyword "LED strip" --output report.md
```

## 输出示例 / Output Example

```
╔══════════════════════════════════════════════════════════════╗
║      卖家精灵选品报告 / SellerSprite Product Report         ║
║  关键词: wireless earbuds  |  市场: US  |  2026-03-09       ║
╚══════════════════════════════════════════════════════════════╝

📊 市场概览 / Market Overview
──────────────────────────────────────
  产品数量    Products   ████████████░  1,284
  平均月销量  Avg Units  ████████░░░░░  456/月
  平均价格    Avg Price  $28.5
  平均评分数  Avg Rtgs   3,210
  蓝海指数    Blue Ocean ████░░░░░░░░░  3.2 / 10

🔴 风险预警 / Risk Signals
══════════════════════════════════════════════
1. 竞争激烈 / High Competition（TOP10 月销量集中度 78%）
2. 头部品牌壁垒 / Brand Barrier（Anker/JBL 占据 35% 份额）
...

🟢 机会窗口 / Opportunity Windows
══════════════════════════════════════════════
1. 价格段空白 / Price Gap（$15-$20 段竞品少，搜索量仍旺）
2. 新品红利 / New Product Bonus（近90天新品销量增长 42%）
...

🎯 推荐切入策略 / Recommended Entry Strategy
══════════════════════════════════════════════
1. 差异化定位 $15-18 价格段，主打运动防水功能...

📌 TOP 5 参考产品 / Top 5 Reference Products
──────────────────────────────────────────────
ASIN         月销量   价格   评分   配送
B0XXXXXXXX   2,340   $19.9   4.3   FBA  ⭐ Best Seller
...
```

## 工作流程 / How It Works

```
① 输入关键词 / ASIN / 品类
      ↓
② 调用卖家精灵 API（选产品 + 关键词选品）
      ↓
③ 解析市场数据（销量、价格、竞争、趋势）
      ↓
④ Claude AI 深度分析（蓝海评估 + 策略建议）
      ↓
⑤ 输出双语结构化选品报告
```

## 脚本文件 / Scripts

| 文件 | 说明 |
|---|---|
| `selection.sh` | 主入口脚本 |
| `fetch.sh` | 卖家精灵 API 数据拉取 |
| `analyze.sh` | AI 选品分析脚本 |

## Marketplace 代码 / Marketplace Codes

| 代码 | 市场 |
|---|---|
| US | 美国 |
| UK | 英国 |
| DE | 德国 |
| JP | 日本 |
| CA | 加拿大 |
| FR | 法国 |
| IT | 意大利 |
| ES | 西班牙 |
| MX | 墨西哥 |
| AU | 澳大利亚 |

## 注意事项 / Notes

- API 调用限制：每分钟 40 次，每次最多返回 100 条
- 同一条件最多获取 TOP 2000 数据
- 单次分析约消耗 3,000-8,000 tokens（$0.02-$0.05）
- 建议查询近 1-3 个月数据，确保时效性
