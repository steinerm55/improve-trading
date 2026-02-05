# improve-trading

I have two trading apps MetaTrader4 (Desktop) and TradingView (Web).

QA

Q: show me what ways of extensibility is available to both of them?
A:

### MetaTrader 4 (Desktop)

**Language:** MQL4 — a C++-like, object-oriented language built into the platform.

**Extension types:**
- **Expert Advisors (EAs)** — fully autonomous trading bots that analyze markets and execute trades
- **Custom Indicators** — technical analysis overlays and oscillators drawn on charts
- **Scripts** — one-shot programs (e.g., close all orders, export data to CSV)
- **Libraries / Include files (.mqh)** — reusable code modules shared across EAs/indicators/scripts

**APIs & integration:**
- `WebRequest()` — outbound HTTP GET/POST to external REST APIs (URLs must be whitelisted)
- **DLL imports** — load C/C++/C#/.NET DLLs, enabling WebSocket clients, database connectors, ML models, ZMQ sockets, and virtually anything that compiles to a Windows DLL
- **File I/O** — read/write CSV, TXT, and binary files for data exchange
- `SendMail()` / `SendNotification()` — built-in email and push alerts
- Community projects for Python integration (OTMql4Py) and REST/WebSocket middleware (MtApi, MetaApi)

**Development environment:**
- **MetaEditor** — bundled desktop IDE with syntax highlighting, auto-complete, debugger, profiler, and integrated compiler (.mq4 → .ex4)
- **Strategy Tester** — built-in backtesting engine with visual replay mode
- Can edit .mq4 source in any external editor (VS Code, etc.) but must compile via MetaEditor

**Community/marketplace:**
- MQL5.com Market — buy, sell, or rent EAs and indicators directly from the MT4 terminal
- MQL5.com Code Base — free open-source MQL4 programs
- Signals Service — publish/subscribe to copy trading signals
- Freelance Service — hire MQL4 developers

**File formats:** `.mq4` (source), `.ex4` (compiled binary), `.mqh` (headers), `.set` (presets), `.tpl` (chart templates)

---

### TradingView (Web)

**Language:** Pine Script (currently v6) — a domain-specific language purpose-built for financial analysis. Runs server-side in TradingView's cloud. Sandboxed: no network requests, no filesystem, no external code.

**Extension types:**
- **Indicators** — custom technical indicators (declared with `indicator()`)
- **Strategies** — backtestable trading strategies with built-in position management and performance reporting (declared with `strategy()`)
- **Libraries** — reusable Pine Script modules importable by other scripts (declared with `library()`)
- **Alerts** — conditional notifications that trigger webhooks, email, SMS, or push

**APIs & integration:**
- **Webhook alerts** — HTTP POST to a custom URL when an alert fires; payload supports dynamic placeholders like `{{ticker}}`, `{{close}}`, `{{strategy.order.action}}` (**requires paid plan**)
- `request.security()` — pull data from other symbols/timeframes
- `request.seed()` — import external datasets hosted on GitHub (2-5 min delay)
- **No outbound HTTP from Pine Script** — webhooks are the only external integration mechanism
- Third-party automation services (TradersPost, PineTrader, PickMyTrade, etc.) bridge webhooks to broker APIs for live execution
- Direct broker integrations (Interactive Brokers, Binance, Bybit, etc.) for order placement from the UI

**Development environment:**
- **Pine Editor** — browser-based editor with syntax highlighting, auto-complete, real-time errors, console logging (`log.info()`), and one-click chart attachment
- **Strategy Tester** — built-in backtesting with equity curves, trade lists, and performance metrics
- No offline development — code lives on TradingView's servers

**Community/marketplace:**
- 100,000+ community scripts at tradingview.com/scripts (open-source, protected, or invite-only)
- Pine Script libraries publishable and importable via `import`
- No built-in payment system — monetization requires external platforms (Gumroad, own website, etc.)

---

### Summary Comparison

| Capability | MetaTrader 4 | TradingView |
|---|---|---|
| Primary Language | MQL4 (C++-like, OOP) | Pine Script v6 (domain-specific) |
| External Language Support | Yes (C/C++/C#/Python via DLLs) | No |
| Autonomous Trading Bots | Yes (Expert Advisors) | No (webhooks + external service needed) |
| Custom Indicators | Yes | Yes |
| Backtesting | Yes (Strategy Tester) | Yes (built-in) |
| Outbound HTTP | Yes (`WebRequest()`) | Webhook alerts only (paid plans) |
| WebSocket Support | Via DLL | No |
| File I/O | Yes | No |
| IDE | MetaEditor (desktop) | Pine Editor (browser) |
| Platform | Windows (Wine/VPS for Mac/Linux) | Web browser (cross-platform) |
| Offline Development | Yes | No |

Q: what are the principles of volume-trading?
A:

Volume trading uses trading volume — the number of shares/contracts traded in a period — to gauge the conviction and participation behind price moves. Price tells you *what* happened; volume tells you *how many people agreed*.

### Core Principles

**1. Volume confirms trends**
- Rising price + rising volume = strong uptrend (buyers are committed)
- Rising price + falling volume = weakening uptrend (demand fading)
- Falling price + high volume = strong downtrend (heavy selling)
- Falling price + low volume = weak selling, possible bounce

**2. Volume precedes price**
Volume changes often appear *before* significant price moves. During consolidation, watch for volume building up — capital is entering before the breakout happens.

**3. Breakouts need volume**
A breakout without volume is a fakeout. High-volume breakouts (50%+ above average) signal institutional commitment and are more likely to sustain. Low-volume breakouts typically fail and revert.

**4. Climax volume signals exhaustion**
Abnormal volume spikes at the end of trends often mark turning points — capitulation (bottoms) or euphoria (tops). When one side becomes overly aggressive, the move stalls or reverses.

**5. Volume divergence warns of reversals**
- Price makes new highs but volume declines = uptrend losing strength
- Price makes new lows but volume declines = downtrend losing momentum
- Volume should expand with the trend; contraction is a red flag

### Key Volume Indicators

| Indicator | What It Does |
|---|---|
| **OBV (On-Balance Volume)** | Cumulative: adds volume on up-closes, subtracts on down-closes. Rising OBV + rising price = confirmation; divergence = warning |
| **VWAP (Volume-Weighted Average Price)** | Session average price weighted by volume. Institutional benchmark. Price above VWAP = bullish; below = bearish |
| **Volume Profile** | Horizontal histogram of volume by price level. High-Volume Nodes = support/resistance zones; Low-Volume Nodes = breakout zones |
| **A/D Line (Accumulation/Distribution)** | Measures money flow based on where price closes within its range. Rising A/D + rising price = accumulation |
| **MFI (Money Flow Index)** | Volume-weighted RSI. MFI > 80 = overbought; MFI < 20 = oversold. Divergences signal trend weakness |
| **CMF (Chaikin Money Flow)** | Buying/selling pressure over a period. CMF > 0 = buying dominates; CMF < 0 = selling dominates |

### Practical Application

**Entries:**
- Breakout + volume surge (50%+ above average) = go
- Bounce off support/resistance with above-average volume = confirmed level
- Climax volume at extremes + divergence in OBV/MFI = reversal setup

**Exits:**
- Volume fading as the move extends = stall warning
- Climax volume spike after an extended run = take profits
- Price stretching far from VWAP = reversion likely

**Rules of thumb:**
1. Volume should expand in the direction of the trend — if it doesn't, be cautious
2. Low-volume rallies or declines are suspect — wait for confirmation
3. Always combine volume with price structure (support/resistance, trendlines) — volume confirms, it doesn't dictate

Q: what user interface is volume-trading using?

A:

Volume trading doesn't have a single dedicated UI — it's done through chart-based trading platforms that display volume data alongside price. Here's how it looks in your two platforms:

### TradingView (Web)

- **Volume bars** at the bottom of every chart (built-in, enabled by default)
- **Volume Profile** — horizontal histogram overlay showing volume by price level (available as a built-in indicator or via community scripts)
- **VWAP** — a line overlaid on the price chart (`indicator: VWAP`)
- **OBV, MFI, CMF, A/D** — all available as built-in indicators added from the Indicators menu
- **Heatmap / Volume Footprint** — available through community Pine Script indicators or TradingView's built-in Volume Footprint chart type (requires Premium+)
- **DOM (Depth of Market)** — order book panel showing bid/ask volume at each price level (available with supported brokers)
- **Alerts** — set volume-based conditions (e.g., "volume crosses above 2x 50-period average") to trigger notifications

### MetaTrader 4 (Desktop)

- **Volume bars** at the bottom of the chart (tick volume by default — MT4 shows tick volume, not real exchange volume, since forex is OTC)
- **Built-in indicators**: OBV, MFI, A/D, Volumes, Money Flow
- **Custom indicators via MQL4** — Volume Profile, VWAP, footprint charts, and delta volume can be added through custom or marketplace indicators
- **Market Depth window** — shows Level 2 order book data (if supported by the broker)
- **Strategy Tester** — backtest volume-based EAs with historical tick data

### Typical Volume Trader's Screen Layout

A volume trader's workspace commonly includes:

1. **Main price chart** with candlesticks + volume bars below
2. **Volume Profile** overlay (or separate indicator pane) showing volume distribution by price
3. **VWAP line** on the price chart as dynamic support/resistance
4. **OBV or A/D** in a sub-pane to track cumulative money flow
5. **MFI or CMF** in another sub-pane for overbought/oversold signals
6. **DOM / Order Book** panel (if available) for real-time bid/ask volume

### Important Note on MT4 Volume

MT4 displays **tick volume** (number of price changes), not **real volume** (actual contracts traded). This is because forex is decentralized (OTC) — there's no single exchange reporting true volume. Tick volume is generally considered a reasonable proxy, but it's less reliable than real exchange volume available on stocks, futures, or crypto. TradingView shows real volume for exchange-traded instruments and tick volume for forex.

Q: what is tick volume vs. real volume?
A:

### Real Volume (Exchange Volume)

The actual number of shares, contracts, or coins traded in a time period. Reported directly by the exchange where the trades occur.

- **Available for:** stocks, futures, commodities, crypto (anything traded on a centralized exchange)
- **Example:** 1.5 million shares of AAPL traded in a 5-minute candle
- **Source:** the exchange's trade matching engine — every filled order is counted
- **Reliability:** high — it's a direct, auditable count of real transactions

### Tick Volume

The number of price changes (ticks) in a time period. Each time the bid/ask price updates, that's one tick — regardless of how many contracts were behind that price change.

- **Available for:** forex, CFDs, and other OTC (over-the-counter) instruments with no central exchange
- **Example:** 3,200 ticks in a 5-minute candle on EUR/USD
- **Source:** your broker's price feed — different brokers may report different tick counts
- **Reliability:** moderate — it's a proxy, not a direct measure of actual money changing hands

### Why Forex Has No Real Volume

Forex is decentralized — trades happen across thousands of banks, brokers, and liquidity providers worldwide. There's no single exchange aggregating all trades, so nobody knows the true total volume. Each broker only sees its own order flow.

### How They Compare

| Aspect | Real Volume | Tick Volume |
|---|---|---|
| Measures | Actual units traded | Number of price changes |
| Source | Centralized exchange | Broker's price feed |
| Consistency | Same across all brokers | Varies by broker |
| Accuracy | Exact | Approximate proxy |
| Where used | Stocks, futures, crypto | Forex, CFDs |
| Size information | Yes (100 shares ≠ 10,000 shares) | No (a 1-lot tick = a 100-lot tick) |

### The Key Limitation of Tick Volume

Tick volume doesn't know *how much* was traded — only that *something* moved. A single tick could be a 0.01-lot retail order or a 500-lot institutional block. Real volume captures that difference; tick volume treats them identically.

### Does Tick Volume Still Work?

Research (including a well-known study by Caspar Marney) shows **tick volume correlates ~90% with real volume** in liquid forex pairs. The reasoning: more real trading activity creates more price updates. So while it's not exact, the volume principles (confirms trends, precedes price, validates breakouts) still broadly apply to tick volume — just with less precision.

### In Your Platforms

- **MT4**: shows tick volume by default for forex pairs. No way to get real forex volume (it doesn't exist centrally). For CFDs on stocks/futures, it's still tick volume from your broker.
- **TradingView**: shows real volume for exchange-traded instruments (stocks, futures, crypto). Shows tick volume for forex. The chart label typically indicates which type is displayed.
