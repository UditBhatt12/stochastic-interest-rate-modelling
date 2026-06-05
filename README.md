# Stochastic Interest Rate Modelling via Cox-Ingersoll-Ross: Calibration, Extensions & Out-of-Sample Forecasting

## Overview
This repository implements and stress-tests the **Cox-Ingersoll-Ross (CIR)** framework for modelling the dynamics of zero-coupon yield curves. The pipeline spans the full quantitative workflow, moving from raw data engineering to blind out-of-sample backtesting.

The instantaneous short rate $r_t$ satisfies:

$$dr_t = \kappa(\theta - r_t)\,dt + \sigma\sqrt{r_t}\,dW_t$$

Strict positivity of $r_t$ is guaranteed by the **Feller condition**:

$$2\kappa\theta > \sigma^2$$

---

## Project Pipeline

### Robust Data Engineering
* **Business-day alignment** and cross-sectional spline repair.
* **Outlier filtering** via a 20-day Rolling Median Absolute Deviation (MAD) filter.

### Cross-Sectional Calibration
* **Tenor-Weighted Mean Squared Error (MSE)** objective.
* **Global optimization** using Differential Evolution.
* **Calibration constrained** by the Feller condition.

### Three Structural Extensions
* **Affine Jump-Diffusion (AJD)** (Duffie-Pan-Singleton) to capture policy shock risk.
* **CIR++** deterministic shift (Brigo-Mercurio) for perfect initial curve fit.
* **Two-Factor CIR** with exact algebraic state extraction for independent factor dynamics.

### Blind Out-of-Sample Backtest
* **Frozen parameters** tested on 523 days of unseen market data.
* **Forward-stepping Extended Kalman Filter**.
* **Rigorous diagnostics** (per-tenor $R^2$ and RMSE).

## Performance Comparison
The models were backtested against out-of-sample data, yielding the following performance metrics:

| Model | OOS $R^2$ | OOS RMSE | Architecture Highlight |
|--------|------------|-----------|-------------------------|
| **Base CIR** | 0.8755 | ~24 bps | Single-factor, 3 parameters, fully analytic |
| **CIR++** | 0.9311 | ~18 bps | Deterministic shift corrects initial curve |
| **AJD** | 0.9342 | ~17 bps | Poisson jumps capture policy shock risk |
| **Two-Factor** | 0.9283 | ~19 bps | Independent short/long-end factor dynamics |

## Executive Conclusion
For production environments, the **Affine Jump-Diffusion (AJD) model paired with an Extended Kalman Filter** provides the best out-of-sample accuracy while maintaining the analytical tractability of the affine class. The base CIR model remains a robust, highly interpretable choice suitable for regulatory stress-testing.