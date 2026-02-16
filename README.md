# Team 02 - Crypto Trading Strategy

## 👥 Members
Kazanfаrov Timur Ruslanovich
Karpova Valeria Igorevna
Novolaeva Eva
Yusupov Khamzat Maazovich

## 🧠 Strategy Overview

### Core Logic
Our strategy focuses on **Mean Reversion + News-Based** approach combining RSI technical indicator with AI-powered news sentiment analysis. The key insight is that crypto markets tend to overreact to news, creating short-term mispricings that we can exploit.

**No HOLD allowed** — every signal results in either a BUY or SELL decision.

**Entry Condition (Buy):**
- RSI < 33 AND Sentiment is NOT NEGATIVE (oversold market)
- OR Sentiment POSITIVE with Score > 0.78 AND RSI < 58 (positive news momentum)

**Exit Condition (Sell):**
- Take-Profit triggered at +4% gain
- Stop-Loss triggered at -2% loss
- RSI > 68 (overbought market)
- Sentiment NEGATIVE with Score > 0.72 (negative news signal)
- Default hedge: exit position if no clear signal

**Risk Management:**
- Stop-Loss: -2% per position (limits downside)
- Take-Profit: +4% per position (locks in gains)
- Portfolio: Capital equally distributed across 9 crypto tickers
- Initial Capital: $10,000 total ($1,111 per ticker)

### Decision Flowchart
```mermaid
graph TD
    Start[Market Data Input: Price + RSI + News] --> CheckTP{In Position AND PnL >= 4%?}
    
    CheckTP -->|Yes| SellTP[SELL: Take-Profit +4%]
    CheckTP -->|No| CheckSL{In Position AND PnL <= -2%?}
    
    CheckSL -->|Yes| SellSL[SELL: Stop-Loss -2%]
    CheckSL -->|No| CheckRSIHigh{RSI > 68?}
    
    CheckRSIHigh -->|Yes| SellRSI[SELL: Overbought]
    CheckRSIHigh -->|No| CheckNeg{Sentiment NEGATIVE AND Score > 0.72?}
    
    CheckNeg -->|Yes| SellNeg[SELL: Negative News]
    CheckNeg -->|No| CheckRSILow{RSI < 33 AND NOT Negative?}
    
    CheckRSILow -->|Yes| BuyRSI[BUY: Oversold RSI]
    CheckRSILow -->|No| CheckPos{Sentiment POSITIVE AND Score > 0.78 AND RSI < 58?}
    
    CheckPos -->|Yes| BuyPos[BUY: Positive News + RSI OK]
    CheckPos -->|No| Default{In Position?}
    
    Default -->|Yes| SellDefault[SELL: Default Hedge]
    Default -->|No| BuyDefault[BUY: Default Enter]

    style SellTP fill:#f8d7da,stroke:#dc3545,stroke-width:2px
    style SellSL fill:#f8d7da,stroke:#dc3545,stroke-width:2px
    style SellRSI fill:#f8d7da,stroke:#dc3545,stroke-width:2px
    style SellNeg fill:#f8d7da,stroke:#dc3545,stroke-width:2px
    style SellDefault fill:#f8d7da,stroke:#dc3545,stroke-width:2px
    style BuyRSI fill:#d4edda,stroke:#28a745,stroke-width:2px
    style BuyPos fill:#d4edda,stroke:#28a745,stroke-width:2px
    style BuyDefault fill:#d4edda,stroke:#28a745,stroke-width:2px




📊 Performance Analysis

Sharpe Ratio: 0.79 (Most Important!)
Total Return: +3.89%
Max Drawdown: -9.53%
Win Rate: 49.07%
Profit Factor: 1.19
Total Trades: 108
Final Balance: $10,388.87

Comparison vs Baseline
MetricBaseline StrategyOur StrategySharpe Ratio-0.92+0.79Total Return-9.16%+3.89%Max Drawdown-23.71%-9.53%Final Balance$9,083.51$10,388.87

💪 Strengths

Strong risk management — Stop-Loss at -2% prevents large losses and keeps Max Drawdown low at -9.53%
Sentiment-driven exits — Selling on negative news (score > 0.72) allows early exit before price drops
No HOLD logic — Every timestep makes an active decision, enabling hedging instead of passive waiting
Significantly outperforms baseline — Sharpe Ratio improved from -0.92 to +0.79

⚠️ Limitations & Learnings

Default trades add noise — When no clear signal exists, default buy/sell creates some low-quality trades
Single sentiment source — All 9 tickers share the same daily news, limiting per-asset precision
Crypto volatility — High market volatility during the 90-day period made consistent gains difficult
RSI thresholds — Fixed RSI thresholds (33/68) may not be optimal across all market conditions
