# US Air Traffic Forecasting with SARIMAX and COVID-19 Intervention Analysis

Time series forecasting of US airline demand across 10 aviation metrics using SARIMAX, with the COVID-19 pandemic modelled as an exogenous structural intervention. Built on 249 monthly observations (Jan 2003 – Sep 2023) from the Bureau of Transportation Statistics.

\---

## The core idea

Standard SARIMA models trained on data that includes the COVID period tend to absorb the pandemic shock into their estimated seasonal and trend parameters — producing distorted post-pandemic forecasts. This project treats COVID-19 as an external structural break via a binary exogenous dummy variable, keeping the model's seasonal structure clean and generating more reliable long-range forecasts.

\---

## Dataset

* **Source:** US Airline Traffic Data — Bureau of Transportation Statistics (BTS), via [Kaggle](https://www.kaggle.com/datasets/yyxian/u-s-airline-traffic-data)
* **Period:** January 2003 – September 2023 (249 monthly observations)
* **Variables:** 10 metrics — domestic and international passengers, available seat kilometres (ASM), revenue passenger kilometres (RPM), flight operations, and load factors

\---

## Methodology

|Step|Detail|
|-|-|
|Stationarity testing|Augmented Dickey-Fuller (ADF) test on all 10 variables|
|Parameter selection|Exhaustive AIC-minimising grid search — 72 candidate models per variable, 720 total|
|Model|SARIMAX with COVID-19 dummy (1 = Mar 2020 – Dec 2021, 0 otherwise)|
|Validation|80/20 temporal train-test split (199 training / 50 test months, test window includes full COVID period)|
|Diagnostics|Ljung-Box, Jarque-Bera, Breusch-Pagan tests|
|Benchmarks|Holt-Winters Exponential Smoothing and Seasonal Naive baseline|
|Forecast horizon|60 months ahead (Oct 2023 – Sep 2028)|

\---

## Key results

**Optimal model parameters (AIC grid search):**

|Variable|SARIMA Order|Seasonal Order|AIC|
|-|-|-|-|
|Dom\_Pax|(2,1,2)|(1,1,1,12)|7344.76|
|Int\_Pax|(0,1,2)|(0,1,1,12)|6481.46|
|Dom\_Flt|(2,1,2)|(1,1,1,12)|5278.03|
|Dom\_LF|(1,0,2)|(0,1,1,12)|1259.53|
|Int\_LF|(1,0,2)|(0,1,1,12)|1174.60|

**Forecast accuracy (50-month test set):**

* Dom\_LF MAPE: 28.42%
* Dom\_Flt MAPE: 37.63%
* Dom\_ASM MAPE: 37.76%
* Note: Elevated MAPE on passenger volume variables is due to near-zero COVID actuals — a known limitation of percentage-based error metrics. RMSE is the primary metric for those variables.

**5-year forecasts (2024–2027):**

* Domestic passengers: 832M (2024) → 941M (2027) — **+13.1% growth**
* International passengers: 128M (2024) → 157M (2027) — **+22.3% growth**
* Domestic load factors stabilise at \~81.5–82.0% through 2027
* International load factor slight decline (80.9% → 78.1%) signals capacity additions outpacing demand in later years

**Derived business metrics:**

* International average haul proxy (RPM per passenger): 2.6248 — **2.94× the domestic equivalent (0.8932)**, reflecting structurally longer international trip distances and higher per-passenger revenue intensity

\---

## Model diagnostics

* 6 of 10 variables pass the Ljung-Box test (random residuals, p > 0.05)
* All variables show non-normal residuals (Jarque-Bera) — attributable to COVID structural break, not model misspecification
* Heteroscedasticity confirmed across all variables (Breusch-Pagan) — variance inflation concentrated in the 2020–2021 COVID period

\---

## Repository structure

```
├── sarimax\\\\\\\_air\\\\\\\_traffic.ipynb   # Full analysis notebook
├── air\\\\\\\_traffic.csv
│   └── us\\\\\\\_airline\\\\\\\_traffic\\\\\\\_bts.csv    # BTS dataset (source: Kaggle)
├── report/
│   └── SARIMAX\\\\\\\\\\\\\\\_Air\\\\\\\\\\\\\\\_Traffic\\\\\\\\\\\\\\\_Final.docx
└── README.md
```

\---

## How to run

```bash
git clone https://github.com/sohom21-bit/us-air-traffic-sarimax-forecasting
cd us-air-traffic-sarimax-forecasting
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn
jupyter notebook sarimax\\\\\\\_air\\\\\\\_traffic.ipynb
```

\---

## Libraries used

`pandas` · `numpy` · `matplotlib` · `seaborn` · `statsmodels` · `scikit-learn`

\---

## Author

**Sohom Halder**
B.Sc. Statistics Honours, University of Calcutta (2025)
Supervised by Prof. Parthasarathi Bera, Department of Statistics, University of Calcutta

\---

## Data source

Bureau of Transportation Statistics, US Department of Transportation.
Dataset accessed via Kaggle: https://www.kaggle.com/datasets/yyxian/u-s-airline-traffic-data

