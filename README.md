# Quantitative Portfolio Optimization & Risk Analysis Engine

An institutional asset allocation and risk modeling framework built in Excel, applying **Markowitz Modern Portfolio Theory (MPT)** across an 8-asset investment universe (2019–2024).

---

## Executive Summary

This project constructs an automated quantitative model to evaluate how mathematical optimization balances risk and return across market cycles. Using 60 monthly price observations (2019–2024), the model processes log returns, computes dynamic variance-covariance matrices, and leverages non-linear solver optimization to construct the **Markowitz Efficient Frontier** and identify the **Tangency (Max Sharpe)** portfolio.

---

## 📊 Asset Universe

The model evaluates an 8-asset universe spanning diverse major asset classes:

| Ticker | Asset Class | Proxy ETF |
| :--- | :--- | :--- |
| **SPY** | US Large-Cap Equities | SPDR S&P 500 ETF Trust |
| **EFA** | International Developed Equities | iShares MSCI EAFE ETF |
| **EEM** | Emerging Markets Equities | iShares MSCI Emerging Markets ETF |
| **AGG** | US Aggregate Fixed Income | iShares Core U.S. Aggregate Bond ETF |
| **TLT** | Long-Term US Treasuries | iShares 20+ Year Treasury Bond ETF |
| **VNQ** | US Real Estate (REITs) | Vanguard Real Estate ETF |
| **GLD** | Physical Commodities | SPDR Gold Shares |
| **IWM** | US Small-Cap Equities | iShares Russell 2000 ETF |

---

## 📈 Key Findings & Performance Summary

Compared to an **Equal-Weighted (1/N) Baseline**, the **Max Sharpe Portfolio** significantly expanded risk-adjusted returns by exploiting cross-asset covariance and allocating heavily to assets with low historical co-movement (specifically Gold and SPY).

| Performance Metric | Equal-Weighted Baseline (1/N) | Optimized Moderate Portfolio (Max Sharpe) | Variance / Delta |
| :--- | :---: | :---: | :---: |
| **Expected Annual Return** | 2.47% | **10.20%** | **+7.73%** |
| **Annualized Volatility ($\sigma$)** | 13.23% | **13.35%** | +0.12% |
| **Sharpe Ratio ($R_f = 4.0\%$)** | -0.12 | **0.46** | **+0.58** |
| **Primary Allocations** | 12.5% per Asset | **57.6% SPY / 42.4% GLD** | High Diversification Efficiency |

---

## ⚙️ Quantitative Architecture & Methodology

1. **Data Pipeline & Continuous Returns (`Returns` Sheet):**
   * Processed 60 monthly price points (2019–2024) into continuous log returns:
     $$r_t = \ln\left(\frac{P_t}{P_{t-1}}\right)$$
   * Standardized monthly baseline statistics into annualized expected returns ($R_i = \bar{r}_m \times 12$) and annualized volatility ($\sigma_i = \sigma_m \times \sqrt{12}$).

2. **Covariance & Correlation Engine (`Matrices` & `Correlation_Heatmap` Sheets):**
   * Built an $8 \times 8$ Variance-Covariance Matrix ($\Sigma$) and Correlation Matrix ($R$) using Excel array algebra (`MMULT`, `TRANSPOSE`) to measure cross-asset interaction.

3. **Non-Linear Optimization & Solver (`Portfolio_Model` Sheet):**
   * Applied GRG Non-Linear Solver constraints:
     $$\max_{w} \text{Sharpe Ratio} = \frac{w^T R - R_f}{\sqrt{w^T \Sigma w}} \quad \text{s.t.} \quad \sum_{i=1}^{8} w_i = 1, \quad w_i \ge 0$$
   * Generated the **Markowitz Efficient Frontier** by isolating minimum volatility portfolios across target return thresholds from 5.0% to 11.0%.

---

## 🛡️ Downside Stress Testing & Crisis Resilience

The model evaluated downside protection during macro stress events:
* **2020 COVID Market Crash:** While the S&P 500 benchmark dropped **-19.6%**, the risk-optimized allocation limited drawdowns to **-6.2%** (+13.4% relative cushioning).
* **2022 Inflation & Rate Hike Shock:** While SPY declined **-18.1%**, the model cushioned losses to **-8.9%** (+9.2% relative cushioning) by eliminating long-duration fixed income exposure (TLT).
