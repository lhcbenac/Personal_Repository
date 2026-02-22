# 📉 Project : Intraday Mean Reversion & Cross-Sectional Fading

> **Objective:** Engineer a structural Reversal system to act as an uncorrelated alpha generator alongside the Project 1 Trend strategy. This system is designed to capitalize on intraday behavioral overreactions (panic selling and euphoric short-covering) by providing liquidity at the Open and flattening by the Close.

<img width="1023" height="451" alt="image" src="https://github.com/user-attachments/assets/e23e28e5-4305-44b4-896b-3f9df8e773a6" />

---
<img width="1023" height="451" alt="image" src="https://github.com/user-attachments/assets/b0ccaa51-7faf-4ab1-8da3-7af2cd4f79e8" />

---
<img width="1023" height="547" alt="image" src="https://github.com/user-attachments/assets/761234d5-aaf4-430e-9f94-efbc096dab4f" />


---

## 📌 The Quantitative Framework

Mean reversion requires a fundamentally different architecture than trend following. While trends persist over time (Time-Series), reversals are sudden, isolated snap-backs. I built two competing models to capture this phenomenon:

1. **Classical Statistics (Time-Series Stationarity):** Utilized the Augmented Dickey-Fuller (ADF) test to mathematically confirm a stock's mean-reverting regime (`p-value < 0.05`), paired with extreme Z-Score thresholds to fade overnight gaps.
2. **Alternative Approach (Cross-Sectional Magnitude):** Discarded individual historical performance in favor of a daily, universe-wide tournament. The algorithm ranks the daily absolute percentage returns of all 39 assets, isolating the most severe market overreactions, and strictly fades the extremes at the next day's Open.

## ⚙️ Parameter Sensitivity & The "Dark Zone"

To stress-test the validity of the models, I executed a Parameter Grid Search mapping out the "Performance Cone" across different constraints.

### The Concentration Premium in Mean Reversion
The Grid Search revealed a critical insight into market micro-structure: **Alpha in reversals is highly concentrated.** * When the Cross-Sectional model was constrained to only the **Top 3** most extreme movers (`Cross_Concentrated`), it generated a massive **1.30 Sharpe Ratio** and **$7,783.12** in absolute profit. 
* When the parameter was relaxed to trade the Top 10 movers (`Cross_Broad`), the edge heavily diluted, dropping the Sharpe to 0.32 and profit to $726.90. 

This proves mathematically that true mean-reverting "snap-backs" only occur at the absolute fringes of behavioral capitulation. Trading merely "large" moves instead of "extreme" moves introduces unnecessary variance and destroys positive expectancy.

### Statistical Rigidity vs. Relative Extremes
The Classical ADF models severely underperformed the Cross-Sectional models. Requiring strict mathematical stationarity and generic Z-Score deviations resulted in the algorithm missing the actual behavioral panic trades. The market reacts to relative extremes (the #1 biggest loser of the day), not isolated statistical boundaries.

## 🎲 Forward Risk Projection (Monte Carlo)

To project the future risk profile of the winning `Cross_Concentrated` strategy, a 1,000-path Monte Carlo bootstrap simulation was run on the daily return stream. Because cross-sectional fading relies on high win rates (53.36%) combined with Positive Skew (capturing massive intraday short-squeezes), the Monte Carlo fan chart displayed a highly aggressive upward trajectory. The 5th Percentile (Max Expected Drawdown) boundary was established to serve as a strict, automated kill-switch for live capital deployment.

---

## 📊 Final Sensitivity Grid Results ($10,000 Initial Capital)

| Strategy | Total Profit ($) | Sharpe Ratio | Accuracy (%) |
| :--- | :--- | :--- | :--- |
| **Cross_Concentrated (Top 3)** | $7,783.12 | 1.30 | 53.36% |
| **Cross_Base (Top 5)** | $2,862.66 | 0.75 | 49.89% |
| **Cross_Broad (Top 10)** | $726.90 | 0.32 | 50.11% |
| **ADF_Aggressive (Z > 1.0)** | $409.07 | 0.56 | 53.41% |
| **SPY Intraday (Benchmark)** | $458.90 | 0.23 | 52.69% |

---

## 🛠️ Technologies Used
* **Python** (Core Logic & Walk-Forward Routing)
* **Pandas / NumPy** (Vectorized Cross-Sectional Ranking)
* **Statsmodels** (Augmented Dickey-Fuller Implementation)
* **yfinance** (Data Ingestion)
* **Matplotlib** (Monte Carlo & Performance Zone Visualization)
