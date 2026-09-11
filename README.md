# Multi-Asset Risk Engine

A multi-asset portfolio risk project exploring factor exposures, dynamic
volatility, Expected Shortfall and stress testing across changing market regimes.

## Research Question

**How stable is portfolio risk when market regimes change?**

A portfolio may contain many different assets and appear diversified.
However, these assets can still be exposed to the same underlying risk factors.
Moreover, volatility and correlations are not constant over time.

This project investigates how portfolio risk changes across different market
conditions and compares static and dynamic approaches to risk measurement.

## Initial Portfolio

| Ticker | Instrument | Main Risk Exposure |
|---|---|---|
| SPY | S&P 500 ETF | US equity market |
| QQQ | Nasdaq-100 ETF | US growth / technology equities |
| IWM | Russell 2000 ETF | US small-cap equities |
| TLT | Long-term US Treasury ETF | Interest-rate / duration risk |
| HYG | High-yield corporate bond ETF | Credit + interest-rate risk |
| GLD | Gold ETF | Gold / real-rate / USD exposure |
| VGK | European equity ETF | European equity risk |
| UUP | US Dollar Index ETF | Foreign-exchange risk |

## Research Roadmap

1. Market data and return construction
2. Static portfolio risk and diversification
3. Dynamic volatility: EWMA and GARCH
4. Factor exposures
5. Value at Risk (VaR) and Expected Shortfall (ES)
6. Historical and hypothetical stress testing
7. Model limitations and market regime changes

## Financial Concepts

### Volatility

Volatility measures the dispersion of asset returns and is commonly estimated
using the standard deviation of returns.

Volatility is a **measure of risk, not the definition of risk**.

### Diversification

Portfolio risk depends not only on the risk of individual assets but also on
how their returns move together.

For a portfolio with weights **w** and covariance matrix **Σ**:

**Portfolio Variance**

σ²ₚ = wᵀΣw

Therefore, owning more assets does not automatically imply better diversification.

> **Number of assets ≠ number of independent risks.**

### Dynamic Risk Models

Later stages of the project will compare static volatility estimates with
dynamic models.

**EWMA — Exponentially Weighted Moving Average**

Recent observations receive more weight than older observations.

**GARCH — Generalized Autoregressive Conditional Heteroskedasticity**

GARCH models time-varying volatility and volatility clustering: periods of
high volatility tend to be followed by high volatility, while calm periods
tend to remain calm.

## Core Principle

Every model in this project will be documented in terms of:

1. Financial question
2. Economic intuition
3. Mathematical formulation
4. Model assumptions
5. Implementation
6. Limitations
7. Empirical validation
