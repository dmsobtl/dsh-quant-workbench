# dsh-quant-workbench

基于 DeepSeek Harness 的量化研究工作台 —— A 股、美股、加密货币一站式分析。

## 快速开始

> 前提：依赖的插件 `dsh-tool-market-data` / `dsh-tool-backtest` / `dsh-llm-vision-router` /
> `dsh-session-analyst` 需要先发布到 npm（`npm publish`），否则 `pnpm install` 拉不到。

```bash
# 1. 安装 dsh
npm install -g @deepseek-ai/dsh

# 2. 把本仓库克隆成名为 quant 的 profile（profile 必须放在 ~/.dsh/profiles/<name>/ 下）
mkdir -p ~/.dsh/profiles
git clone https://github.com/dmsobtl/dsh-quant-workbench.git ~/.dsh/profiles/quant
cd ~/.dsh/profiles/quant

# 3. 安装插件依赖
pnpm install
# 或逐个：dsh plugin add dsh-tool-market-data dsh-tool-backtest dsh-llm-vision-router dsh-session-analyst

# 4. 配置 API Key
export DEEPSEEK_API_KEY=your-deepseek-key
export OPENAI_API_KEY=your-openai-key   # 可选，用于 K 线图/收益曲线的视觉理解

# 5. 启动（注意：profile 是名字，不是路径）
dsh --profile quant "看看贵州茅台最近的走势，RSI 是否超买？"
```

## 包含插件

| 插件 | 工具 | 用途 |
|------|------|------|
| dsh-tool-market-data | `market_quote` / `market_kline` / `market_screen` / `market_fundamentals` | 行情、K 线、选股、基本面 |
| dsh-tool-backtest | `run_backtest` / `calc_indicator` | 策略回测、技术指标 |
| dsh-llm-vision-router | — | 有图片时自动切视觉模型 |
| dsh-session-analyst | `analyze_session` / `compare_sessions` | 研究效率分析 |

## 预置 Skill（位于 `.agents/skills/`，DSH 自动发现）

| Skill | 功能 |
|-------|------|
| momentum-scan | 动量选股：放量突破 + RSI 确认 |
| strategy-backtest | 策略回测标准流程 |
| crypto-analysis | 加密货币多维分析框架 |

## 目录结构

```
dsh-quant-workbench/
├── package.json          # dsh.profile.bundles: dsh-base + dsh-headless
├── cordis.patch.yml      # 插入业务插件 + 覆盖 system-prompt 人格
├── pnpm-workspace.yaml
├── .agents/skills/       # 项目级 skill
│   ├── momentum-scan.md
│   ├── strategy-backtest.md
│   └── crypto-analysis.md
└── README.md
```

## 示例对话

### 行情查询

```
User: 茅台现在多少钱？
Agent: → market_quote({ symbol: "600519" })
贵州茅台 (600519) 当前价格 ¥1,856.00，涨幅 +1.23%...
```

### 策略回测

```
User: 用 5 日线和 20 日线金叉策略回测沪深 300ETF

Agent:
→ market_kline({ symbol: "510300", period: "1d", limit: 250 })
→ calc_indicator({ prices: [...], indicator: "sma", period: 5 })
→ calc_indicator({ prices: [...], indicator: "sma", period: 20 })
（生成交叉信号）
→ run_backtest({ strategyName: "MA5/20 金叉", bars: [...], signals: [...] })
```

### 加密货币

```
User: BTC 技术面怎么看？

Agent:
→ market_quote({ symbol: "BTC", market: "crypto" })
→ market_kline({ symbol: "BTC", period: "1d", limit: 60 })
→ calc_indicator({ prices: [...], indicator: "rsi" })
→ calc_indicator({ prices: [...], indicator: "macd" })
```

## 数据源说明

| 市场 | 行情源 | 费用 | 限制 |
|------|--------|------|------|
| A 股 | 新浪/东方财富 | 免费 | 免费接口可能有 3-5 秒延迟 |
| 美股 | Yahoo Finance | 免费 | 实时延迟 15 分钟 |
| 加密 | Binance | 免费 | 实时，无延迟 |

## License

MIT —— 本工具仅用于研究学习，不构成投资建议。
