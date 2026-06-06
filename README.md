# Finance_openProject
Reconstructing the US Treasury yield curve from the 3-Month rate using the CIR interest rate model, PCA factor decomposition, and ML benchmarks — achieving R² of 0.89 out-of-sample.

# Yield Curve Reconstruction using CIR Model

Reconstructing the full US Treasury yield curve from a single observable — the 3-Month zero-coupon yield — using the Cox-Ingersoll-Ross (CIR) interest rate model, PCA-based factor analysis, and supervised learning benchmarks.

---

## Overview

This project explores how much of the yield curve can be inferred from the short end alone. Starting from the theoretical CIR framework, the notebook walks through calibration, in-sample validation, out-of-sample testing, and a comparison against data-driven alternatives.

The core question: *can a single-factor short-rate model, calibrated on historical data, reconstruct the term structure well enough to be practically useful?*

---

## Methodology

**1. CIR Model Calibration**
The CIR stochastic differential equation is calibrated by minimizing MSE between model-implied and observed yield curves on the training set. Parameters (κ, θ, σ) are estimated once and frozen for all out-of-sample evaluation.

**2. PCA Factor Analysis**
Principal Component Analysis is applied to decompose the yield curve into latent factors. The analysis identifies a dominant Level Factor and a secondary Slope Factor, together explaining ~99.9% of term structure variation — and directly explains where the single-factor CIR model loses accuracy.

**3. PCA-Based Extension**
An extension maps the 3-Month yield to the latent PCA factors via polynomial regression, then reconstructs the full curve through the inverse PCA transform. Kept entirely out-of-sample.

**4. ML Benchmark**
Ridge, Lasso, ElasticNet, KNN, XGBoost, and Gradient Boosting are evaluated on the same train-test split to establish a data-driven performance ceiling.

---

## Results

| Model | Out-of-Sample R² |
|---|---|
| Ridge Regression | **0.9462** |
| CIR (calibrated) | 0.8945 |
| ElasticNet | 0.8914 |
| Lasso | 0.8895 |
| Gradient Boosting | 0.8020 |
| PCA Extension | 0.7855 |
| KNN | 0.3779 |

The CIR model achieves strong generalization and retains full economic interpretability, while Ridge Regression sets the predictive ceiling — confirming the relationship between the short rate and longer maturities is largely linear over the sample period.

---

## Repository Structure

```
├── yield_curve_prediction.ipynb   # Main notebook
├── train_data.csv                 # Training set (May 2016 – Apr 2024)
├── test_data.csv                  # Full test set (all maturities)
├── test_data_3M.csv               # Test set input (3M yield only)
└── README.md
```

---

## Requirements

```
pandas
numpy
scipy
matplotlib
scikit-learn
xgboost
```

Install with:
```bash
pip install pandas numpy scipy matplotlib scikit-learn xgboost
```

---

## Data

Zero-coupon US Treasury yields across 9 maturities: 3M, 6M, 9M, 1Y, 2Y, 5Y, 10Y, 20Y, 30Y.  
Training period: May 2016 – April 2024 (1,976 trading days).
