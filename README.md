# Pairs Trading: Cointegration and Statistical Arbitrage (KO/PEP)

A coursework project testing whether a classic mean-reversion pairs trading strategy holds up once look-ahead bias, transaction costs, and multiple-testing are handled properly.

## Background

The first version of this project (see commit history) showed a promising +23% return on Coca-Cola / PepsiCo. It turned out to be an artifact of using full-sample statistics — the hedge ratio, mean, and standard deviation were all computed with knowledge of future prices. Once that's fixed, the picture changes completely.

## What's here

- OLS-based hedge ratio and spread construction on log prices
- Rolling 252-day beta and rolling 60-day z-score (instead of one fixed estimate for the whole period)
- A state machine for entries and exits: enter at |z| > 2, exit on return to ±0.5, stop-loss at |z| > 4, time-stop at 60 days
- Transaction costs modeled on turnover, with a sensitivity table across 0–20bps
- An intra-sector screen across 23 tickers (Consumer Staples, Banks, Tech Hardware) and 84 pairs, formation period 2015–2019, with a check against how many pairs would pass by chance alone
- Out-of-sample test (2020–2026) of the top screened candidates

## Result

No combination — the original KO/PEP pair or the pairs found through screening — produces a Sharpe ratio suggesting a tradeable edge after costs. KO/PEP nets -16.9% over the period; the best of the screened banking pairs is roughly flat. Even pairs that screened better than chance alone in-sample (12 of 84 vs ~4 expected) failed to hold up out-of-sample.

This lines up with what's been published on the topic — Do & Faff (2010, 2012) found that daily-frequency pairs trading profitability in US equities mostly disappeared after the early 2000s. A negative, well-diagnosed result here is worth more than a positive one that doesn't survive scrutiny.

## Known limitations

- Only three sectors and 23 tickers were screened — a larger universe might turn up something this one didn't
- The state-machine exit rule performs worse than a simpler "re-evaluate signal daily" version on this data; the notebook digs into why, but it's not fully resolved
- No Bonferroni/Benjamini-Hochberg correction is applied formally to the screen, only compared against the naive expected-by-chance count

## Running it

Everything is in `cointegrated_pairs_trading.ipynb`. Needs `pandas`, `numpy`, `statsmodels`, `yfinance`, `matplotlib`. Data is pulled live from Yahoo Finance, so exact numbers will drift slightly over time as adjusted closes get revised.

---
First real git/GitHub project — earlier commits are rougher than the later ones, and that's on purpose left visible rather than cleaned up.