# Demand Forecasting for E-commerce: Statistical and Machine Learning Approaches

14-day demand forecasts by product category on the Olist Brazilian e-commerce dataset, evaluated with rolling-origin backtesting against a seasonal naive baseline.

Notebook: [`demand_forecasting_v4_explained.ipynb`](demand_forecasting_v4_explained.ipynb) (open in Colab from the badge inside the notebook).

## Task

Forecast daily demand (number of ordered item lines) for the next 14 days, starting 7 days after the last date in the training data, for all product groups. Compare a classical time-series approach with machine learning, justify the choice of method, and show how to get a forecast for new products with little or no history.

## Data

- Olist Brazilian E-Commerce public dataset (orders, order items, products, category translation).
- 99,441 orders and 112,650 order-item rows; about 1.42% of items have no category and are kept under a fallback label.
- The target counts **ordered item lines of all order statuses** (including canceled), so it measures orders placed, not delivered sales.
- Top categories are modelled separately; the rest are grouped as `other`. Categories are chosen using only the first training period, so future demand is not used for selection.

## Method

- **Features (leak-free):** every demand feature uses data no later than target date minus 14 days. Lags 14, 21, 28, 35, 42, 56; rolling mean, median and standard deviation; mean of the last four same-weekday values; weekday, month, day of month, weekend, Black Friday period, cyclical day-of-year; category.
- **Models:** seasonal naive, seasonal mean (4 weeks), Ridge, Random Forest, LightGBM; SARIMA and Holt-Winters on the total series.
- **Validation:** rolling-origin backtest, 8 consecutive 14-day windows (2018-05-01 to 2018-08-20, 112 test days). Hyperparameters are fixed, not tuned on the test windows.
- **Metrics:** WAPE and bias (MAE and RMSE as additional). MAPE is not used because daily demand can be zero.

## Results

### Total level (same dates for all models)

| Model | WAPE | Bias |
|---|---|---|
| **LightGBM** | **0.207** | -0.035 |
| Random Forest | 0.219 | -0.022 |
| SARIMA | 0.234 | +0.058 |
| Seasonal mean (4 weeks) | 0.259 | -0.032 |
| Ridge | 0.260 | -0.077 |
| Holt-Winters | 0.277 | +0.008 |
| Seasonal naive (baseline) | 0.294 | -0.012 |

LightGBM lowers WAPE from 29.4% to 20.7% against the baseline (about 30% relative reduction).

### Category level (all categories together)

| Model | WAPE | Bias |
|---|---|---|
| LightGBM | 0.297 | -0.035 |
| Random Forest | 0.307 | -0.022 |
| Ridge | 0.324 | -0.077 |
| Seasonal mean (4 weeks) | 0.332 | -0.032 |
| Seasonal naive (baseline) | 0.388 | -0.012 |

## Key findings

1. **LightGBM has the best overall WAPE, Random Forest is a close second** (21.9%).
2. **The best average model does not win every period.** LightGBM has the lowest WAPE in 3 of 8 windows; SARIMA, Ridge, Random Forest, seasonal mean and Holt-Winters each win a window. The second window (2018-05-14) is hard for every model (WAPE 45% to 63%).
3. **Bias differs by model and category.** LightGBM under-forecasts by 3.5% overall, SARIMA over-forecasts by 5.8%. `health_beauty` (-16%) and `housewares` (-19%) are clearly under-forecast.
4. **Category-level error is higher than total-level error** (29.7% vs 20.7%): small categories are noisier.
5. **Winsorization did not help the stronger models.** LightGBM WAPE rose from 20.7% to 21.2%; Random Forest from 21.9% to 22.2%; Ridge improved only slightly (26.0% to 25.8%). The raw target is kept.
6. **Feature importance:** the most used features are the four-week same-weekday mean and the 28-day rolling mean. This is descriptive, not causal.
7. **Stationarity:** ADF on the level series gives p = 0.056, so the unit-root null is not rejected at 5%; the series is stationary after differencing.

## Uncertainty and new products (exploratory)

- **Prediction interval:** empirical 80% interval from out-of-fold residuals (q10 = -88, q90 = +93 item lines). In the last window all 14 days were inside the interval, but 14 observations are not enough to call it calibrated.
- **Cold start:** forecast of the category divided by the number of distinct products sold in it during the last 90 days. This is an equal-share baseline, not a product-level forecast.
- **Final forecast** for 2018-08-21 to 2018-09-03 is produced by a model refitted on all data.

## Limitations

- Eight 14-day windows (112 days): no full annual cycle and no complete Black Friday period, so the Black Friday effect is not validated.
- The target includes canceled and unavailable orders.
- `other` groups many different categories and hides their patterns.
- No promotions, prices or marketing data; hyperparameters are not tuned.
- SARIMA and Holt-Winters are evaluated at total level only.
- The interval and the cold-start allocation are baselines, not validated methods.

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

## Tech stack

Python, pandas, NumPy, statsmodels (SARIMA, Holt-Winters, ADF), scikit-learn (Ridge, Random Forest, preprocessing), LightGBM, Matplotlib, Seaborn, Jupyter / Google Colab.

## Original task statement

> Implement a demand forecasting system for a short-term period (14 days) starting 7 days after the last date in the data, for all product groups. The system must also be able to produce a forecast for new products with little or no training data. Two approaches are compared: machine learning and classical time-series forecasting. The technical solution must be justified by comments explaining why each method is appropriate.
