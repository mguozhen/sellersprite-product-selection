# sellersprite-product-research

AI-powered Amazon product selection skill for [Claude Code](https://claude.ai/code) and [OpenClaw](https://openclaw.ai). Input a keyword or ASIN, get a deep bilingual product opportunity report — powered by SellerSprite's real market data.

## What it does

Calls the SellerSprite Open API to pull live Amazon market data and runs it through your OpenClaw model for strategic product selection analysis.

- **Blue Ocean Index** — 0–10 score measuring how easy it is to enter the market
- **Market overview** — avg price, monthly sales, revenue, FBA ratio, brand concentration
- **Risk signals** — competition level, brand barriers, rating thresholds
- **Opportunity windows** — price gaps, feature differentiation, niche segments
- **Entry strategies** — concrete positioning and launch recommendations
- **Recommended price range** — data-backed pricing sweet spot
- **Reference ASINs** — top products worth benchmarking
- **Bilingual output** — every insight in both Chinese and English

## Install

### Claude Code
```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/mguozhen/sellersprite-product-research.git sellersprite-product-research
```

### OpenClaw
```bash
clawhub install sellersprite-product-research
```

## Setup

1. **SellerSprite API Key** — get yours at [open.sellersprite.com](https://open.sellersprite.com):
   ```bash
   export SELLERSPRITE_SECRET_KEY="your-secret-key"
   ```

2. **OpenClaw** — the skill uses your currently configured model. No extra API key needed:
   ```bash
   # Check your current model
   openclaw models status
   ```

## Usage

### Natural language (just talk to Claude)
- "帮我选品 wireless earbuds，美国市场"
- "分析 yoga mat 的市场竞争情况"
- "Do a product research on 'phone stand' for US market"
- "竞品分析 ASIN B08N5WRWNW"

### Command line
```bash
# Keyword research (US market, default)
bash ~/.claude/skills/sellersprite-product-research/selection.sh --keyword "wireless earbuds"

# Specify marketplace
bash ~/.claude/skills/sellersprite-product-research/selection.sh --keyword "yoga mat" --marketplace UK

# Competitor analysis via ASIN
bash ~/.claude/skills/sellersprite-product-research/selection.sh --asin B08N5WRWNW

# Save report to file
bash ~/.claude/skills/sellersprite-product-research/selection.sh --keyword "LED strip" --output report.md

# Specify month
bash ~/.claude/skills/sellersprite-product-research/selection.sh --keyword "phone case" --month 202502
```

## Sample Output

```
╔══════════════════════════════════════════════════════════════╗
║      卖家精灵选品报告 / SellerSprite Product Report         ║
║  关键词: wireless earbuds  |  市场: US  |  2026-03-09       ║
╚══════════════════════════════════════════════════════════════╝

📊 市场概览 / Market Overview
──────────────────────────────────────
  产品数量  Products     1,284 条
  平均月销量 Avg Units   456 件/月
  平均价格  Avg Price    $28.50
  价格区间  Price Range  $8.99 — $89.99
  平均评分数 Avg Ratings 3,210
  FBA 比例  FBA Ratio    82.4%
  头部集中度 Top10 Conc  68.3%

🌊 蓝海指数 / Blue Ocean Index
──────────────────────────────────────
  ████░░░░░░░░░░░░  3.8 / 10  🟡 一般

🔴 风险预警 / Risk Signals
1. 头部品牌壁垒高 / Strong brand barrier（Anker/JBL 占 35%）
2. 平均评分门槛高 / High rating threshold（均值 3,200+）
3. 价格战激烈 / Price war（$15–30 段竞品密集）

🟢 机会窗口 / Opportunity Windows
1. [价格空白] $12–16 价格段竞品稀少，搜索量仍旺盛
2. [功能差异] 运动防水款评分低，用户有明确需求
3. [细分人群] 老年人专用（大音量+简单操作）几乎空白

🎯 推荐切入策略
1. 主打 $13.99 运动防水，差异化对抗红海主力价格段...

💰 推荐定价区间 / Recommended Price Range
  $12.99 — $16.99
  避开红海竞争最密集区间，保留利润空间

📌 TOP 参考产品
  B0XXXXXXXX  月销量 TOP，性价比标杆，适合对标
  B0YYYYYYYY  新品爆款，近90天增速最快

📋 选品结论 / Final Verdict
  市场竞争激烈但存在细分机会，建议以差异化功能+精准人群切入
  Competitive market with niche opportunities; enter with differentiated features
```

## Supported Marketplaces

| Code | Market |
|---|---|
| US | United States |
| UK | United Kingdom |
| DE | Germany |
| JP | Japan |
| CA | Canada |
| FR | France |
| IT | Italy |
| ES | Spain |
| MX | Mexico |
| AU | Australia |

## File Structure

```
sellersprite-product-research/
├── SKILL.md        # AI skill metadata & trigger description
├── selection.sh    # Main entry — arg parsing & orchestration
├── fetch.sh        # SellerSprite API calls & data processing
├── analyze.sh      # AI analysis & bilingual report rendering
└── README.md       # This file
```

## Requirements

- SellerSprite API Key with data access ([open.sellersprite.com](https://open.sellersprite.com))
- `curl` and `python3` (pre-installed on macOS/Linux)
- `openclaw` CLI ([openclaw.ai](https://openclaw.ai))

## Notes

- SellerSprite API rate limit: 40 requests/minute
- Each query retrieves up to TOP 2,000 results
- Single analysis consumes ~3,000–8,000 tokens (~$0.02–$0.05)
- Recommend querying the last 1–3 months for freshness

---

*Built with Claude Code + OpenClaw | 用 AI 构建，用 AI 发布*
