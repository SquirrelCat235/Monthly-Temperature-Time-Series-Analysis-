# 🌡️ Monthly Temperature Time Series Analysis — India (1901–2021)

A statistical time series project that models, forecasts, and analyzes monthly temperature data across India using classical decomposition, ARMA modeling, anomaly detection, and structural break analysis — built entirely in **R**.


## 📌 Project Overview

This notebook performs a full time series pipeline on 120 years of monthly temperature records sourced from the Government of India's open data portal. The goal is to understand long-term climate trends, model seasonal patterns, forecast future temperatures, and detect anomalies and structural shifts in India's climate.


## 📂 Dataset

| Field | Details |
|---|---|
| **Name** | Annual and Seasonal Mean Temperature of India |
| **Source** | [data.gov.in](https://www.data.gov.in/resource/monthly-seasonal-and-annual-mean-temperature-series-period-1901-2021) |
| **Period** | 1901 – 2021 |
| **Frequency** | Monthly |

---

## 🔬 Methodology

### 1. Exploratory Analysis
- Plotted the full monthly time series (1901–2021)
- Identified a slight upward trend, consistent seasonality, and absence of cyclical variation
- Applied the **Mann-Kendall Trend Test** to statistically confirm the presence of a trend

### 2. Trend Modeling
- Compared six trend models: Linear, Quadratic, Cubic, Exponential, Logarithmic, Polynomial (degree 4)
- Selected **Quadratic Trend** based on AIC and R² — best balance of fit and simplicity
- Converted annual trend coefficients to a monthly trend equation

 <img width="348" height="348" alt="image" src="https://github.com/user-attachments/assets/3a55d737-e729-474a-b594-a2eeba83dc7d" />


### 3. Seasonal Decomposition
- Detrended the series by subtracting the quadratic trend
- Computed seasonal indices using both a **manual method** and R's `decompose()` function
- Verified both methods produce near-identical seasonal components

<img width="292" height="292" alt="image" src="https://github.com/user-attachments/assets/952479b6-9201-4772-8e89-aa32c3715d03" />


### 4. Residual Analysis
- Extracted residuals after removing trend and seasonality
- **ADF Test** confirmed residuals are stationary (no differencing needed)
- **Box-Pierce Test** confirmed significant autocorrelation in residuals
- Examined ACF, PACF, and EACF plots to identify ARMA structure

<img width="364" height="364" alt="image" src="https://github.com/user-attachments/assets/ab6136cd-e190-4e10-b160-acf1dd2e0f43" />



### 5. ARMA Modeling
- Fitted and compared ARMA(1,2), ARMA(2,2), ARMA(3,2)
- Selected **ARMA(1,2)** based on AIC — AR(1) captures the PACF cutoff, MA(2) captures early ACF spikes
- Validated with **Ljung-Box Test** on model residuals


### 6. Forecasting
- Reconstructed the full forecast as: **Trend + Seasonal + ARMA Residual Forecast**
- Evaluated on a held-out test set (last 5% of data, ~2016 onwards)
- Forecast extended up to 2032

> <img width="313" height="313" alt="image" src="https://github.com/user-attachments/assets/83cc45e4-c5df-4a38-b410-d5f0af651a5b" />

### 7. Anomaly Detection
- Used **Isolation Forest** (`isotree` package) on ARMA residuals to flag historical anomalies
- Applied a statistical bound (mean ± 2 SD) to detect anomalies in future forecasts
- Plotted anomalies per decade to track climate variability over time
- Computed **baseline-adjusted temperature anomalies** using the first 30 years as reference

<img width="320" height="320" alt="image" src="https://github.com/user-attachments/assets/bc06ae4c-7810-47f8-8270-16bf466bb3dd" />


### 8. Structural Break Detection
- Used the `strucchange` package to identify regime changes in the temperature series
- Detected a significant structural break around the **early 2000s**, indicating a shift in baseline climate

> <img width="364" height="365" alt="image" src="https://github.com/user-attachments/assets/c2ad825c-0c6f-470f-ad2a-f44b705c2f14" />


## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| R | Core language |
| `forecast` | ARIMA modeling and forecasting |
| `Kendall` | Mann-Kendall trend test |
| `tseries` | ADF stationarity test |
| `TSA` | EACF analysis |
| `isotree` | Isolation Forest anomaly detection |
| `strucchange` | Structural break detection |


## 🚀 Getting Started

### Prerequisites
- R (≥ 4.0) or RStudio
- Google Colab (R runtime) — a Colab badge is included in the notebook

### Install Packages

```r
install.packages(c("forecast", "Kendall", "tseries", "TSA", "isotree", "strucchange"))
```

### Run the Notebook

1. Clone this repository
2. Place `TEMP_ANNUAL_MEAN_1901-2021.csv` in the working directory
3. Open `Monthly_Temp_Time_Series_Analysis.ipynb` in Colab or RStudio and run all cells


## 📊 Key Observations

- India's monthly temperatures show a statistically significant **upward trend** over 120 years
- A **quadratic model** best captures the long-term trend
- Seasonal patterns are stable and consistent throughout the series
- **ARMA(1,2)** adequately models residual autocorrelation
- Anomaly frequency per decade fluctuates, with extreme temperatures becoming increasingly normalized in recent decades
- A **structural break around the early 2000s** confirms a shift in India's climate baseline
