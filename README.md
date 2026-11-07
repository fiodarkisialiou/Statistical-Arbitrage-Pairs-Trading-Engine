# Statistical-Arbitrage-Pairs-Trading-Engine
A fully vectorized Python backtesting engine that identifies mean-reverting equity pairs and executes a statistical arbitrage strategy based on rolling Z-score thresholds.

## Overview
This project implements a quantitative pairs trading strategy from scratch. It scans a universe of equities to find statistically linked pairs, generates long/short trade signals using localized standard deviation breaches, and calculates historical P&L with institutional-grade risk metrics.

## Core Features
* **Statistical Cointegration:** Utilizes the Engle-Granger test (`statsmodels`) to identify stationary relationships between asset pairs.
* **Dynamic Spread Standardization:** Calculates a 21-day rolling mean and standard deviation to generate responsive, time-localized Z-scores.
* **Vectorized Signal Generation:** Uses Pandas boolean masking to generate entry (±2 std dev) and exit (mean reversion) signals, avoiding slow `for` loops.
* **Rigorous Backtesting:** Implements `.shift(1)` logic to entirely eliminate look-ahead bias in the daily returns calculation.
* **Risk Metrics:** Outputs quantitative desk-standard performance metrics, including Annualized Sharpe Ratio and Maximum Drawdown.

## Methodology
1. **Universe Selection:** Downloads historical adjusted close data via `yfinance`.
2. **Hypothesis Testing:** Iterates through pairs to find p-values < 0.05, confirming cointegration.
3. **The Trigger Engine:** 
   * **Long:** Z-score drops below -2.0
   * **Short:** Z-score spikes above +2.0
   * **Flat (Exit):** Z-score reverts to 0 (±0.5)
4. **P&L Calculation:** Multiplies the daily spread return by the lagged signal vector to simulate real-world trading equity curves.

## Tech Stack
* **Python 3.x**
* **Pandas & NumPy** (Vectorized time-series analysis)
* **Statsmodels** (ADF / Cointegration testing)
* **yfinance** (Data ingestion)
* **Matplotlib** (Visualization)
