## Project Development and Improvements

The project evolved through several stages. Each iteration focused on improving the forecasting methodology, validation strategy, and robustness of the results.

| Development stage | Limitation or problem identified | Improvement introduced | Why it was needed |
|---|---|---|---|
| Initial forecasting approach | The baseline model relied mainly on historical demand patterns and had limited ability to capture changing demand. | Introduced Linear Regression with lagged demand features. | To provide the model with information about recent and past demand and measure the value of historical observations. |
| Feature engineering | A small set of lag features could not fully represent recurring demand patterns and local fluctuations. | Expanded the feature set with longer lags, rolling statistics, seasonal features, and calendar information. | To capture different temporal patterns and improve the representation of demand dynamics. |
| Model comparison | A single modelling approach was insufficient to understand which methods were better suited to the data. | Added classical time-series models and machine learning algorithms, including SARIMA, Holt-Winters, Random Forest, Ridge Regression, and LightGBM. | To compare statistical and ML approaches under the same forecasting setup. |
| Time-based validation | A single train-test split could provide an incomplete picture of model performance. | Introduced rolling-origin backtesting with eight consecutive validation periods. | To evaluate model stability across different points in time and reduce dependence on one validation window. |
| Data robustness | Demand data contains irregular spikes that may affect model fitting. | Tested Winsorization and compared model performance before and after outlier treatment. | To determine whether limiting extreme values improves forecasting accuracy or removes useful demand information. |
| Forecast error analysis | Aggregate accuracy alone does not show whether a model systematically overestimates or underestimates demand. | Added WAPE and bias analysis, together with category-level and validation-period comparisons. | To evaluate both forecast error magnitude and systematic prediction errors. |
| Forecast uncertainty | Point forecasts do not describe the range of possible deviations from actual demand. | Explored prediction intervals based on historical residuals. | To provide additional information about forecast uncertainty. |

The development process focused on evaluating each improvement empirically rather than assuming that more complex models or additional preprocessing would always produce better results.
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

| Model | MAE | RMSE | 
| :--- | :---: | :---: | 
| **Linear Regression (with Lags)** | **39.54** | **47.69** | 
| **SARIMA** | 1.02 | 1.07 | 
| **Random Forest** | 94.68 | 110.24 |


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







