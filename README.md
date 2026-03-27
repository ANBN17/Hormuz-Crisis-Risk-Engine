# Hormuz Crisis Risk Engine

> **K & T Quant Labs** | Quantitative Research & Market Analytics

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)
![Domain](https://img.shields.io/badge/Domain-Quant%20Risk-orange)

---

## Overview

A production-grade quantitative risk engine built to model, stress-test,
and visualise the financial market impact of the **2026 Strait of Hormuz
crisis** — the largest oil supply disruption in recorded history.

The engine covers five interconnected risk dimensions:

| Risk Dimension | Methodology | Assets |
|---------------|-------------|--------|
| Oil price volatility | GARCH(1,1) with Student-t innovations | Brent, WTI, NG |
| Market risk | 99% VaR + CVaR (Expected Shortfall) | 18 global assets |
| Model validation | Kupiec POF + Basel Traffic Light (SR 11-7) | All assets |
| Geopolitical stress | 4-scenario disruption analysis | Energy equities |
| Insurance exposure | Aggregate loss model (hull + cargo) | Marine insurers |

---

## The Crisis — Key Facts

On **28 February 2026**, the United States and Israel launched
coordinated airstrikes on Iran (Operation Epic Fury), triggering
an unprecedented closure of the Strait of Hormuz — the world's
most critical oil chokepoint, carrying ~20% of global oil supply.

| Metric | Value |
|--------|-------|
| Brent crude pre-crisis | $89.40 / bbl |
| Brent crude today (Mar 26) | $106.12 / bbl |
| Brent intraday peak | $119.50 / bbl (March 9, 2026) |
| WTI crude today | $93.61 / bbl |
| Price increase since conflict | +18.7% |
| Strait transit collapse | -81% within 24 hours |
| War-risk insurance surge | 0.030% to 0.750% (25x) |
| Per-VLCC voyage cost | $36,000 to $900,000 |
| Cover withdrawn by | Gard, Skuld, NorthStandard, London P&I, American Club |
| DFC / Chubb backstop | $20B reinsurance + $205B statutory cap |
| Expected insured losses | $2.84B (hull + cargo) |
| IEA emergency release | 400 million barrels (largest in history) |

---

## Methodology

### 1. GARCH(1,1) Volatility Model
Uses full-sample GARCH(1,1) with **Student-t innovations** to capture
the fat tails characteristic of geopolitical price shocks.
Conditional volatility is extracted directly from the fitted model
for computational efficiency (~3 seconds per asset vs ~4 minutes
for rolling estimation).

The variance equation:

    sigma^2_t = omega + alpha * epsilon^2_{t-1} + beta * sigma^2_{t-1}

Where alpha + beta (persistence) approaching 1 indicates
volatility shocks decay slowly — typical in crisis periods.

### 2. VaR and CVaR (Expected Shortfall)
- **99% VaR**: Maximum expected loss with 99% confidence over 1 day
- **CVaR**: Expected loss *beyond* the VaR threshold — captures tail risk
  that VaR ignores. Preferred under Basel III/IV as a coherent risk measure.

    CVaR = sigma_t * f(z) / (1 - alpha) * (nu + z^2) / (nu - 1)

### 3. SR 11-7 Model Validation (Kupiec POF Test)
Implements the Federal Reserve's **SR 11-7** model risk management
guidance through the Kupiec (1995) Proportion of Failures test:

- **H0**: Observed violation rate == expected rate (model is calibrated)
- **Reject H0** (p < 0.05): Model is mis-specified
- **Basel Traffic Light**: GREEN (<=4 violations/250d), YELLOW (5-9), RED (10+)

### 4. Geopolitical Scenario Stress Test
Four Hormuz disruption scenarios grounded in Goldman Sachs,
IEA, UBS and Barclays research (March 2026):

| Scenario | Oil Shock | Probability | Brent Target | Source |
|----------|-----------|-------------|--------------|--------|
| Base: 2-week resolution | +15% | 40% | $100 | Goldman Sachs base case |
| Prolonged: 1-2 months | +35% | 35% | $135 | UBS upside scenario |
| Severe: 3+ months | +60% | 20% | $160 | IEA critical supply loss |
| Catastrophic: full blockade | +120% | 5% | $200 | Iran $200 warning / tail |

Energy equity returns modelled using empirical oil-price betas
(upstream: 0.65-0.85) applied to each scenario's oil price change.

### 5. Marine War-Risk Insurance Exposure Model
Bottom-up aggregate loss model covering the Gulf VLCC fleet:

    Expected loss = gulf_fleet * attack_prob * (hull_loss + cargo_loss)

Calibrated to real Lloyd's, Gard and NorthStandard market data.

---

## Results Summary

### GARCH Backtest Results (18 assets)
- **13 of 18 assets PASS** Kupiec POF test at 99% confidence
- Brent Crude: 8 violations (0.4%) — Basel GREEN zone
- WTI Crude: 8 violations (0.4%) — Basel GREEN zone
- Persistence (alpha+beta) range: 0.72 to 0.999
- Highest persistence: IEF (0.9981) — Treasury vol extremely sticky

### Energy Equity Scenario Returns
| Ticker | Beta | Expected Return | Worst Case |
|--------|------|----------------|------------|
| XOM | 0.65 | +23.5% | +9.8% |
| CVX | 0.70 | +25.3% | +10.5% |
| BP  | 0.72 | +26.0% | +10.8% |
| SHEL| 0.68 | +24.6% | +10.2% |
| COP | 0.78 | +28.2% | +11.7% |
| OXY | 0.85 | +30.7% | +12.8% |

### Insurance Market Impact
| Metric | Value |
|--------|-------|
| War-risk premium surge | 0.030% to 0.750% (25x) |
| Pre-crisis per voyage (VLCC) | $36,000 |
| Current per voyage (VLCC) | $900,000 |
| Expected hull losses | $2.03B |
| Expected cargo losses | $0.81B |
| Total expected insured losses | $2.84B |

---

## Charts

| Chart | Description |
|-------|-------------|
| 01_Oil_Price_Timeline | Brent crude price with crisis event markers |
| 02_GARCH_Volatility | Time-varying conditional volatility (annualised) |
| 03_VaR_CVaR | Daily returns vs 99% VaR/CVaR bands + violations |
| 04_Equity_Stress_Test | Energy equity returns across all 4 scenarios |
| 05_Insurance_Premium_History | War-risk premium vs historical crises |
| 06_Kupiec_Backtest | SR 11-7 model validation — all 18 assets |
| 07_Scenario_Analysis | Scenario probabilities + expected vs worst case |
| 08_Full_Dashboard | Combined 6-panel overview dashboard |

---

## Repository Structure

    Hormuz-Crisis-Risk-Engine/
    |-- charts/
    |   |-- 01_Oil_Price_Timeline.png
    |   |-- 02_GARCH_Volatility.png
    |   |-- 03_VaR_CVaR.png
    |   |-- 04_Equity_Stress_Test.png
    |   |-- 05_Insurance_Premium_History.png
    |   |-- 06_Kupiec_Backtest.png
    |   |-- 07_Scenario_Analysis.png
    |   |-- 08_Full_Dashboard.png
    |-- outputs/
    |   |-- garch_results.csv
    |   |-- stress_test.csv
    |   |-- insurance_exposure.csv
    |   |-- executive_summary.txt
    |-- hormuz_risk_engine.py
    |-- requirements.txt
    |-- LICENSE
    |-- README.md

---

## Assets Covered (18 total)

**Oil & Commodities (4)**
Brent Crude (BZ=F), WTI Crude (CL=F), Natural Gas (NG=F), Gold (GC=F)

**Energy Equities (6)**
ExxonMobil (XOM), Chevron (CVX), BP (BP), Shell (SHEL),
ConocoPhillips (COP), Occidental (OXY)

**Insurance & Financials (5)**
Chubb (CB), AIG (AIG), Markel (MKL), Everest Group (EG), Travelers (TRV)

**Macro (3)**
S&P 500 (SPY), US 10Y Treasury (IEF), Oil ETF (USO)

---

## Requirements

    pip install arch yfinance scipy matplotlib pandas numpy requests

Or install from requirements.txt:

    pip install -r requirements.txt

## Run

    python hormuz_risk_engine.py

Expected runtime: approximately 2 minutes (full-sample GARCH on 18 assets).

---

## Skills Demonstrated

- Quantitative risk modelling (GARCH, VaR, CVaR, Expected Shortfall)
- Model risk validation per SR 11-7 / Basel III framework
- Geopolitical scenario analysis and stress testing
- Marine insurance exposure quantification
- Python financial data engineering (yfinance, arch, scipy)
- Professional research-grade data visualisation (matplotlib)

---

## Disclaimer

This project is for **research and educational purposes only**.
It does not constitute financial advice.
All crisis data points are sourced from publicly available
information as of March 26, 2026.

---

*K & T Quant Labs | Quantitative Research & Market Analytics*
*Generated: 27 March 2026*