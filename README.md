# International Traveler Demand Forecasting

A time-series forecasting project analyzing **monthly international traveler volumes to Canada** and comparing classical statistical models with modern forecasting approaches. The project focuses on identifying trend, seasonality, and temporal dependencies and evaluating how well different models forecast future traveler demand.

## Project Objectives

- Analyze the temporal structure of Canadian international traveler data.
- Identify **trend, seasonality, stationarity, and autocorrelation** patterns.
- Establish simple and statistical forecasting baselines.
- Develop and tune **ARMA and SARIMA** models.
- Compare classical time-series models with **Prophet**.
- Evaluate models using **rolling one-step-ahead forecasting** and multiple error metrics.

## Dataset

The project uses monthly international traveler data from **2010 onward**. The original dataset contains historical observations dating back to 1972, but the analysis focuses on the more recent period to make the forecasting problem more representative of modern applications.

The target variable is the **total number of travelers**.

## Methodology

### 1. Exploratory Time-Series Analysis

The analysis investigates:

- Time-series trends
- Annual seasonality
- Stationarity
- Autocorrelation
- Partial autocorrelation

An **Augmented Dickey-Fuller (ADF) test** was used to assess stationarity. The series achieved a p-value below 0.05, providing evidence against the unit-root null hypothesis.

ACF/PACF analysis revealed strong temporal dependencies and a recurring **6-month seasonal pattern**.

### 2. Baseline Models

Several baseline approaches were established:

- **Mean:** predicts the historical average.
- **Last Value:** predicts the most recent observation.
- **Seasonal Naive (S-Naive):** predicts each month using the corresponding month from the previous year.

The S-Naive model achieved approximately **4.7% MAPE**, outperforming the basic MA and AR models and demonstrating that annual seasonality is an important forecasting signal.

### 3. Statistical Forecasting Models

The project evaluated:

- **Moving Average (MA)**
- **Autoregressive (AR)**
- **Autoregressive Moving Average (ARMA)**
- **Seasonal ARIMA (SARIMA)**

Model parameters were selected using **AIC and residual diagnostics**.

For ARMA, tuning substantially improved performance, reducing MAPE from approximately **4.8% to 1.88%**.

### 4. Residual Diagnostics

Residuals were evaluated using:

- Normal Q-Q plots
- Residual distributions
- Standardized residuals
- **Ljung-Box test**

The final models showed no statistically significant residual autocorrelation across the tested lags, indicating that the models captured the major systematic temporal patterns.

### 5. Seasonal Forecasting with SARIMA

Because the data exhibited strong annual seasonality, **SARIMA** was introduced with a seasonal period of 6 months.

STL decomposition also showed that the seasonal pattern changed substantially during the **COVID-19 disruption**, before gradually moving toward a pattern closer to the pre-COVID period.

SARIMA provided only a marginal improvement over ARMA, suggesting that much of the predictive information was already captured by the non-seasonal temporal dynamics.

### 6. Prophet

**Prophet** was evaluated as an alternative trend-and-seasonality forecasting approach.

The COVID period from **January 2020 to May 2022** was incorporated as a special event/holiday period. Prophet hyperparameters, including:

- `changepoint_prior_scale`
- `seasonality_prior_scale`

were tuned using cross-validation.

Prophet was then evaluated using the same rolling one-month-ahead forecasting framework.

## Evaluation

Models were evaluated using:

- **MAPE** — relative forecasting error
- **MAE** — average magnitude of forecast errors
- **MSE** — penalizes larger forecasting errors more heavily

The evaluation used **rolling one-step-ahead forecasting**, where the model predicts one month ahead and the process is repeated as new observations become available.

## Key Results

| Model            |            MAPE |
| ---------------- | --------------: |
| **SARIMA** | **1.87%** |
| **ARMA**   | **1.89%** |
| Seasonal Naive   |           4.70% |
| AR               |           8.79% |
| MA               |          11.75% |
| Prophet          |          14.33% |

### Key Findings

- **SARIMA achieved the lowest MAPE at 1.87%**, narrowly outperforming ARMA at 1.89%.
- **ARMA substantially improved after parameter tuning**, demonstrating the importance of selecting appropriate AR and MA orders.
- **S-Naive outperformed the basic AR and MA models**, confirming that annual seasonality is an important characteristic of traveler demand.
- SARIMA's improvement over ARMA was **very small**, suggesting that explicitly adding seasonal components provided limited additional predictive power for this one-month-ahead forecasting task.
- **Prophet performed substantially worse**, suggesting that its trend-and-seasonality formulation did not capture the temporal dynamics of this particular series as effectively as the ARMA/SARIMA models.

## Conclusion

The results show that **autoregressive and seasonal time-series models were most effective for forecasting Canadian international traveler demand**. SARIMA achieved the best overall performance, while ARMA performed nearly identically with a simpler structure. The strong performance of S-Naive also confirms meaningful annual seasonality, while Prophet's substantially higher error indicates that its modeling approach was less suited to the short-term dynamics of this dataset.

## Technologies

**Python · Pandas · NumPy · Statsmodels · Prophet · Matplotlib · Scikit-learn · Statistical Time-Series Forecasting**
