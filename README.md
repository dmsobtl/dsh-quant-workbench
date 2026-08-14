# dsh-quant-workbench

基于 DeepSeek Harness 的量化研究工作台 — A 股、美股、加密货币一站式分析。

## 快速开始

```bash
git clone https://github.com/dmsobtl/dsh-quant-workbench.git
cd dsh-quant-workbench && npm install
export DEEPSEEK_API_KEY=your-key
dsh --profile .
```

然后直接对话：

```
> 看看贵州茅台最近的走势，RSI 是否超买？
> BTC 当前什么位置？用双均线策略回测最近半年
> 帮我筛选今天 A 股放量突破 20 日均线的股票
> 用 MACD 金叉策略回测 AAPL 一年，10 万美元本金
```

## 包含插件

| 插件 | 工具 | 用途 |
|------|------|------|
| dsh-tool-market-data | `market_quote` | 实时行情（A股/美股/币） |
| | `market_kline` | 历史 K 线 |
| | `market_screen` | 条件选股 |
| | `market_fundamentals` | 基本面数据 |
| dsh-tool-backtest | `run_backtest` | 策略回测 |
| | `calc_indicator` | 技术指标计算 |
| dsh-llm-vision-router | — | K 线图理解（按需） |
| dsh-session-analyst | `analyze_session` | 研究效率分析 |

## 预置 Skill

| Skill | 功能 |
|-------|------|
| momentum-scan | 动量选股：放量突破 + RSI 确认 |
| strategy-backtest | 策略回测标准流程 |
| crypto-analysis | 加密货币多维分析框架 |

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

回测结果：
- 年化收益：12.3%
- 最大回撤：8.7%
- 夏普比率：1.45
- 胜率：62%
- 共交易 23 次
```

### 加密货币

```
User: BTC 技术面怎么看？

Agent:
→ market_quote({ symbol: "BTC", market: "crypto" })
→ market_kline({ symbol: "BTC", period: "1d", limit: 60 })
→ calc_indicator({ prices: [...], indicator: "rsi" })
→ calc_indicator({ prices: [...], indicator: "macd" })

BTC 当前 $67,230，RSI(14) = 58（中性偏多），
MACD 在零轴上方但柱状缩短，短期可能进入震荡...
```

## 数据源说明

| 市场 | 行情源 | 费用 | 限制 |
|------|--------|------|------|
| A 股 | 新浪/东方财富 | 免费 | 实时有 15 分钟延迟 |
| 美股 | Yahoo Finance | 免费 | 实时延迟 15 分钟 |
| 加密 | Binance | 免费 | 实时，无延迟 |

## License

MIT — 本工具仅用于研究学习，不构成投资建议。
