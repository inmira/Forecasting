## Demand Forecasting: Statistical & ML ApproachTime-Series Analysis for E-commerce 
Inventory ManagementThis project focuses on forecasting consumer demand over a 14-day horizon. The study implements a range of methodologies, from classical statistical models to modern machine learning algorithms with extensive feature engineering.
# Project Structure
1. Exploratory Data Analysis (EDA): Visualization of trends, seasonality, and correlation analysis.
2. Feature Engineering: Generation of deterministic components (trends, Fourier transforms) and lagged variables (lag 1 to 14).
3. Model Implementation: Comparative analysis of three distinct approaches:
   Linear Regression: Baseline and lag-enhanced versions.
   SARIMA: A classical statistical time-series model.
   Random Forest: A non-linear machine learning approach.
4. Quantitative Evaluation: Performance assessment using MAE, RMSE, and MAPE metrics.
5. Advanced Research: Robustness testing via Winsorization and stationarity verification (ADF Test).

# Quantitative Evaluation (Metrics)
The following results were obtained during the testing phase for a 14-day forecast horizon:

| Model | MAE | RMSE | MAPE |
| :--- | :---: | :---: | :---: |
| **Linear Regression (with Lags)** | **39.54** | **47.69** | **13.26%** |
| **SARIMA** | 1.02 | 1.07 | 4.05e15 |
| **Random Forest** | 94.68 | 110.24 | 31.27% |

Note: The extreme MAPE value for SARIMA is a mathematical artifact caused by zero-inflated data points (division by values close to zero). In terms of absolute volume (MAE), SARIMA performed with high precision on sparse data.

# Key Findings
The Power of Lags: Introducing short-term memory (lags 1-14) reduced the Linear Regression error by approximately 50% (MAE decreased from 78.6 to 39.5), proving that historical demand persistence is a critical predictor.
Signal vs. Noise: Implementing Winsorization (capping extreme spikes at the 99th percentile) proved essential for preventing model overfitting to anomalous market events.
Stationarity: The Augmented Dickey-Fuller (ADF) test confirmed the statistical properties of the cleaned series, ensuring the data is suitable for advanced forecasting pipelines.
Model Selection: While Random Forest is a powerful non-linear tool, the Lag-Enhanced Linear Regression provided the most stable and interpretable results for this specific dataset.
# Tech Stack
Python3.10+Pandas / NumPy: Data manipulation and aggregation.
Statsmodels: SARIMA, Augmented Dickey-Fuller test, DeterministicProcess.
Scikit-learn: Linear Regression, Random Forest Regressor.
Plotly / Seaborn: Interactive and static data visualizations.



The main goal of this block is to implement a demand forecasting system for a short-term period (14 days) 7 days from the last date in the data for all product groups.
We rely on the transaction data provided to you. It is important to make sure the system can produce a forecast for new products (which have little or no training data). 
We would like to see 2 approaches for comparison: using machine learning and classical time series forecasting. The technical solution must be justified by comments regarding
the appropriateness of a particular method or approach.
![image](https://github.com/inmira/Forecasting/assets/159158194/8578907b-a190-4ed8-9ce3-333e1e2539bd)
![image](https://github.com/inmira/Forecasting/assets/159158194/a4899cf0-dbfd-410d-a5d5-cce65c80de06)
![image](https://github.com/inmira/Forecasting/assets/159158194/8960100a-a0b8-4c9a-afcf-e8eb3cb66a85)
![image](https://github.com/inmira/Forecasting/assets/159158194/7c347739-cd85-4cbe-9a98-6c6888109692)
![image](https://github.com/inmira/Forecasting/assets/159158194/9bd4ab7e-5396-4ee9-a714-362987585c33)
![image](https://github.com/inmira/Forecasting/assets/159158194/0857ebb2-e677-4266-b9c9-dc25cf3a683e)
![image](https://github.com/inmira/Forecasting/assets/159158194/7c654bcf-7dd5-45f8-87f8-b3c0a15183bf)
![image](https://github.com/inmira/Forecasting/assets/159158194/98062800-8004-43f4-8149-3f51f218d191)
![image](https://github.com/inmira/Forecasting/assets/159158194/716e3abd-c161-4659-8e31-60fc56edffeb)







