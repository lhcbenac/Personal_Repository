COLAB (https://colab.research.google.com/drive/1HdY7aWQfkyWvsfSUf7jnMePtK-4HjOm1?usp=sharing)
Mean-Reversion Spread Trading Quant Strategy
Overview
This notebook implements a simple spread trading mean-reversion strategy using major US stocks (e.g., AAPL, MSFT, GS) and Bollinger Bands over a 1-year backtest. It demonstrates the mechanics of pairwise spread, bands, and rule-based position management, including basic take-profit and stop-loss logic.

Important Caveat:
While the backtest results may appear positive, this code does not produce real alpha or constitute a robust market hedge strategy. The primary goal is exploratory—reflecting an approach to market understanding rather than actual investment merit.

What’s Included
Downloads data for selected tickers using yfinance

Calculates spread, returns, Bollinger Bands

Implements entry/exit rules (buy/sell at band extremes, close on reversion or TP/SL)

Tracks trade status and simulates PnL

Visualizes spread and returns

Limitations & Discussion
No true market neutrality or alpha generation: This backtest does not account for transaction costs, slippage, risk management, or portfolio construction, nor does it model market regimes or cross-hedging with other asset classes.

Exploratory context: The code demonstrates how to structure, test, and visualize such a strategy, serving as a learning tool for market dynamics and quant research.

Potential extensions:

Test with other spreads (e.g., sector pairs, ETFs)

Add portfolio/risk controls

Explore alternative bands, filters, and signal logic

Combine with market hedge approaches or factor analytics

How To Use
Clone the repo and install requirements:

bash
git clone <repo_URL>
pip install -r requirements.txt
Run the notebook in Jupyter or Colab.
You may edit the ticker list and parameter values directly in code.

Acknowledgements
Built for research, learning, and market exploration purposes.
This is not investment advice or a production trading system.


