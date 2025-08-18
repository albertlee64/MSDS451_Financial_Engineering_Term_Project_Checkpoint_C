# MSDS451_Financial_Engineering_Term_Project_Checkpoint_C

Multi-Asset Strategy Backtesting (1999–2024)

Walk-forward, fees, crisis analytics, and Monte Carlo (expected ROI after fees)

This project evaluates a transparent, rules-based long-only multi-asset strategy across 1999–2024. It includes:

Real-data walk-forward backtests with monthly rebalancing

Optimization (Max Sharpe, Min Variance, Risk Parity, Max Return) with realistic constraints

Fee modeling: management fees, trading costs, and 20% performance fee on alpha vs SPY

Benchmarking vs S&P 500 (SPY)

Crisis / stress testing (Dot-com, GFC, COVID, 2022)

Monte Carlo (stationary bootstrap) percentiles and ending-balance histogram in dollars

Reproducible EDA, plots, and tables suitable for an investor deck

Education / research code only, not investment advice.

# 1) Quick start

Environment

Python ≥ 3.10

Recommended packages:

pandas, numpy, scipy, scikit-learn (LedoitWolf), matplotlib

statsmodels (optional; pairs trading & diagnostics)

yfinance (or your data loader)

# 2) Run

Pull or load monthly prices for the 15-asset universe

Run EDA and summary tables

Construct signals (momentum + mean-reversion)

Optimize and backtest with fees/costs

Benchmark vs SPY and compute metrics

Create crisis plots/tables

Run Monte Carlo percentiles + ending-balance histogram (in dollars)

Save all tables/plots to outputs/

# 3) Data & Universe

Tickers (15):
AAPL, MSFT, JPM, HON, NOC, JNJ, PG, SPY, IWM, QQQ, ACWI, TLT, GLD, VNQ, UUP

<img width="468" height="257" alt="image" src="https://github.com/user-attachments/assets/a07c97f5-1a9b-461d-873d-e0912d2e31ac" />

Dynamic universe: assets join when they actually exist (no look-ahead).

Frequency: monthly (using month-end closes).

<img width="468" height="319" alt="image" src="https://github.com/user-attachments/assets/83cb1986-2562-45c7-92d5-a5f368155e60" />

<img width="468" height="185" alt="image" src="https://github.com/user-attachments/assets/3acae938-7a2a-456b-a500-04750fc93569" />

# 4) Models & Signals

Signals

Momentum: 12-month price momentum (lagged).

Mean reversion: z-score of price vs 6-month mean/std (inverted to favor reversion).

Signals are blended into a tilt on expected returns prior to optimization.

Portfolio construction (long-only)

Max Sharpe, Min Variance, Risk Parity (inverse-vol), Max Return

Ledoit–Wolf covariance shrinkage when feasible

Realism constraints

Caps: UUP / TLT / GLD ≤ 1% each

Floors: small minimum weight for every live asset each rebalance (e.g., 0.5% default; 0.1% for capped trio)

Rebalancing: monthly (configurable)

Baselines

BENCH: SPY (S&P 500 ETF)

60_40: 60% SPY / 40% TLT (example classic)

EW_LIVE: Equal weight across all currently live assets each rebalance

<img width="468" height="162" alt="image" src="https://github.com/user-attachments/assets/fcf4b6d6-ccc3-44fa-987f-5e7f27f9a974" />

# 5) Backtesting & Fees

Walk-forward

At each rebalance:

Estimate μ/Σ from a fixed lookback window

Apply signals → tilt expected returns

Solve constrained optimization

Apply fees & trading costs

Compound to next period

Fees & costs (all included in net results)

Management fee: e.g., 0–4%/yr (applied monthly)

Trading costs: turnover × bps per trade (monthly rebalance)

Performance fee: 20% on alpha vs SPY, annual crystallization

# 6) Crisis / Stress Testing

Windows (configurable in the notebook):

Dot-com: 2000-03-01 → 2002-10-31

GFC: 2007-10-01 → 2009-03-31

COVID shock: 2020-02-01 → 2020-04-30

2022 drawdown: 2022-01-01 → 2022-10-31

Artifacts

<img width="468" height="185" alt="image" src="https://github.com/user-attachments/assets/70a80091-213d-46f2-82fe-cbf7ae18e740" />

Table (per period): Months, CAGR, Total Return, AnnVol, Sharpe, MaxDD, Worst Month, Terminal $
→ outputs/crisis_metrics_best_vs_spy.csv

# 7) Monte Carlo (Expected ROI after all fees)

Method: Stationary bootstrap of the best strategy’s net monthly returns to simulate 25 years (300 months). We produce:

Percentile wealth paths in dollars (5th/25th/50th/75th/95th) from a $1,000,000 start
→ mc_percentile_paths_dollar_lines_<BEST>.png
→ mc_percentile_paths_dollars_<BEST>.csv

Ending-balance histogram (dollars) with markers for mean, 5th, 95th percentiles

<img width="468" height="185" alt="image" src="https://github.com/user-attachments/assets/2e8e84b7-8fe3-41f0-8fda-9985843ecb7a" />

A summary table (inline) at 5/10/15/20/25 years in dollars

Extension (optional): multivariate block-bootstrap of asset returns with re-optimization along each path (closer to a full market simulation).

# 8) Metrics (How to read)

We report, for each portfolio:

CAGR — compound annual growth

AnnVol — annualized volatility

Sharpe — return per unit of risk

Alpha (ann) — annual alpha vs SPY (adjusted for beta)

Beta — sensitivity to SPY

MaxDD — maximum drawdown

Calmar — CAGR / |MaxDD|

See the “Backtesting: Best Strategy vs S&P 500 (Net)” table and the example provided in your notebook run.

# AI Usage

Parts of this repository (research framing, code scaffolding, doc text, and refactoring) were assisted by an AI coding assistant. All outputs were reviewed and edited by human maintainers.  

# MIT License

MIT License


