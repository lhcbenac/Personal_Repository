# ⚖️ Project 3: Intraday Market Neutral & Volatility-Adjusted Gap Fading

> **Objective:** Engineer a zero-beta, pure alpha market-neutral portfolio. By strictly maintaining 100% Gross Exposure (50% Long / 50% Short) and executing purely intraday (Open-to-Close), this strategy eliminates both overnight gap risk and broader market directional risk.


---

## 📌 The Evolution: The Flaw of Raw Returns

The initial iteration of this Long/Short strategy relied on a basic mean-reversion premise: *Long yesterday's biggest percentage losers, Short yesterday's biggest percentage winners.* When subjected to a strict, realistic Intraday execution environment (buying the Open, selling the Close), this raw logic failed entirely, yielding a **-0.11 Sharpe Ratio**. 
* **The Problem:** A 2% drop in a low-volatility utility stock represents massive capitulation, while a 2% drop in a high-beta tech stock is standard daily noise. Ranking assets by raw percentages forces the algorithm to compare apples to oranges, frequently resulting in catching "falling knives."

## ⚙️ The Quantitative Framework: Volatility-Adjusted Z-Scores

To transform this into an institutional-grade model, I pivoted from raw returns to **Volatility-Adjusted Relative Extremes**, focusing specifically on human behavioral overreactions at the market open.

1. **The Overnight Gap:** The algorithm isolates the overnight price action `(Today's Open - Yesterday's Close)`. This gap captures the accumulation of after-hours news, retail panic, and pre-market euphoria.
2. **The Volatility Filter (Z-Score):** The raw gap is divided by the stock's 20-day historical standard deviation. This normalizes the data, converting a simple percentage into a statistical Z-Score. The algorithm now measures exactly how *unusual* a gap is relative to that specific stock's historical baseline.
3. **Cross-Sectional Market Making:** At exactly 9:30 AM, the algorithm ranks the 39-ticker universe by Gap Z-Score. 
    * **Long:** The Top 5 most severely suppressed gaps (Extreme negative Z-Score / Morning Panic).
    * **Short:** The Top 5 most severely inflated gaps (Extreme positive Z-Score / Morning Euphoria).

By acting as a liquidity provider during the chaotic opening minutes, the algorithm captures the intraday mean-reversion as the stock naturally gravitates back to its fair value by the 4:00 PM close.

## 📊 Results & Analytics ($10,000 Initial Capital)

The introduction of the Volatility-Adjusted Gap filter completely unlocked the strategy's alpha, transforming a negative-expectancy model into a highly robust equity curve that climbs entirely independent of the S&P 500 benchmark.

<img width="1156" height="577" alt="image" src="https://github.com/user-attachments/assets/dfd6376a-19f6-4b77-9b7c-4885e755b0bf" />

| Metric | Strategy Performance |
| :--- | :--- |
| **Final Portfolio Value** | $20,291.50 |
| **Absolute Profit** | +$10,291.50 |
| **Annualized Sharpe Ratio** | 1.47 |
| **Accuracy (Win Rate)** | 55.38% |
| **Net Market Exposure (Beta)** | 0.00 (Market Neutral) |

### Key Takeaways:
* **True Uncorrelated Alpha:** The S&P 500 benchmark experienced high volatility and chop over the backtested period, yet the Market Neutral strategy generated a smooth, 45-degree equity curve. Because the portfolio is always equally weighted Long and Short, it is immunized against broader market crashes.
* **Apples-to-Apples Ranking:** Normalizing data via historical standard deviations is a mandatory requirement for cross-sectional ranking. Fading statistical anomalies ($Z > 3.0$) provides a mathematical edge that fading arbitrary percentages cannot match.

---

## 🛠️ Technologies Used
* **Python** (Core Execution Logic)
* **Pandas / NumPy** (Vectorized Z-Score Normalization & Rolling Volatility)
* **yfinance** (Data Ingestion)
* **Matplotlib** (Equity Curve & Benchmark Visualization)
