# Kelly Criterion: Expansion and Reassessment

May 2026 — Columbia Statistical Methods in Finance class project: literature examination of the Kelly Criterion, covering theoretical foundations, practical limitations, estimation risk, drawdown sensitivity, and modern portfolio-sizing extensions.

---

## 📋 Project Overview

This repository contains a rigorous statistical and computational examination of the Kelly Criterion—a growth-optimal asset allocation framework with well-known sensitivity to estimation noise. The research:

- **Validates** Kelly geometry under known parameters via Monte Carlo simulation
- **Audits** Several papers' multi-asset optimization pipeline on real market data
- **Challenges** the statistical robustness of aggressive Kelly variants using formal hypothesis tests
- **Demonstrates** how long-only budget constraints mechanically collapse seemingly different strategies

### Key Insight

The apparent outperformance of aggressive Kelly multiples (Double, Triple) over standard Kelly is an **artifact of constraint collapse**—when the long-only constraint $\sum w_i = f \le 1$ binds for $f \ge 1$, Full/Double/Triple Kelly collapse onto an identical portfolio, making statistical comparison meaningless without formal testing.

---

## 🎯 Research Objectives

| Objective | Description |
|-----------|-------------|
| **O1** | Validate Kelly geometry under known GBM parameters via Monte Carlo across four horizons |
| **O2** | Replicate CC20's pipeline—single-equity (Banca Intesa), in-sample, out-of-sample long-only |
| **O3** | Extend to unconstrained (leveraged) Kelly and compare across multiple universes |
| **O4** | Apply formal statistical inference (HAC tests) to deflate apparent performance gaps |

---

## 📊 Key Findings

### Finding 1: Mean-vs-Median Divergence is Structural
The divergence between mean and median terminal wealth grows with leverage ($f^2\sigma^2$). At 10,000 trades:
- **Full Kelly**: Mean = 31.47× but Median = 6.89× (P(Loss) = 12.3%)
- **Triple Kelly**: Mean = 518.50× but Median = 0.01× (P(Loss) = 79.0%)

This shows aggressive multiples destroy median outcomes despite massive mean gains.

### Finding 2: Long-Only Constraint Collapse
Once the budget cap binds ($\sum w_i = f \le 1$), **Full/Double/Triple Kelly are mathematically identical**—they all solve the same QP with $f = 1$.
- Out-of-sample Sharpe: 0.621 (identical for F/D/T Kelly)
- The "outperformance" of aggressive variants is illusion, not optimization

### Finding 3: Formal Tests Deflate Apparent Gaps
Newey-West HAC tests (5 lags) on daily return differences:
- Half Kelly vs Full Kelly: **p = 0.014** ✓ (significant)
- Half Kelly vs MinVar: **p = 0.858** ✗ (not significant)
- Full Kelly vs MinVar: **p = 0.141** ✗ (not significant)

Only Half-vs-Full is statistically significant—driven by the constraint collapse artifact.

### Finding 4: Unconstrained Kelly Fails Out-of-Sample
Lifting the budget cap:
- Sharpe ratios collapse to 0.10–0.18
- Max drawdowns reach 72%–97%
- Leverage amplifies estimation error catastrophically

---

## 🛠️ Technical Stack

| Tool | Purpose |
|------|---------|
| **R** | Core statistical computing and analysis |
| **Quarto** | Literate programming (`.qmd` reports) |
| **quadprog** | Quadratic programming solver for Kelly QP |
| **tidyquant** | Financial data import (Yahoo Finance) |
| **quantmod** | OHLC data processing |
| **sandwich** / **lmtest** | HAC covariance & t-tests |
| **tidyverse** | Data wrangling & visualization |
| **knitr** | R Markdown table generation |

---

## 📝 Notes

- This project was created to rigorously assess the statistical robustness of Kelly Criterion implementations, particularly challenging claims about aggressive leverage in academic literature.
- All analyses are based on historical data (2000–2018) and do not constitute investment advice.
- Replication requires R with `tidyquant`, `quadprog`, `sandwich`, and related packages.

---

## Authors

Syed Bashir Hydari et al.
