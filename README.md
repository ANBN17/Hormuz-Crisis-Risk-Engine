# Hormuz Crisis Risk Engine
**K & T Quant Labs** | March 2026

## Overview
Quantitative risk analysis of the 2026 Strait of Hormuz crisis.
Covers oil price volatility, energy equity stress testing,
marine war-risk insurance exposure, and GARCH-based VaR/CVaR
modelling across 18 global assets.

## Key Findings
- Brent crude: $89.4 to $106.12/bbl (+18.7% since Feb 28, 2026)
- Brent intraday peak: $119.5/bbl (March 9, 2026)
- Strait of Hormuz transits collapsed -81% within 24 hours
- War-risk insurance: 0.030% to 0.750% (25x surge) per hull value per voyage
- Expected aggregate insured losses: $2.05B

## Methodology

| Module       | Method                                          |
|-------------|--------------------------------------------------|
| Volatility  | GARCH(1,1) with Student-t innovations            |
| Market Risk | 99% VaR and CVaR (Expected Shortfall)            |
| Validation  | Kupiec POF Test + Basel Traffic Light (SR 11-7)  |
| Stress Test | 4-scenario Hormuz disruption analysis             |
| Insurance   | Aggregate loss model (hull + cargo)               |

## Repository Structure

    Hormuz_Crisis_Risk_Engine/
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
    |-- README.md

## Assets Covered
- **Oil and Gas:** Brent (BZ=F), WTI (CL=F), Natural Gas (NG=F)
- **Energy Equities:** XOM, CVX, BP, SHEL, COP, OXY
- **Insurance:** CB (Chubb), AIG, MKL, EG, TRV
- **Macro:** Gold, S&P 500, 10Y Treasury, USD Index, Oil ETF

## Scenarios

| Scenario              | Oil Change | Prob | Brent Target |
|----------------------|-----------|------|-------------|
| Base: 2-week         | +15%      | 40%  | $100        |
| Prolonged: 1-2 months| +35%      | 35%  | $135        |
| Severe: 3+ months    | +60%      | 20%  | $160        |
| Catastrophic blockade| +120%     | 5%   | $200        |

## Requirements

    pip install arch yfinance scipy matplotlib pandas numpy

## Run

    python hormuz_risk_engine.py

---
*K & T Quant Labs | Research Only | Not Financial Advice*
*Generated: 27 March 2026 01:08*