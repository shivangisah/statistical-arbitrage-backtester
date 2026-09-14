Statistical Arbitrage Backtester

A Python-based statistical arbitrage research and backtesting project
that tests whether a cointegration-based mean-reversion signal can
survive chronological validation and out-of-sample evaluation.

The project screens 190 U.S. equity pairs, estimates an OLS hedge
relationship, constructs residual spreads, applies stationarity
diagnostics, generates stateful Z-score trading signals, and evaluates
performance after an execution lag and transaction costs.

Main finding: TXN--GS produced a positive 2024 validation result,
but the same frozen specification deteriorated in the 2025
out-of-sample period. The project therefore demonstrates backtesting
discipline and model-risk diagnosis rather than claiming a profitable
strategy.

Research Design

Stage           Period       Purpose

Training        2021--2023   Pair screening and parameter estimation
Validation      2024         Holdout evaluation
Out-of-Sample   2025         Final unseen test

The final universe contains 20 liquid U.S. equities, creating 190
possible pairs.

Methodology

The research pipeline uses:

Engle--Granger cointegration screening across all candidate
pairs using training data only

OLS regression to estimate the hedge relationship

Residual spread construction for the selected pair

Augmented Dickey--Fuller (ADF) diagnostic on the residual spread

Mean-reversion half-life estimation

Benjamini--Hochberg FDR correction as a multiple-testing
diagnostic

Rolling Z-score signals

One-day execution lag

Hedge-adjusted spread P&L

Turnover-based transaction costs

Chronological validation and out-of-sample testing

Selected Pair: TXN--GS

Training-period estimates:

Parameter                              Value

Engle--Granger p-value              0.000993
Residual ADF p-value                0.000160
OLS Alpha                            86.4514
OLS Beta                              0.2200
Estimated Half-Life        12.3 trading days
Z-score Lookback             12 trading days

No candidate pair survived the 5% Benjamini--Hochberg FDR
correction. TXN--GS is therefore treated as an exploratory
candidate, not as statistically robust evidence of cointegration after
multiple-testing adjustment.

Trading Rules

The final specification was frozen before the 2025 test:

Long spread: Z-score < -2.0

Short spread: Z-score > +2.0

Exit: |Z-score| <= 0.5

Execution: one trading day after signal formation

Transaction cost: 10 bps per unit of position turnover

Hedge ratio: fixed from the 2021--2023 training period

For the residual spread

Spread_t = TXN_t - (alpha + beta × GS_t)

daily hedged P&L is approximated as

Delta_TXN_t - beta × Delta_GS_t

and normalized by previous-day gross capital.

Results

Metric               2024 Validation      2025 Out-of-Sample

Net Return                +6.41%              -4.22%
Sharpe Ratio               0.818              -0.317
Maximum Drawdown          -3.70%              -9.92%
Trades / Entries            8 trades   15 entries / 15 exits

Additional 2025 results:

Gross return: -1.30%

Annualized volatility: 11.60%

Sortino ratio: -0.304

Modeled transaction cost: 3.00%

Interpretation

The 2024 validation performance did not generalize to the final 2025
test. Gross OOS performance was already slightly negative, while
turnover costs pushed the net result further down.

Rather than optimizing the thresholds after seeing the 2025 result, the
weak OOS performance is retained. This highlights several issues that
matter in quantitative research:

apparent historical relationships may be regime-dependent;

transaction costs can materially alter strategy economics;

testing many candidate pairs creates false-discovery risk;

validation performance alone does not establish robustness;

untouched out-of-sample testing can reveal model instability.

Limitations

This is a research project rather than a production trading system.
Important limitations include:

no pair survived the 5% FDR correction;

the hedge ratio is static rather than dynamically estimated;

transaction costs are simplified and exclude bid-ask spread,
slippage, borrow costs and market impact;

adjusted-close data are used rather than an institutional execution
dataset;

the final OOS period covers one calendar year;

the equity universe was expanded during the research process after
an initial smaller-universe prototype.

Tech Stack

Python: Pandas, NumPy, Statsmodels, Matplotlib, yfinance

Quantitative methods: Cointegration, OLS regression, ADF testing,
mean-reversion half-life, Z-scores, Sharpe/Sortino ratios, drawdown
analysis, multiple-testing correction

Repository Structure

statistical-arbitrage-backtester/
├── README.md
├── Statistical_Arbitrage_Backtest_Clean.ipynb
├── results/
│   └── TXN_GS_Statistical_Arbitrage_Dashboard.xlsx
└── report/
    └── TXN_GS_Statistical_Arbitrage_Project_Report.docx

Key Takeaway

The strongest result of this project is not a profitable backtest. It is
the construction of a cost-aware, chronologically separated research
pipeline that identifies when an apparently promising statistical
relationship fails to generalize.

This is an educational quantitative-finance research project and is
not investment advice.
