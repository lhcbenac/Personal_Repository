# 📈 Project : Intraday Trend Following & Walk-Forward Dynamic Allocation

> **Objective:** Engineer a robust, pure intraday trend-following portfolio across a diversified universe of 39 US equities. By strictly executing trades Open-to-Close, the strategy intentionally eliminates overnight gap risk—a major source of uncontrolled variance in algorithmic trading—and relies entirely on capturing intraday momentum.

---

## 📌 Project Overview

To identify these trends, I developed two competing quantitative models:

1. **Classical Statistics (ARIMA):** An Autoregressive Integrated Moving Average model to forecast next-day directional bias based on statistical memory.
2. **Alternative / Fractal Math (Hurst Exponent):** A Hurst Exponent regime filter paired with Exponential Moving Average (EMA) crossovers to detect and trade mathematical persistence (`H > 0.50`).

## ⚙️ The Engineering Process: Static vs. Dynamic Allocation

The initial iteration applied the ARIMA and Hurst models statically across all 39 assets. This resulted in negative expectancy; the models traded too frequently in mean-reverting regimes and bled capital to intraday noise.

To solve this, I implemented a **Walk-Forward Dynamic Allocation** architecture. Recognizing that alpha decays and market regimes constantly shift, I built a daily tournament engine. Rather than firing signals blindly, the algorithm calculates the hypothetical historical performance of the strategy for every stock. Capital is then dynamically routed *only* to the Top 5 historically performing assets. By starving the losers and feeding the winners, the system became self-correcting.

## 🔍 Parameter Sensitivity & The Grid Search

To ensure the strategy was not curve-fitted to a single lucky backtest, I subjected the models to a rigorous Parameter Sensitivity Analysis. I included my "Original" baseline settings alongside a grid of new constraints (Fast/Base/Slow for ARIMA, and Loose/Base/Strict for Hurst) to map a "Performance Cone" of expected returns.

### Key Takeaways from the Grid:
* **The Frequency vs. Selectivity Trade-off:** The `Hurst_Original` model (Lags: 2-20, `H > 0.52`) generated massive outperformance in absolute dollar terms (~$1,473 profit) with an impressive **1.22 Sharpe Ratio**. Conversely, the `Hurst_Strict` model (Lags: 5-30, `H > 0.62`) traded much less frequently, yielding lower absolute profit (~$483), but maintained the exact same 1.22 Sharpe and achieved the highest overall win rate (**55.07%**). This perfectly illustrates that demanding higher mathematical persistence improves accuracy, but sacrifices total trade volume.
* **Memory Filters Noise:** On the classical side, `ARIMA_Slow` (50-day lookback) dramatically outperformed the fast and baseline models, generating $1,601 in profit. This proved that a longer statistical memory is required to filter out the "chop" of daily intraday price action.
* **Benchmark Beating:** Both the optimized Classical and Alternative dynamic portfolios vastly outperformed the SPY Intraday benchmark.

## 🎲 Forward Risk Projection (Monte Carlo)

Because historical backtests only represent one realized path of probability, I extracted the daily return stream of the winning strategy and ran a 1,000-path Monte Carlo bootstrap simulation. 

This projected the strategy's behavior 252 trading days into the future, establishing a precise 5th Percentile (Worst Case) Confidence Interval. In a live deployment scenario, this absolute drawdown boundary serves as the system's "kill switch"—if the portfolio ever breaches this line, it indicates a structural regime breakdown, and the execution algorithms are immediately halted.

---

## 📊 Final Sensitivity Grid Results ($10,000 Initial Capital)

| Strategy | Total Profit ($) | Sharpe Ratio | Accuracy (%) |
| :--- | :--- | :--- | :--- |
| **ARIMA_Slow** | $1,601.51 | 0.75 | 50.63% |
| **Hurst_Original** | $1,473.82 | 1.22 | 53.91% |
| **Hurst_Strict** | $483.74 | 1.22 | 55.07% |
| **SPY Intraday (Benchmark)** | $458.90 | 0.23 | 52.69% |

---
<img width="1178" height="588" alt="image" src="https://github.com/user-attachments/assets/7d6dd2fb-be06-4e52-8030-d24d9caa0ec7" />

## 🛠️ Technologies Used

* **Python** (Core Logic)
* **Pandas / NumPy** (Vectorized Data Manipulation)
* **Statsmodels** (ARIMA Implementation)
* **SciPy** (Linear Regression & Statistical Functions)
* **yfinance** (Data Ingestion)
* **Matplotlib** (Financial Visualization)
