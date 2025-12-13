# Temperature Forecasting with Dynamic Harmonic Regression and Boosting

This repository contains the code, experiments, and analyses developed for the article:  
**Avaliação da Regressão Harmônica Dinâmica com Boosting para Previsões de Temperatura em 365 Dias: Um Estudo de Caso em Chicago**

---

## Overview

Long-horizon temperature forecasting is a challenging problem due to strong annual seasonality and the presence of heteroscedasticity. This project evaluates a **hybrid** forecasting approach that combines:

- **Dynamic Harmonic Regression (DHR)** to model temporal and seasonal structure;
- **LightGBM** applied to the DHR residuals to capture remaining patterns not explained by the statistical model.

The study uses daily meteorological data from **Chicago (USA)** and considers forecasting horizons of up to **365 days ahead**.

---

## 🎯 Objectives

- Evaluate the performance of Dynamic Harmonic Regression in long-term forecasts;
- Investigate whether modeling residuals with machine learning improves accuracy;
- Analyze limitations of hybrid models in scenarios with strong seasonal structure and covariates with low predictive power at large lags.

---

## 📊 Dataset

- **Source:** `Meteostat` API (Python)
- **Period:** 2014-11-21 to 2025-11-09
- **Frequency:** Daily
- **Target variable:** Daily average temperature (`tavg`)
- **Covariates used:**
  - Minimum and maximum temperature
  - Precipitation
  - Snow
  - Wind speed
  - Atmospheric pressure
  - Daily thermal amplitude (derived variable)

Variables with low completeness were removed, and the data underwent rigorous sanitation and preparation.

---

## 🧠 Methodology

The work was structured into four main stages:

### 1. Data Sanitization
- Consistency and temporal continuity checks;
- Removal of variables with high missingness;
- Treatment of outliers and inconsistencies.

### 2. Exploratory Analysis
- Identification of dominant annual seasonality via wavelet analysis;
- Investigation of trend (considered negligible);
- Assessment of heteroscedasticity;
- Tests with Box–Cox transformation (later discarded).

### 3. Temporal Modeling
- Fitting multiple **Dynamic Harmonic Regression** models (ARIMA with Fourier terms);
- Model selection based on AIC and sliding-window validation;
- Selection of DHR as the baseline model.

### 4. Residual Modeling
- Extensive feature engineering with long lags (365–730 days);
- Comparison between XGBoost and LightGBM;
- Multi-stage variable selection (gain and forward selection);
- Hyperparameter optimization using Optuna and Grid Search.

---

## 📈 Results

- **Dynamic Harmonic Regression alone** performed as well as or better than the hybrid model across most forecast horizons;
- The **hybrid model (DHR + LightGBM)** produced marginal improvements, more noticeable at longer horizons;
- Results indicate that in contexts with strong seasonality and weakly informative covariates, the potential gain from machine learning is limited.

---

## 🧪 Evaluation Metrics

- **MAE (Mean Absolute Error)** – main metric  
- **RMSE (Root Mean Squared Error)** – used in exploratory analyses

---

## 🛠️ Technologies Used

- Python  
- `statsmodels`  
- `lightgbm`  
- `xgboost`  
- `optuna`  
- `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`

---

## 📁 Repository Structure

├── data/ # Raw and processed data  
├── notebooks/ # Exploratory analyses and experiments  
├── src/ # Model source code  
├── results/ # Results, metrics, and figures  
├── README.md # Project documentation  
└── artigo.pdf # Final article version

---

## 🚀 Reproducibility

The experiments use sliding-window validation and cross-validation. Preprocessing, modeling, and evaluation steps were automated to facilitate the reproduction of results.

---

## 📚 Reference

If you use this repository, cite:

> Moura, P. G. *Assessment of Dynamic Harmonic Regression with Boosting for 365-Day Temperature Forecasts: A Case Study in Chicago*.
