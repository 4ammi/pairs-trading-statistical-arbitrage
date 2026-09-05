# Statistical Arbitrage: Pairs Trading Strategy (KO & PEP)

A quantitative finance project implementing a Mean-Reversion Pairs Trading strategy on Coca-Cola (`KO`) and PepsiCo (`PEP`) stock prices using 5-year historical data (2020–2025).

## Executive Summary
This strategy identifies temporary price divergences between two historically cointegrated (not yet tested) assets and exploits mean-reverting behavior via statistical arbitrage. 

- **Asset Pair:** Coca-Cola (`KO`) & PepsiCo (`PEP`)
- **Period:** 2020 - 2025 (Daily Data via Yahoo Finance)
- **Core Concept:** OLS Regression, Spread Construction, Z-Score Thresholds
### Preliminary result, before bias correction:
- **Optimized Strategy Return:** **+23.08%**
- **Sharpe Ratio:** **0.50**
- **Max Drawdown:** **-16.65%**

---

## Mathematical Framework & Strategy Logic

1. **Hedge Ratio Calculation (OLS Regression):**
   We estimate the equilibrium relationship between the two stock prices using Ordinary Least Squares (OLS):
   $$\text{Price}_{KO} = \alpha + \beta \times \text{Price}_{PEP} + \epsilon$$
   *Calculated Hedge Ratio ($\beta$):* **0.2989**

2. **Spread Construction:**
   $$\text{Spread}_t = \text{Price}_{KO, t} - \beta \times \text{Price}_{PEP, t}$$

3. **Z-Score Normalization:**
   To systematically generate trade signals, the spread is normalized into a Z-Score:
   $$Z_t = \frac{\text{Spread}_t - \mu_{spread}}{\sigma_{spread}}$$

4. **Trading Signals:**
   - **$Z_t > +1.0$ (Overvalued KO):** Short KO, Long $\beta$ amount of PEP.
   - **$Z_t < -1.0$ (Undervalued KO):** Long KO, Short $\beta$ amount of PEP.
   - **$Z_t = 0$:** Exit position (Spread reverted to mean).

---

## Performance & Sensitivity Analysis

| Metric | Conservative Strategy ($Z = \pm 2.0$) | Optimized Strategy ($Z = \pm 1.0$) |
| :--- | :--- | :--- |
| **Total Cumulative Return** | +0.72% | **+23.08%** |
| **Sharpe Ratio** | N/A (Low frequency) | **0.50** |
| **Max Drawdown** | Low | **-16.65%** |

### Key Takeaways & Quantitative Insights:
- **Threshold Sensitivity:** The conservative threshold ($Z = 2.0$) generated very few trades on an efficient large-cap pair. Lowering the entry signal to $Z = 1.0$ captured higher transaction frequencies, yielding +23.08% overall growth.
- **Drawdown Analysis:** The drawdown experienced in late 2024 / 2025 highlights the risk of **static beta estimation** when market regimes shift (cointegration structural breakdown).

---

## Future Improvements
To eliminate look-ahead bias and improve risk-adjusted performance:
1. **Dynamic Beta:** Implement **Rolling OLS** or a **Kalman Filter** to adaptively update the hedge ratio over time.
2. **Stop-Loss Risk Management:** Introduce maximum holding periods and Z-score stop-loss thresholds to mitigate regime-shift losses.
3. **Transaction Costs:** Incorporate realistic bid-ask spreads and slippage into the backtest framework.

---

## Tech Stack & Dependencies
- **Language:** Python
- **Libraries:** `pandas`, `numpy`, `statsmodels`, `yfinance`, `matplotlib`
> **first time writing something and uploading it to GitHub. Hope there will be more in the future**
