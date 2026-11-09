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
# Demand Forecasting: Statistical and Machine Learning Approaches

## Time Series Forecasting for E-commerce Inventory Management

### Project Overview

This project explores short-term demand forecasting using the Olist Brazilian E-commerce dataset. The objective is to forecast demand for the next 14 days, starting 7 days after the last date in the training data.

The analysis compares classical time-series methods with machine learning approaches. Particular attention is given to feature engineering, time-based validation, model stability, and the ability to generate forecasts for product categories with limited historical data.

The project focuses on building a forecasting approach that can capture different demand patterns across product categories while remaining robust to sparse observations and irregular demand.

## Project Structure

### 1. Data Preparation

- Loading and inspecting the Olist datasets.
- Checking duplicates, missing values, and data consistency.
- Joining order, product, and category information.
- Translating product categories and handling missing category values.
- Aggregating order-item records into daily demand time series.
- Selecting product categories for individual analysis and grouping less frequent categories into an `other` category.

### 2. Exploratory Data Analysis (EDA)

- Analyzing overall demand trends and daily fluctuations.
- Exploring weekly and seasonal patterns.
- Examining differences in demand across product categories.
- Identifying sparse time series and demand spikes.
- Evaluating the distribution and variability of daily demand.

### 3. Feature Engineering

The machine learning approach uses historical demand and calendar information to represent temporal patterns.

Features include:

- Lagged demand values: 14, 21, 28, 35, 42, and 56 days.
- Rolling statistics, including means and medians.
- Seasonal baseline features.
- Calendar-based features.
- Cyclical day-of-year representations.
- Product category information.

Feature engineering is designed to capture short-term demand dynamics, recurring patterns, and differences between product categories.

### 4. Model Implementation

The project compares classical statistical forecasting methods with machine learning models.

**Classical time-series approaches:**
- Seasonal Naive
- Seasonal Mean
- SARIMA
- Holt-Winters

**Machine learning approaches:**
- Ridge Regression
- Random Forest
- LightGBM

The models are evaluated using the same time-based validation framework to make their results comparable.

### 5. Model Validation and Evaluation

A rolling-origin backtesting strategy is used to evaluate forecasting performance across eight consecutive validation periods.

The main evaluation metrics are:

- **WAPE (Weighted Absolute Percentage Error):** measures total absolute forecast error relative to total actual demand.
- **Bias:** measures systematic over- or under-forecasting.

Additional analysis examines:
- Model performance across individual validation periods.
- Differences in performance between product categories.
- Forecast stability over time.
- The contribution of engineered features to model predictions.

### 6. Robustness Analysis

Additional experiments investigate how sensitive model performance is to data characteristics and preprocessing decisions.

These include:
- Winsorization of extreme demand values.
- Comparison of model performance before and after outlier treatment.
- Analysis of prediction errors.
- Evaluation of prediction intervals based on historical residuals.

## Key Findings

The current experiments show that model performance depends on the forecasting method and the structure of the demand series.

- **LightGBM** achieves the best overall WAPE among the evaluated models.
- **Random Forest** also provides competitive results.
- Classical time-series methods remain useful as benchmarks and can perform well for specific demand patterns.
- Lagged and rolling features provide the machine learning models with information about historical demand and recurring patterns.
- Model performance varies across validation periods, which highlights the importance of time-based backtesting rather than relying on a single train-test split.
- Winsorization does not improve every model, so outlier treatment should be evaluated rather than assumed to be beneficial.

## Tech Stack

- **Python** — primary programming language.
- **Pandas, NumPy** — data processing and numerical operations.
- **Matplotlib, Seaborn** — data visualization and exploratory analysis.
- **Statsmodels** — classical time-series modelling and statistical analysis.
- **Scikit-learn** — preprocessing, regression models, and evaluation.
- **LightGBM** — gradient boosting for demand forecasting.
- **Jupyter Notebook** — experimentation and documentation.
## Multi-Model Demand Forecasting for E-commerce
## Project Objective

The main objective is to develop and compare statistical and machine learning approaches for short-term e-commerce demand forecasting.
The forecasting setup considers a 14-day prediction horizon beginning 7 days after the final date in the training data. The solution also considers product categories with limited historical observations, for which category-level patterns and shared information can be useful.
The project emphasizes transparent methodology, reproducible validation, and evidence-based model selection.

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







