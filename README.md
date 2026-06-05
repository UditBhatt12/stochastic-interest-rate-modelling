# Stochastic Interest Rate Modelling via Cox–Ingersoll–Ross: Calibration, Extensions & Out-of-Sample Forecasting

## Overview

This repository implements and stress-tests the **Cox–Ingersoll–Ross (CIR)** framework for modelling the dynamics of zero-coupon yield curves. The pipeline spans the full quantitative workflow, moving from raw data engineering to blind out-of-sample backtesting.

The instantaneous short rate (r_t) satisfies:

[
dr_t = \kappa(\theta - r_t),dt + \sigma\sqrt{r_t},dW_t
]

Strict positivity of (r_t) is guaranteed by the **Feller condition**:

[
2\kappa\theta > \sigma^2
]

---

## Project Pipeline

### Robust Data Engineering

* Business-day alignment
* Cross-sectional spline repair
* Outlier filtering via a 20-day Rolling Median Absolute Deviation (MAD) filter

### Cross-Sectional Calibration

* Tenor-Weighted Mean Squared Error (MSE) objective
* Global optimization using Differential Evolution
* Calibration constrained by the Feller condition

### Three Structural Extensions

#### 1. Affine Jump-Diffusion (AJD)

* Duffie–Pan–Singleton framework
* Models sudden interest-rate regime shifts and policy shocks

#### 2. CIR++

* Brigo–Mercurio deterministic shift extension
* Corrects the initial term structure exactly

#### 3. Two-Factor CIR

* Independent short-end and long-end latent factors
* Exact algebraic state extraction
* Enhanced yield curve flexibility

### Blind Out-of-Sample Backtest

* Frozen parameter evaluation
* Forward-stepping Extended Kalman Filter (EKF)
* Per-tenor (R^2) and RMSE diagnostics
* Tested across **523 days of unseen market data**

---

## Performance Comparison

| Model              | OOS (R^2) | OOS RMSE | Architecture Highlight                      |
| ------------------ | --------- | -------- | ------------------------------------------- |
| **Base CIR**       | 0.8755    | ~24 bps  | Single-factor, 3 parameters, fully analytic |
| **CIR++**          | 0.9311    | ~18 bps  | Deterministic shift corrects initial curve  |
| **AJD**            | 0.9342    | ~17 bps  | Poisson jumps capture policy shock risk     |
| **Two-Factor CIR** | 0.9283    | ~19 bps  | Independent short/long-end factor dynamics  |

---

## Executive Conclusion

For production environments, the **Affine Jump-Diffusion (AJD)** model paired with an **Extended Kalman Filter (EKF)** provides the best out-of-sample accuracy while maintaining the analytical tractability of the affine class.

The **base CIR model** remains a robust and highly interpretable alternative, making it particularly suitable for regulatory stress-testing, scenario analysis, and educational applications.

---

## Key Features

* End-to-end quantitative finance workflow
* Affine term structure modelling
* Differential Evolution calibration
* Feller-condition constrained optimization
* Extended Kalman Filtering
* Blind out-of-sample validation
* Yield curve forecasting and diagnostics
* Research-grade implementation with production-oriented evaluation

---

## Results Summary

* **Best Accuracy:** AJD + EKF
* **Best Interpretability:** Base CIR
* **Best Initial Curve Fit:** CIR++
* **Best Multi-Factor Representation:** Two-Factor CIR

The project demonstrates that carefully engineered extensions of the CIR framework can significantly improve forecasting performance while preserving the mathematical elegance and tractability of affine interest-rate models.
