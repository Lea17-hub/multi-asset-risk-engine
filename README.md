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

## First Findings

### 1. Adjusted Close vs. Close

`Close` is the actual market closing price on a given day.

`Adjusted Close` is a historical price series adjusted for distributions and
corporate actions such as stock splits. It is more suitable for our purpose
because we want to measure investment returns rather than raw price changes.

Therefore, returns are calculated using Adjusted Close prices.

### 2. The Eight Assets

- SPY — US large-cap equities
- QQQ — US growth / technology equities
- IWM — US small-cap equities
- VGK — European equities
- HYG — high-yield corporate bonds
- TLT — long-term US Treasury bonds
- GLD — gold
- UUP — US dollar exposure

### 3. Diversification

Having many assets does not automatically mean having many independent risks.

For example, SPY, QQQ, IWM and VGK show high correlations because they all
contain substantial equity-market risk.

HYG is a bond ETF, but it is also strongly correlated with equities because
high-yield corporate bonds contain significant credit risk.

**Number of assets ≠ number of independent risks.**

### 4. Correlation Is Not Stable

The full-sample correlation between SPY and TLT is about -0.17.

However, the 60-day rolling correlation changes substantially over time,
from strongly negative to clearly positive values.

This means that the diversification benefit of long-term Treasury bonds is
not constant.

A simple intuition:

- During some economic downturns, stocks may fall while interest rates fall.
  Falling interest rates can increase long-term Treasury bond prices.
  In this situation, SPY may fall while TLT rises.

- During an inflation and rising-rate environment, higher interest rates can
  hurt both long-term bonds and stocks.
  In this situation, SPY and TLT may fall together.

Therefore:

**Diversification itself can be regime-dependent.**

This motivates the central question of the project:

> How stable is portfolio risk when market regimes change?

## Core Principle

Every model in this project will be documented in terms of:

1. Financial question
2. Economic intuition
3. Mathematical formulation
4. Model assumptions
5. Implementation
6. Limitations
7. Empirical validation
