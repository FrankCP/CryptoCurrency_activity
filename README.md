## 🧠 Strategy Overview

### Core Logic
This strategy combines **Trend Following**, **Mean Reversion**, and **News-Based confirmation**.

The objective is to enter positions when price momentum and positive sentiment align, while exiting quickly when momentum weakens or risk increases. The strategy balances trade frequency and risk control by combining technical indicators with sentiment analysis.

---

### Entry Conditions (Buy)

A position is opened when **one of the following conditions** is satisfied:

#### 1. Trend Entry (Primary Mode)
- EMA(10) > EMA(30)
- RSI between **38 and 70**
- Sentiment = **POSITIVE**
- Sentiment Score ≥ **0.65**

This captures continuation moves during established uptrends.

#### 2. Oversold Bounce Entry (Secondary Mode)
- RSI ≤ **30**
- Sentiment = **POSITIVE**
- Sentiment Score ≥ **0.65**

This allows participation in short-term rebounds after oversold conditions.

---

### Exit Conditions (Sell)

A position is closed when **any** of the following occurs:

- **Stop Loss:** Loss reaches approximately **-5%**
- **Take Profit:** Profit reaches approximately **+8%**
- **Trailing Stop:** Price falls about **5%** from the highest level since entry
- **Negative Sentiment Exit:** Sentiment becomes NEGATIVE with score ≥ **0.70**
- **Time Stop:** Position held for **4 days** with profit < **+1.5%**

These exit rules prioritize capital preservation and allow faster capital rotation.

---

### Decision Flowchart (Mermaid)

```mermaid
graph TD
    Start[Market Data Input] --> Trend{EMA10 > EMA30?}

    Trend -->|Yes| RSITrend{38 <= RSI <= 70?}
    Trend -->|No| RSIBounce{RSI <= 30?}

    RSITrend -->|Yes| NewsOK{Sentiment POS & score >= 0.65?}
    RSITrend -->|No| Hold[Hold]

    RSIBounce -->|Yes| NewsOK
    RSIBounce -->|No| Hold

    NewsOK -->|Yes| Buy[Enter Position]
    NewsOK -->|No| Hold

    Buy --> Monitor[Holding]

    Monitor --> SL{PnL <= -5%?}
    SL -->|Yes| Sell[Exit Position]

    Monitor --> TP{PnL >= +8%?}
    TP -->|Yes| Sell

    Monitor --> Trail{Drop ~5% from peak?}
    Trail -->|Yes| Sell

    Monitor --> NegNews{Sentiment NEG & score >= 0.70?}
    NegNews -->|Yes| Sell

    Monitor --> TimeStop{Held >= 4 days & PnL < +1.5%?}
    TimeStop -->|Yes| Sell
