# European Average Wages Analysis — Regression Modelling

**Authors:** Islambek Ziyash, Oleksandr Popovych, Yehor Teroshyn  
**Date:** 28 May 2025

## Overview

Statistical analysis of average annual wages across European countries (2016) using data from the **Eurostat** database. The project covers exploratory data analysis, linear regression modelling, and full model diagnostics.

## Project Structure

```
prs_du_3/
├── prs_du_3.Rmd    # R Markdown source document
└── prs_du_3.html   # Rendered HTML report
```

## Datasets (Eurostat)

| Dataset code | Description |
|---|---|
| `nama_10_fte` | Average annual wages (EUR) by country |
| `nama_10_pc` | GDP per capita |
| `educ_uoe_enra04` | Share of population with tertiary education |

## Analysis

### Task 1 — Exploratory Data Analysis
- Loading and filtering data for 2016 (unit: EUR)
- Distribution analysis — histogram and boxplot
- Descriptive statistics (median: €23,002 · mean: €26,846 · range: €6,767–€62,037)
- Right-skewed distribution identified; outliers noted (Luxembourg, Denmark)

### Task 2 — Regression Model
Linear regression model explaining average wages using:
- `gdp_per_capita` — GDP per capita *(strongest predictor)*
- `tertiary_edu` — share of population with tertiary education
- `uses_euro` — whether the country uses the euro (yes/no)
- `government_type` — form of government (monarchy vs. republic)

Additional statistical tests:
- **Pearson χ²-test** of independence between `government_type` and `uses_euro`
- **Welch t-test** comparing wages between euro and non-euro countries

### Task 3 — Model Diagnostics & Optimisation
Step-by-step:
1. Baseline model — Adjusted R² = 0.777
2. **Stepwise AIC** — `uses_euro` automatically removed → simpler model
3. **VIF diagnostics** — no multicollinearity detected (all VIF < 2)
4. Residual diagnostic plots
5. **Shapiro-Wilk test** for normality of residuals (p = 0.638 → normality confirmed)
6. **Breusch-Pagan test** for homoscedasticity (p = 0.16 → assumption satisfied)
7. **Cook's distance** — Luxembourg identified as an influential observation
8. Model without Luxembourg — Adjusted R² = 0.901
9. Final model: `salary ~ gdp_per_capita + government_type`

## R Packages

| Package | Purpose |
|---|---|
| `tidyverse` | Data manipulation and visualisation |
| `eurostat` | Fetching data from Eurostat API |
| `countrycode` | Converting country codes to names |
| `lmtest` | Breusch-Pagan test |
| `car` | VIF diagnostics |
| `DT`, `knitr` | Interactive tables and report rendering |

## Running the Project

1. Install required packages:

```r
install.packages(c("tidyverse", "eurostat", "countrycode", "DT", "knitr", "lmtest", "car"))
```

2. Open `prs_du_3.Rmd` in RStudio and click **Knit → Knit to HTML**.

> An internet connection is required — data is fetched directly from Eurostat at runtime.

## Key Findings

- **GDP per capita** is the strongest predictor of wages (p < 0.001)
- **Form of government** (monarchy vs. republic) has a statistically significant effect on wages
- **Euro adoption** shows no statistically significant effect once other variables are controlled for
- After removing Luxembourg as an influential outlier, the model explains **90% of wage variability**
