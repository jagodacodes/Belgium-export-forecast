# Data Description

## Source
**Eurostat** — Quarterly National Accounts (ESA 2010 methodology)  
URL: https://ec.europa.eu/eurostat/web/national-accounts/data/database

## Dataset: `belgium_quarterly_2000_2024.csv`

| Column | Eurostat Code | Unit | Description |
|--------|--------------|------|-------------|
| `date` | — | YYYY-QN | Quarter identifier (e.g. 2000-Q1) |
| `export` | P6 | €M (current prices) | Exports of goods and services |
| `import` | P7 | €M (current prices) | Imports of goods and services |
| `gdp` | B1GQ | €M (current prices) | Gross Domestic Product |
| `consumption` | P3 | €M (current prices) | Final consumption expenditure |
| `investment` | P5G | €M (current prices) | Gross fixed capital formation |

## Coverage
- **Country:** Belgium (BE)
- **Frequency:** Quarterly
- **Estimation sample:** 2000 Q1 – 2022 Q4 (92 observations)
- **Extended sample (for ex-post validation):** up to 2024 Q4

## Notes
- All values in millions of euros, current prices (not seasonally adjusted)
- First differences (`diff()`) applied in Gretl scripts to ensure stationarity
- ADF test results confirmed all differenced series are I(0)
