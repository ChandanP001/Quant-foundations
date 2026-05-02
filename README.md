# Quant Foundations

A self-directed quantitative finance research portfolio built alongside my BTech 
at IIT Patna. Each project translates mathematical theory directly into working 
Python code using real NSE/Nifty 50 market data.

**Study path:** Probability Theory → Stochastic Calculus → Time Series → 
Algorithmic Trading.

**Course foundation:** NPTEL — Probability and Stochastics for Finance 
(IIT Kanpur, Prof. Joydeep Dutta)

---

## Projects

### 01 — Chebyshev Inequality Simulation
**Concept:** Chebyshev's inequality states P(|X−μ| ≥ kσ) ≤ 1/k² for any 
distribution — not just normal.

Verified empirically across 100,000 samples of an exponential distribution. 
Exponential was chosen deliberately because it is highly non-normal — proving 
the universality of the bound.

`numpy` `matplotlib` `probability theory`

---

### 02 — Law of Large Numbers Simulation
**Concept:** The sample mean converges to the true mean as n → ∞. 
Variance of sample mean = σ²/n — more data means more certainty.

Simulated 10,000 Bernoulli coin flips tracking the running average. 
20 independent simulations overlaid — all converge to p=0.5. 
Direct connection to why backtests need large sample sizes.

`numpy` `matplotlib` `probability theory`

---

### 03 — Stationarity Analysis on Nifty 50
**Concept:** A time series is stationary if its mean and variance are 
constant over time. Raw stock prices are not stationary — returns are.

Applied Augmented Dickey-Fuller test to 5 years of real Nifty 50 data:
- Prices: ADF p-value = 0.9398 → not stationary
- Returns: ADF p-value = 0.0000 → stationary

COVID volatility clustering clearly visible. Foundation for all time 
series modelling.

`yfinance` `statsmodels` `pandas` `matplotlib`

---

### 04 — ACF and PACF Analysis on Nifty 50
**Concept:** ACF measures total correlation between current and past values. 
PACF isolates the direct effect only — removing indirect lag effects.

Computed 40 lags on Nifty 50 daily returns. Confidence bands ±0.0559.

Key finding: Statistically significant spikes at lags 5, 6, 7 — confirming 
a weekly trading cycle (5 trading days = 1 week). Most lags insignificant — 
consistent with the Efficient Market Hypothesis.

`statsmodels` `pandas` `matplotlib`

---

### 05 — Brownian Motion and GBM Simulator
**Concept:** Brownian motion is a continuous-time stochastic process with 
independent N(0, dt) increments. Stock prices follow Geometric Brownian Motion 
(GBM) — the exponential of BM — to ensure non-negativity.

Five simulations built:
- Single BM path — W(0)=0, dW ~ N(0,dt)
- 20 BM paths — shows uncertainty growing as √t
- Quadratic variation verification — 1000 paths, mean QV = 1.0001 vs 
  theoretical T = 1.0 (empirical proof of [W,W]_T = T)
- GBM simulation — S(t) = S₀exp((μ−σ²/2)t + σW(t)), 50 paths
- GBM vs real Nifty 50 — parameters μ=18.8%, σ=9.8% estimated from 
  2023 data, 30 simulated paths overlaid against real market trajectory

The −σ²/2 correction term in GBM comes from Itô's Lemma — ordinary 
calculus would give μ, stochastic calculus corrects it to μ−σ²/2.

`numpy` `matplotlib` `yfinance` `stochastic calculus`

---

### 06 — ARIMA Forecasting on Nifty 50
**Concept:** ARIMA(p,d,q) combines autoregression (AR), differencing (I) 
to enforce stationarity, and moving average (MA) of past errors.

Pipeline:
- ADF test confirmed d=1 (prices non-stationary, returns stationary)
- PACF identified significant weekly cycle at lags 5 and 6 → p=5
- ARIMA(5,0,0) fitted on 1201 training days (Jan 2019 — Nov 2023)
- Only AR lags 1 (p=0.004) and 5 (p=0.000) statistically significant

30-day out-of-sample forecast:
- RMSE: 0.006806
- MAE: 0.005001
- Actual cumulative return: +10.5% (Nov-Dec 2023 bull run)
- Forecasted cumulative return: +2.0%

ARIMA captures autocorrelation structure — not directional trend. 
Conservative forecast during a strong bull run is expected behaviour, 
not model failure.

`statsmodels` `yfinance` `pandas` `sklearn`

---

### 07A — Momentum Strategy Backtester — Single Asset
**Concept:** Momentum strategy — assets that have performed well recently 
tend to continue performing well in the short term.

Strategy: 20-day momentum signal on Nifty 50. Long when momentum > 0, 
flat when momentum < 0. Signal shifted by 1 day to eliminate lookahead bias. 
Transaction cost = 0.1% per trade.

Results (2018—2024, 1456 trading days):

| Metric | Strategy | Buy and Hold |
|--------|----------|--------------|
| Total Return | 79.72% | 97.06% |
| Annualised Return | 10.68% | 12.46% |
| Sharpe Ratio | **0.441** | 0.406 |
| Max Drawdown | **−15.93%** | −38.44% |
| Days in Market | 62.2% | 100% |

Higher Sharpe ratio and dramatically lower drawdown. Strategy stepped 
out of market before the COVID crash of 2020 — reducing max drawdown 
from 38% to 16%.

`pandas` `numpy` `matplotlib` `yfinance`

---

### 07B — Momentum Strategy Backtester — Multi Asset
**Concept:** Cross-sectional momentum — rank assets by recent performance 
and rotate into the strongest relative performers.

Strategy: Daily ranking of 10 large-cap Nifty 50 stocks by 20-day momentum. 
Hold top 3 with equal weight. Rebalance daily. Transaction cost = 0.1% 
per stock change.

Universe: RELIANCE, TCS, HDFCBANK, INFY, ICICIBANK, HINDUNILVR, 
ITC, SBIN, BAJFINANCE, AXISBANK

Results (2018—2024):

| Metric | Portfolio | Nifty 50 |
|--------|-----------|----------|
| Total Return | 90.63% | 96.67% |
| Annualised Return | 11.81% | 12.41% |
| Annualised Volatility | 22.53% | 18.32% |
| Sharpe Ratio | 0.342 | 0.404 |
| Max Drawdown | −34.61% | −38.44% |

Concentration in 3 stocks increases volatility without proportional 
return improvement. Single-asset momentum (7A) superior for capital 
protection. Multi-asset momentum outperforms visually post-2021.

`pandas` `numpy` `matplotlib` `yfinance`

---

## Skills Demonstrated

**Mathematics:** Probability theory, measure theory (introductory), 
martingales, Brownian motion, Itô calculus, stochastic differential equations

**Statistics:** Stationarity testing, autocorrelation analysis, 
ARIMA modelling, hypothesis testing

**Finance:** Time series modelling, momentum strategies, backtesting 
methodology, Sharpe ratio, maximum drawdown, transaction cost modelling, 
lookahead bias prevention

**Python:** NumPy, Pandas, Matplotlib, Statsmodels, yfinance, Scikit-learn

---
