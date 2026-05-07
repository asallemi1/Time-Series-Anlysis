# Time-Series-Anlysis
A complete time series forecasting project focused on road traffic flow prediction using statistical and machine learning approaches.  
The project analyzes hourly traffic measurements collected between January 2015 and November 2016 and forecasts traffic values for December 2016.

---

## Project Overview

The main objective of this project is to compare different forecasting methodologies for highly seasonal traffic time series data.

The workflow includes:

- Exploratory Data Analysis (EDA)
- Data cleaning and preprocessing
- Stationarity analysis
- Time series decomposition
- Feature engineering
- Statistical forecasting models
- Machine learning forecasting models
- Model comparison using MAE
- Final deployment of the best-performing model

The project evaluates:

- ARIMA models
- Unobserved Components Models (UCM)
- Machine Learning models:
  - Random Forest
  - XGBoost
  - K-Nearest Neighbors (KNN)

---

## Dataset

The dataset contains hourly traffic measurements from:

- **Start:** 2015-01-01 00:00:00
- **End:** 2016-12-31 23:00:00

### Dataset Structure

| Column | Description |
|---|---|
| timestamp | Full datetime |
| date | Date |
| hour | Hour of the day |
| traffic_measurement | Traffic flow value |

### Main Characteristics

- 17,544 observations
- Hourly frequency
- Strong weekly seasonality
- Presence of:
  - zero values
  - outliers
  - seasonal patterns

---

# Data Preprocessing

## 1. Zero Values Handling

The dataset contained **107 zero values**, considered unrealistic traffic measurements.

### Strategy

Zeros were replaced using:

- **Hourly median imputation**
- Median calculated across the same hour of different days

This preserves daily traffic structure while avoiding unrealistic shutdown periods.

---

## 2. Outlier Treatment

Outliers were defined as:

- Values above the **99th percentile**

### Strategy

Extreme values were replaced with:

- Global median of the time series (`0.0368`)

This reduced the impact of rare spikes on model training.

---

## 3. Time Series Splitting

The original time series was split into:

- **24 independent hourly sub-series**
- One model for each hour of the day

This approach improved the ability to capture hour-specific traffic dynamics.

---

## 4. Stationarity Processing

To stabilize variance and remove seasonality:

### Applied transformations

- Box-Cox transformation
- Seasonal differencing with period `7`

### Validation

Stationarity was verified using:

- ACF/PACF analysis
- Augmented Dickey-Fuller (ADF) test

---

## 5. Feature Engineering

Additional features included:

- 1-day lag
- 7-day lag
- 1-day difference
- 7-day difference
- Day of week
- Day of year
- Holiday dummy variables

### Holidays Included

- Christmas
- Christmas Eve
- St. Stephen’s Day
- New Year’s Day
- New Year’s Eve
- Epiphany
- Valentine’s Day
- Easter
- Easter Monday
- Ferragosto

---

# Train / Validation Split

| Set | Period |
|---|---|
| Training | 2015-01-01 → 2016-09-21 |
| Validation | 2016-09-22 → 2016-11-30 |
| Forecast Period | December 2016 |

---

# Evaluation Metric

Model performance was evaluated using:

## Mean Absolute Error (MAE)

\[
MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i - \hat{y}_i|
\]

Where:

- \( y_i \) = actual value
- \( \hat{y}_i \) = predicted value

Lower MAE indicates better forecasting performance.

---

# Models Evaluated

## ARIMA Models

Tested configurations:

- ARIMA(3,1,1)(0,1,1)[7]
- ARIMA(3,1,1)(0,1,2)[7]
- ARIMA(3,1,1)(1,1,1)[7]

Both with and without holiday dummy variables.

### Best ARIMA Model

| Model | MAE |
|---|---|
| ARIMA(3,1,1)(0,1,1)[7] + dummies | **0.009141** |

---

## Unobserved Components Models (UCM)

Tested components:

- Local Linear Trend (LLT)
- Weekly seasonal dummy
- Trigonometric seasonal harmonics
- Holiday dummy variables

### Best UCM Model

Components:

- Local Linear Trend
- Weekly seasonal dummy (7)
- Two annual trigonometric harmonics (365)
- Holiday dummy variables

| Model | MAE |
|---|---|
| LLT + seasonal dummy + 2 harmonics + dummies | **0.008935** |

This was the best-performing model overall.

---

## Machine Learning Models

Tested algorithms:

- Random Forest
- XGBoost
- KNN

Both with and without holiday dummies.

### Best ML Model

| Model | MAE |
|---|---|
| KNN | **0.013021** |

### Observation

Machine learning models struggled to capture:

- Weekly seasonality
- Long-term temporal dependencies

compared to classical statistical approaches.

---

# Final Results

| Model Family | Best Model | MAE |
|---|---|---|
| ARIMA | ARIMA(3,1,1)(0,1,1)[7] + dummies | 0.009141 |
| UCM | LLT + seasonal dummy + 2 harmonics + dummies | **0.008935** |
| ML | KNN | 0.013021 |

---

# Key Findings

- Strong weekly seasonality dominates traffic dynamics
- Statistical models outperformed ML approaches
- UCM achieved the best balance between:
  - interpretability
  - robustness
  - predictive accuracy
- Holiday effects slightly improved forecasts
- Splitting the series into 24 hourly sub-series significantly improved performance

---

# Technologies Used

- Python / R 
- Time Series Analysis
- ARIMA
- State Space Models
- Machine Learning
- Statistical Forecasting
- Data Visualization

---

# Future Improvements

Possible extensions of the project:

- Deep Learning models (LSTM, GRU, Transformers)
- Hyperparameter optimization
- Probabilistic forecasting
- Combinign multiple models
- External weather data integration

---

# Conclusion

This project demonstrates that classical statistical forecasting techniques remain highly effective for structured seasonal traffic time series.

Among all evaluated approaches, the Unobserved Components Model (UCM) achieved the best overall forecasting performance and interpretability, making it the most suitable candidate for deployment in real-world traffic prediction systems.

---

# License

This repository is intended for academic and educational purposes only.
