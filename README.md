# CAPM Analysis — Beta, Alpha & the Security Market Line

Capital Asset Pricing Model analysis across a 6-stock basket spanning US equities and UK-listed stocks — implemented in Python.

# Project Overview

This project applies the Capital Asset Pricing Model (CAPM) to a basket of six stocks over a 4-year window (2021–2024), using the S&P 500 (SPY) as the market proxy. For each stock, the model estimates beta (systematic risk), Jensen's alpha (risk-adjusted outperformance), and R² (the proportion of return variance explained by the market).

The central question: did each stock generate return above and beyond what its level of market risk warranted?

Assets covered: US mega-cap tech (AAPL, META), speculative growth (PLTR, TSLA), and UK-listed equities (LLOY.L, BP.L).

# Methodology

CAPM formula:

E(Rᵢ) = Rf + βᵢ × (E(Rm) − Rf)
Rf — daily risk-free rate, derived from the US 10-year Treasury yield (4.5% annualised ÷ 252)
βᵢ — slope of the OLS regression of stock excess returns on market excess returns
E(Rm) − Rf — equity risk premium: annualised SPY excess return over the period

Beta and Alpha are estimated via scipy.stats.linregress:

Beta = slope of the regression line (systematic risk relative to SPY)
Alpha (Jensen's) = regression intercept × 252, annualised — the return earned above CAPM's expectation
R² = proportion of the stock's daily variance explained by SPY movements
Summary Statistics (2021–2024)
Ticker	Actual Return	CAPM Expected	Alpha (ann.)	Beta	R²
AAPL	21.60%	18.02%	+3.58%	1.2189	0.5803
META	15.94%	23.01%	−7.07%	1.6692	0.3644
PLTR	35.05%	26.55%	+8.51%	1.9878	0.2504
TSLA	11.24%	25.90%	−14.66%	1.9297	0.2894
LLOY.L	7.76%	8.81%	−1.05%	0.3883	0.0574
BP.L	9.78%	6.44%	+3.34%	0.1748	0.0107
Visualisations

The notebook produces three charts using Plotly:

1. Regression subplots — one scatter plot per stock, daily excess returns plotted against SPY excess returns, with the OLS regression line overlaid. The slope of each line is beta — steeper = higher systematic risk. The spread of points around the line reflects R².

<img width="1057" height="582" alt="Screenshot 2026-10-26 at 23 05 39" src="https://github.com/user-attachments/assets/4279402d-975f-419e-a8b7-1016441f3e0b" />

2. Security Market Line (SML) — each stock plotted by beta (x) against actual annualised return (y). The dashed SML shows the CAPM-expected return at each beta level. Stocks above the line generated positive alpha; stocks below underperformed on a risk-adjusted basis.

<img width="1096" height="523" alt="Screenshot 2026-10-26 at 23 06 26" src="https://github.com/user-attachments/assets/22109138-7f4c-44a6-a016-65a90b9b8d7f" />

3. Alpha bar chart — Jensen's alpha for each stock, annualised. Green bars indicate outperformance above CAPM expectations; red bars indicate underperformance. The zero line is the CAPM break-even.

<img width="1044" height="471" alt="Screenshot 2026-10-26 at 23 06 54" src="https://github.com/user-attachments/assets/ebfa70ba-fa76-40e4-bd13-40defc807a84" />

# Key Findings

PLTR generated the highest alpha (+8.51%) — even accounting for its near-double market beta of 1.99, it still returned 8.51% annually above what CAPM predicted. Almost entirely driven by its AI-driven rally in late 2024.

TSLA produced the worst alpha (−14.66%) — with a beta of 1.93, CAPM demanded a return of 25.9% to justify the risk taken. TSLA delivered only 11.2%. The highest beta in the basket, the lowest risk-adjusted return.

META's 2022 crash explains its negative alpha (−7.07%) — its high beta of 1.67 set a demanding return threshold. The year-long drawdown in 2022 wiped enough cumulative return that the 4-year figure never fully recovered on a risk-adjusted basis.

AAPL sits closest to the SML (+3.58% alpha) — the most well-behaved stock in the basket. Moderate beta (1.22), the highest R² at 0.58, and modest but consistent positive alpha.

The UK stocks (LLOY.L, BP.L) have near-zero R² — the market explains almost nothing about their returns. BP.L's R² of 0.01 confirms it moves on oil prices and company-specific factors, not broad US market conditions.

# Requirements
yfinance
pandas
numpy
scipy
plotly
bash
pip install yfinance pandas numpy scipy plotly
Usage

Open capm_analysis.ipynb in Jupyter Notebook or JupyterLab and run all cells top to bottom. Data is pulled live from Yahoo Finance — no manual download needed.

Note: Always run cells in order from the top. Variables are lost on kernel restart and must be re-computed by running each cell sequentially.

# Reflections

This project was built during my second year of university to apply regression-based financial modelling to real market data. It demonstrates:

Computing excess returns and applying the CAPM framework using pandas, numpy, and scipy
Estimating beta, Jensen's alpha, and R² via OLS regression for a mixed US/UK asset basket
Building a three-chart analytical suite using plotly.graph_objects and make_subplots
Interpreting CAPM outputs in the context of risk-adjusted performance across geographies and sectors

Risk-free rate: The US 10-year Treasury (4.5%) is used as the risk-free rate throughout. Since all assets are benchmarked against SPY (a US market index), consistency with a US rate is appropriate. The UK Gilt yield (~4.2% over the period) would be more precise for LLOY.L and BP.L, but the spread is small enough that it does not materially affect beta or alpha estimates.

^FTSE excluded from CAPM analysis: ^FTSE is a market index, not an individual stock. Regressing it against SPY would measure index-vs-index correlation rather than a stock's systematic risk relative to the market. It is retained in the data pull but excluded from all CAPM calculations.

Author

Ryan James Forrester — BSc Economics and Finance, Leeds Beckett University
