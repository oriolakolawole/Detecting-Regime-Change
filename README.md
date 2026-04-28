# Detecting Regime Change in Financial Time Series

A quantitative finance project that identifies **hidden volatility regimes** in Apple (AAPL) stock returns using a **two-state Markov Switching Model**. The analysis models how financial markets alternate between low-volatility and high-volatility states, with a full model improvement pipeline covering stationarity testing, feature engineering, and multicollinearity checks.

---

## Core Idea

Financial return series do not behave uniformly over time. Periods of calm markets alternate with crisis periods — and the transition between these states is not directly observable. The **Markov Switching Model** treats each regime as a hidden state and estimates:

- The **mean and variance** of returns in each regime
- The **transition probabilities** between regimes
- The **smoothed probability** of being in each regime at every point in time

$$y_t = \mu_{S_t} + \varepsilon_t, \quad \varepsilon_t \sim \mathcal{N}(0, \sigma^2_{S_t}), \quad S_t \in \{1, 2\}$$

[Click Here to view the Project](Detecting%20Regime%20Change.ipynb)

---

## Key Findings

Using daily log returns of AAPL from **January 2015 to January 2025**:

| Regime | Mean Return | Std Dev | Interpretation |
|---|---|---|---|
| Regime 0 | +0.159% | ~1.13% | Low volatility / normal market |
| Regime 1 | ~0% (insignificant) | ~2.81% | High volatility / crisis market |

- Both regimes are **highly persistent** — once entered, the market tends to stay in the same state
- Returns are driven primarily by **volatility clustering**, not directional shifts in mean returns
- The final enhanced model achieves **AIC = 9188.27**, substantially outperforming both the GARCH(1,1) benchmark (AIC = 9586.22) and the initial Markov switching model (AIC = 9507.41)

---

## What's Covered

| Section | Description |
|---|---|
| 1. Introduction | Theory of regime switching, Markov chain formulation, full notation table |
| 2. Data Preparation | AAPL download via `yfinance`, log return computation |
| 3. Exploratory Analysis | Price and return plots, return distribution, ACF |
| 4. Benchmark: GARCH(1,1) | Baseline volatility model for comparison |
| 5. Markov Switching Model | 2-regime estimation, parameter extraction, transition matrix |
| 6. Visualisation | Smoothed regime probabilities, regime-coloured price chart, 2022 candlestick with regime overlay |
| 7. Diagnostics | Residual plots, Q-Q plot, residual ACF, 2- vs 3-regime AIC/BIC comparison |
| 8. Model Weaknesses | Stationarity assumption, no feature selection, no out-of-sample validation |
| 9. Model Improvements | ADF/KPSS stationarity tests, feature engineering, outlier removal, VIF check |
| 10. Final Model | Enhanced 2-regime model with exogenous features |
| 11. Deployment | Example decision rule for real-time regime alerting |
| 12. Conclusion | Full AIC/BIC model comparison summary |

---

## Model Improvement Pipeline

The final model is built through a structured data science workflow:

1. **Stationarity testing** — ADF and KPSS tests confirm the return series is stationary
2. **Feature engineering** — 5-day rolling volatility, 21-day rolling volatility, 5-day momentum (all lagged to prevent look-ahead bias)
3. **Outlier removal** — 1st/99th percentile clipping per feature
4. **Multicollinearity check** — VIF values below 10 for all features
5. **Final estimation** — 2-regime `MarkovRegression` with exogenous feature matrix

---

## Deployment Example

The model outputs a real-time regime signal:

```python
latest_prob = final_result.smoothed_marginal_probabilities[1].iloc[-1]

if latest_prob > 0.70:
    print("ALERT: High-volatility regime detected")
else:
    print("Market stable")
```

**Use cases:** market crash detection, volatility monitoring, dynamic portfolio allocation, trading signal generation, risk management dashboards.

---

## Installation

```bash
pip install numpy pandas matplotlib seaborn yfinance arch statsmodels scipy plotly
```

---

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   cd Detecting-Regime-Change
   ```
2. Install dependencies (see above).
3. Open the notebook:
   ```bash
   jupyter notebook Detecting_Regime_Change.ipynb
   ```
4. Run all cells (`Kernel → Restart & Run All`).

> **Note:** An active internet connection is required to download AAPL data via `yfinance`.

---

## Dependencies

| Package | Purpose |
|---|---|
| `yfinance` | Download AAPL historical price data |
| `pandas` / `numpy` | Data manipulation and numerical operations |
| `matplotlib` / `seaborn` | Static charting and diagnostics |
| `plotly` | Interactive candlestick chart with regime overlay |
| `statsmodels` | Markov Switching Model, ACF plots, ADF/KPSS tests, VIF |
| `arch` | GARCH(1,1) benchmark model |
| `scipy` | Q-Q plot and statistical tests |

---

## Repository Structure

```
.
├── Detecting_Regime_Change.ipynb    # Main analysis notebook
└── README.md                        # This file
```

---

## Disclaimer

This project is for **educational purposes only** and does not constitute financial or investment advice. Model outputs should not be used as the sole basis for trading or portfolio decisions.


