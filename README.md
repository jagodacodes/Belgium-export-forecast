# 🇧🇪 Belgium Export Forecasting — Econometric Time Series Analysis

> **Time Series | Econometric Modeling | Forecasting | Statistical Testing**

[![Tool](https://img.shields.io/badge/Tool-Gretl-4A90D9?style=flat-square)](http://gretl.sourceforge.net/)
[![Tool](https://img.shields.io/badge/Tool-Excel-217346?style=flat-square)]()
[![Model](https://img.shields.io/badge/Model-Distributed%20Lag-success?style=flat-square)]()
[![Data](https://img.shields.io/badge/Source-Eurostat-003399?style=flat-square)](https://ec.europa.eu/eurostat)
[![Period](https://img.shields.io/badge/Period-2000Q1–2022Q4-lightgrey?style=flat-square)]()

---

## Project Overview

End-to-end econometric analysis forecasting **Belgium's quarterly exports of goods and services** using macroeconomic drivers. The project covers the full analytical pipeline: variable selection, stationarity testing, model estimation, diagnostic verification, and both ex-post and ex-ante forecasting.

**Business question:** *Which macroeconomic factors drive Belgium's export performance — and how accurately can we forecast it one to two years ahead?*

**Key finding:** Consumption growth, lagged GDP, and lagged imports collectively explain ~64% of quarterly export variation. Post-COVID structural shifts (2023–2024) significantly reduce short-term forecast accuracy, highlighting the limits of models trained on stable pre-pandemic data.

---

## Technologies Used

| Tool | Purpose |
|------|---------|
| **Gretl** | Econometric estimation, hypothesis testing, forecasting |
| **Excel** | Data preprocessing, exploratory analysis, chart preparation |
| **Eurostat** | Quarterly national accounts data (ESA 2010) |

**Methods applied:** OLS regression · Distributed Lag (DL) model · ADF unit root tests · White heteroskedasticity test · Chow structural break test · RESET specification test · Breusch-Godfrey LM test · Jarque-Bera normality test · AR(1) autoregressive forecasting · ACF/PACF analysis

---

## Project Process

```
Data Collection (Eurostat)
        ↓
Exploratory Analysis & Descriptive Statistics
        ↓
Stationarity Testing (ADF) → Non-stationary → First Differencing
        ↓
Static OLS Model → Heteroskedasticity Detected (White Test)
        ↓
Distributed Lag (DL) Model → Full Diagnostic Verification
        ↓
Ex-Post Forecast (2023–2024) ← evaluate accuracy on known data
        ↓
Ex-Ante Forecast (2025–2026) via AR(1) sub-models for regressors
```

---

## Data

- **Source:** [Eurostat — Quarterly National Accounts](https://ec.europa.eu/eurostat/web/national-accounts) (ESA 2010)
- **Frequency:** Quarterly | **Sample:** 2000 Q1 – 2022 Q4 | **N = 92 observations**

| Variable | Eurostat Code | Description |
|----------|--------------|-------------|
| **Export** *(dependent)* | P6 | Exports of goods & services (€M) |
| Import | P7 | Imports of goods & services (€M) |
| GDP | B1GQ | Gross Domestic Product (€M) |
| Consumption | P3 | Final consumption expenditure (€M) |
| Investment | P5G | Gross fixed capital formation (€M) |

All variables transformed to **first differences** (Δ) to remove non-stationarity — confirmed via ADF tests at α = 0.05.

---

## Project Structure

```
belgium-export-forecasting/
│
├── README.md
├── report/
│   └── Final_Ekonometria-Projekt-3.pdf   # Full analysis report
│
├── data/
│   └── DATA_DESCRIPTION.md               # Variable definitions & source
│
├── models/                                     # Gretl scripts (.inp)
│   ├── 01_static_ols_baseline.inp             # Baseline OLS + ADF tests + heteroskedasticity detection
│   ├── 02_dl_model_final.inp                  # Final DL model + full diagnostics + ex-post forecast
│   └── 03_ar_exante_forecast.inp              # AR sub-models + ex-ante forecast pipeline 2025–2026
│
└── outputs/
    ├── forecast_expost_2023_2024.png      # Ex-post forecast chart
    └── forecast_exante_2025_2026.png      # Ex-ante forecast chart
```

---

## Results

### Final Model — Distributed Lag (DL)

**Dependent variable:** `Δexport` (quarter-over-quarter change in exports, €M)
**Estimation period:** 2001 Q2 – 2022 Q4 | **N = 87**

| Variable | Coefficient | Std. Error | p-value | |
|----------|------------|------------|---------|--|
| `Δconsumption` | **+1.217** | 0.135 | < 0.001 | *** |
| `Δgdp` *(lag 1)* | **+0.645** | 0.109 | < 0.001 | *** |
| `Δimport` *(lag 4)* | **−0.250** | 0.067 | 0.0003 | *** |
| `Δinvestment` | +0.374 | 0.196 | 0.059 | *(marginal)* |
| Constant | 187.2 | 162.0 | 0.251 | |

**R² = 0.644** — model explains ~64% of variation in export growth
**Adjusted R² = 0.627** | **F(4, 82) = 37.13** | **p < 0.001**

#### Diagnostic Tests

| Test | Statistic | p-value | Result |
|------|-----------|---------|--------|
| RESET (functional form) | F = 0.229 | 0.796 | ✅ Correctly specified |
| White (heteroskedasticity) | LM = 13.57 | 0.482 | ✅ Homoskedastic residuals |
| Breusch-Godfrey LM (lag 1) | LMF = 0.215 | 0.644 | ✅ No 1st-order autocorrelation |
| Breusch-Godfrey LM (lag 4) | LMF = 3.885 | 0.006 | ⚠️ 4th-order autocorrelation |
| Chow structural break | F(5,77) = 3.66 | 0.005 | ⚠️ Break at 2012 Q2 |
| Jarque-Bera (normality) | χ²(2) = 32.90 | < 0.001 | ⚠️ Non-normal residuals |

---

### Key Findings & Business Interpretation

**📈 Consumption → Exports (+1.22)**
A €1M increase in Belgium's domestic consumption growth is associated with a €1.22M increase in export growth. Belgium's highly open economy means domestic demand boosts production capacity, which simultaneously feeds export output.

**📈 GDP growth with 1-quarter lag (+0.65)**
Last quarter's GDP expansion predicts stronger current exports — a one-period lag reflecting the time between economic expansion and export fulfillment. Useful as a leading indicator for export planning.

**📉 Import growth with 4-quarter lag (−0.25)**
Import surges 4 quarters ago correlate with lower current exports — reflecting delayed substitution effects within global value chains. When Belgium ramps up imports of intermediate goods, domestic production temporarily shifts toward the internal market.

---

### Forecast Results

#### Ex-Post Accuracy (2023–2024, 8 observations)

| Metric | Value | Interpretation |
|--------|-------|----------------|
| ME | −2,665.8 | Systematic underestimation |
| RMSE | 3,169.5 | Avg. error ~€3.2B per quarter |
| MAE | 2,774.3 | Avg. absolute error ~€2.8B |
| MAPE | 728.6% | Poor relative accuracy |
| Theil's U₂ | 2.08 | Underperforms naive benchmark |

> **Why the poor performance?** The 2023–2024 period saw post-COVID export rebounds and global demand volatility that the model — trained on 2000–2022 stable patterns — could not anticipate. The Chow-test-detected structural break (2012 Q2) further weakens out-of-sample stability. This is a realistic and expected outcome for linear macro models applied to shock periods — and an important lesson in the difference between in-sample fit and out-of-sample validity.

#### Ex-Ante Forecast (2025–2026)

Explanatory variable forecasts generated via individual **AR(1)** models (lag selection via ACF/PACF):

| Variable | AR Spec | Rationale |
|----------|---------|-----------|
| `Δimport` | AR(4) | Strong seasonal (annual) pattern |
| `Δgdp` | AR(1) | Significant lag-1 autocorrelation (PACF = −0.28***) |
| `Δinvestment` | AR(1) | Mean-reverting short-term dynamics |
| `Δconsumption` | AR(1) | Stable short-term pattern |

**Projection:** Belgium's export level expected to stabilize around **~93,000–94,000 €M quarterly** through 2026, with widening 95% confidence intervals. No strong trend in either direction under baseline macro conditions.

---

## What I Learned

- **Non-stationarity handling** — applied first differencing to all I(1) series; a critical prerequisite before regression to avoid spurious results
- **Iterative model building** — detected heteroskedasticity in static OLS (White test), resolved it by introducing lagged regressors and switching to a DL specification
- **Lag structure selection** — used ACF/PACF plots and AIC to determine optimal AR orders for auxiliary forecast models
- **Compound forecasting pipeline** — built a multi-stage chain where AR sub-models project regressors, which then feed a causal DL model; learned how forecast uncertainty accumulates across stages
- **Honest model evaluation** — distinguishing good in-sample fit (R² = 0.64) from poor out-of-sample performance (Theil U₂ > 1) is one of the most important skills in applied analytics

---

## What Could Be Improved

- Add **exchange rates** (EUR/USD, EUR/GBP) and **commodity price indices** as external regressors
- Test **ARDL** or **VECM** for long-run cointegration between export and import levels
- Include a **COVID dummy variable** (2020 Q2) to isolate the pandemic shock from structural trends
- Use **rolling window estimation** to improve parameter stability across structural breaks
- Benchmark against **ARIMA/SARIMA** or **Prophet** to assess whether causal models outperform pure time series approaches

---

## How to Replicate

1. Download quarterly data for Belgium from [Eurostat](https://ec.europa.eu/eurostat/web/national-accounts/data/database) — codes: P6, P7, B1GQ, P3, P5G
2. Open in **Excel** for initial cleaning and exploratory analysis
3. Import into **[Gretl](http://gretl.sourceforge.net/)** (free) and apply `diff()` transformations
4. Run OLS → check White test → introduce lags → re-run full diagnostics
5. Generate forecasts using `fcast` with `--dynamic` option

Full methodology in [`report/Final_Ekonometria-Projekt-3.pdf`](report/Final_Ekonometria-Projekt-3.pdf)

---



---

*Open to feedback — feel free to open an issue or reach out on LinkedIn.*
