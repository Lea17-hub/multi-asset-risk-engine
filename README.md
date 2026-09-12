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

### 5. Why Correlation and Volatility Belong Together

Correlation and volatility are not two separate topics chosen independently.
Together, they determine portfolio risk.

For two assets:

σ²ₚ = w₁²σ₁² + w₂²σ₂² + 2w₁w₂ρ₁₂σ₁σ₂

More generally:

σ²ₚ = wᵀΣw

The covariance matrix Σ therefore combines two important pieces of information:

- **Volatility** describes how strongly each individual asset fluctuates.
- **Correlation** describes how different assets move relative to each other.

Since:

Cov(rᵢ, rⱼ) = ρᵢⱼ σᵢ σⱼ

portfolio risk can change because individual volatilities change, because
correlations change, or because both change at the same time.

This gives the analysis a natural progression:

**Returns → Correlations and Volatilities → Covariance Matrix → Portfolio Risk**

The previous analysis showed that relationships between assets are not stable.
The next question is therefore complementary:

> **Is the risk of an individual asset itself stable?**

Together, these observations suggest a broader hypothesis:

> **Diversification can become unstable when market regimes change because both
> individual asset risk and relationships between assets can change.**

## Dynamic Volatility: Rolling, EWMA and GARCH

Daily SPY returns show clear volatility clustering: large price movements tend to occur in periods of market stress, while quieter periods contain much smaller fluctuations. This means that a single full-sample volatility estimate cannot describe how risk evolves over time.

### 60-Day Rolling Volatility

A 60-day rolling standard deviation estimates current volatility using approximately the last three months of trading days.

It makes changes in market risk visible, but gives every observation inside the window equal weight and then completely removes it once it leaves the window. This can create mechanical features such as plateaus and sudden drops that are partly caused by the model rather than by the market itself.

### EWMA

Exponentially Weighted Moving Average (EWMA) gives more weight to recent observations:

σ²ₜ = λσ²ₜ₋₁ + (1 − λ)r²ₜ₋₁

Using λ = 0.94, recent shocks receive more weight while older information gradually decays.

Unlike a rolling window, EWMA does not suddenly forget an observation. However, λ must be chosen, and the model has no explicit long-run volatility level. If no new shocks occur, its variance estimate eventually decays toward zero.

### GARCH(1,1)

GARCH extends this idea:

σ²ₜ = ω + αr²ₜ₋₁ + βσ²ₜ₋₁

For SPY over the sample period, the fitted model produced approximately:

- ω = 0.0395
- α = 0.1604
- β = 0.8042
- α + β = 0.9646

The high value of α + β indicates persistent volatility, while α + β < 1 allows volatility to mean-revert toward a long-run level.

Because returns were scaled to percentage units before estimation, the implied long-run daily volatility is approximately 1.06%.

Compared with EWMA (λ = 0.94), the fitted GARCH model reacts more strongly to new shocks and allows shock sensitivity and volatility persistence to be estimated separately.

### What the Comparison Shows

The three models can give substantially different risk estimates from exactly the same return history.

During abrupt shocks such as 2020 and 2025, GARCH reacts much more strongly than EWMA or the 60-day rolling estimator. During the more persistent volatility of 2022–2023, the three estimates are considerably closer.

This suggests an important distinction:

> Risk is not simply observed; it is estimated.

The measured level of risk depends not only on the market regime, but also on the model used to measure it. Model dependence appears particularly important during abrupt changes in market conditions.

This leads to the next question:

**How can we evaluate whether a volatility model is actually good?**

The next stage of the project will therefore focus on model validation rather than immediately adding more complex models.

## Core Principle

Every model in this project will be documented in terms of:

1. Financial question
2. Economic intuition
3. Mathematical formulation
4. Model assumptions
5. Implementation
6. Limitations
7. Empirical validation
