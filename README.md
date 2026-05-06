# Kelly Criterion: Expansion and Reassessment

**Reproduction and Statistical Reassessment of Kelly Literature**

A comprehensive research project examining the practical implementation of the Kelly Criterion, replicating and critically assessing findings from Carta & Conversano (2020).

---

## 📋 Project Overview

This repository contains a rigorous statistical and computational examination of the Kelly Criterion—a growth-optimal asset allocation framework with well-known sensitivity to estimation noise. The research:

- **Validates** Kelly geometry under known parameters via Monte Carlo simulation
- **Replicates** Carta & Conversano's (2020) multi-asset optimization pipeline on real market data
- **Challenges** the statistical robustness of aggressive Kelly variants using formal hypothesis tests
- **Demonstrates** how long-only budget constraints mechanically collapse seemingly different strategies

### Key Insight

The apparent outperformance of aggressive Kelly multiples (Double, Triple) over standard Kelly is an **artifact of constraint collapse**—when the long-only constraint $\sum w_i = f \le 1$ binds for $f \ge 1$, Full/Double/Triple Kelly collapse onto an identical portfolio, making statistical comparison meaningless without formal testing.

---

## 📂 Repository Structure

```
kelly-literature-examination/
├── README.md                          # This file
├── docs/
│   ├── Final_Report.qmd               # Main research report (Quarto/R Markdown)
│   ├── Final_Report.pdf               # Compiled main report
│   ├── Final_Report_Appendix.qmd      # Extended appendix with full details
│   └── Final_Report_Appendix.pdf      # Compiled appendix
└── figures/
    ├── fig-A1-1.pdf                   # Monte Carlo validation plots
    ├── fig-A2-1.pdf                   # Banca Intesa backtest results
    ├── fig-A3-1.pdf                   # In-sample EuroStoxx 50 analysis
    ├── fig-A4-1.pdf                   # Out-of-sample long-only Kelly variants
    ├── fig-A5-1.pdf                   # Unconstrained Kelly performance
    ├── fig-A6-1.pdf                   # Summary metrics comparison
    └── fig-A7-1.pdf                   # Dow Jones Industrial Average backtest
```

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

## 🔬 Methodology

### Data
- **35 surviving EuroStoxx 50 constituents** (daily adjusted closes)
- **Period**: January 2000 – December 2018 (19 years)
- **Filtering**: 20% missing-data threshold, forward-fill imputation
- **Source**: Yahoo Finance via `tidyquant` and `quantmod` R packages

### Kelly Optimization Framework
The multi-asset Kelly Quadratic Program:
$$\max_{\mathbf{w}} \; \mathbf{w}^\top(\boldsymbol{\mu} - r_f \mathbf{1}) - \tfrac{1}{2}\mathbf{w}^\top \boldsymbol{\Sigma} \mathbf{w}$$

Subject to:
- $\mathbf{1}^\top \mathbf{w} = f$ (budget constraint with $f \in \{0.5, 1, 2, 3\}$)
- $w_i \ge 0$ (long-only) *or* unconstrained with leverage cap $|f| \le 3$

Solver: `quadprog` R package (interior-point method)

### Kelly Variants
- **Half Kelly** ($f = 0.5$): Conservative, reduced leverage
- **Full Kelly** ($f = 1$): Growth-optimal, "bet the whole account"
- **Double Kelly** ($f = 2$): Aggressive, $2\times$ leverage
- **Triple Kelly** ($f = 3$): Ultra-aggressive, $3\times$ leverage

### Statistical Tests
- **HAC (Heteroskedasticity & Autocorrelation Consistent)**: Newey-West, 5 lags
- **Null Hypothesis**: Mean daily return difference = 0
- **Alternative**: Two-tailed at $\alpha = 0.05$

---

## 📈 Main Results Summary

### Monte Carlo Validation (10,000 trades, 1,000 paths)

| Strategy | Mean | Median | Std Dev | P(Loss) | P(W>2) | P(W>10) |
|----------|------|--------|---------|---------|--------|---------|
| Half Kelly | 7.03 | 4.54 | 9 | 4.0% | 83.6% | 20.0% |
| Full Kelly | 31.47 | 6.89 | 94 | 12.3% | 76.3% | 41.7% |
| Double Kelly | 183.80 | 1.67 | 1,803 | 44.5% | 48.3% | 29.5% |
| Triple Kelly | 518.50 | 0.01 | 11,867 | 79.0% | 17.5% | 10.8% |

**Interpretation**: Jensen's inequality ($\mathbb{E}[\log W] < \log \mathbb{E}[W]$) widens with leverage.

### Out-of-Sample Long-Only Performance (EuroStoxx 50, 2005–2018)

| Strategy | Sharpe | Max DD | Ann Return | Ann Vol | Sortino | CVaR (95%) |
|----------|--------|--------|-----------|---------|---------|-----------|
| Half Kelly | 0.556 | 35.2% | 7.87% | 14.15% | 0.835 | 0.0195 |
| Full Kelly | 0.621 | 63.9% | 16.87% | 27.17% | 0.925 | 0.0380 |
| Double Kelly | 0.621 | 63.9% | 16.87% | 27.17% | 0.925 | 0.0380 |
| Triple Kelly | 0.621 | 63.9% | 16.87% | 27.17% | 0.925 | 0.0380 |
| MinVar | 0.480 | 58.0% | 8.55% | 17.79% | 0.685 | 0.0261 |

**Interpretation**: Full/Double/Triple are **identical**—constraint collapse in action.

### HAC Pairwise Tests (5 lags, long-only OOS)

| Pair | Mean Diff | HAC SE | t-stat | p-value | Significant? |
|------|-----------|--------|--------|---------|--------------|
| Half Kelly vs Full Kelly | −0.0004 | 0.0001 | −2.4646 | **0.0138** | ✓ Yes |
| Half Kelly vs MinVar | 0.0000 | 0.0002 | −0.1785 | 0.8584 | ✗ No |
| Full Kelly vs MinVar | 0.0003 | 0.0002 | 1.4736 | 0.1407 | ✗ No |

**Interpretation**: Only Half-vs-Full is statistically significant—a constraint artifact, not true optimization gain.

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

## 💡 Practical Takeaways for Practitioners

1. **Aggressive Kelly multiples are not "better"—they're different risk profiles.** Comparing mean terminal wealth without accounting for variance is misleading. Use formal hypothesis tests.

2. **Long-only constraints eliminate the leverage benefit.** If you can't short or leverage, Full/Double/Triple Kelly are mathematically identical. Don't "choose" between them; any selection is arbitrary.

3. **Mean-vs-median matters in the real world.** A strategy with 31.47× mean but 0.01× median (Triple Kelly) will bankrupt you before reaching the mean. Median wealth is your actual experience.

4. **Estimation noise kills unconstrained Kelly.** Without perfect parameter knowledge, unlimited leverage amplifies errors catastrophically. Half or Full Kelly with long-only constraints is far more robust.

5. **Always conduct statistical inference, not just backtests.** Cumulative wealth plots and Sharpe ratios can obscure what formal hypothesis testing reveals: many "improvements" are noise.

---

## 📚 References

- **Breiman, L.** (1961). Optimal gambling systems for favorable games. *Proceedings of the Fourth Berkeley Symposium on Mathematical Statistics and Probability*, 1, 65–78.

- **Carta, A., & Conversano, C.** (2020). Practical implementation of the Kelly Criterion. *Frontiers in Applied Mathematics and Statistics*, 6, 577050.

- **Cover, T. M.** (1991). Universal portfolios. *Mathematical Finance*, 1(1), 1–29.

- **Kelly, J. L.** (1956). A new interpretation of information rate. *Bell System Technical Journal*, 35(4), 917–926.

- **MacLean, L. C., Thorp, E. O., & Ziemba, W. T.** (Eds.). (2011). *The Kelly Capital Growth Investment Criterion*. World Scientific.

- **Newey, W. K., & West, K. D.** (1987). A simple, positive semi-definite, heteroskedasticity and autocorrelation consistent covariance matrix. *Econometrica*, 55(3), 703–708.

- **Thorp, E. O.** (2011). The Kelly criterion in blackjack, sports betting, and the stock market. In *The Kelly Capital Growth Investment Criterion* (pp. 789–832). World Scientific.

---

## 👥 Contributors

| Name | Role |
|------|------|
| **Syed Bashir Hydari** | Core reproduction, source code, final paper, speaker |
| **Aniqa Nayim** | Core reproduction, source code, final paper, speaker |

---

## 📄 Report Files

### Main Report
- **`Final_Report.qmd`** – Quarto R Markdown source
- **`Final_Report.pdf`** – Compiled PDF (~70 KB)
  - Includes introduction, methodology, results, discussion, conclusion
  - Contains embedded tables and references

### Appendix
- **`Final_Report_Appendix.qmd`** – Extended technical appendix source
- **`Final_Report_Appendix.pdf`** – Full appendix (~891 KB)
  - Detailed figure layouts, additional analyses, supplementary tables

---

## 🔍 How to Use This Repository

1. **Read the Research**: Start with `Final_Report.pdf` for the executive summary
2. **Explore the Analysis**: Open `Final_Report_Appendix.pdf` for full technical details
3. **Review the Code**: Check `Final_Report.qmd` and `Final_Report_Appendix.qmd` for reproducible source code
4. **View the Figures**: See `figures/` directory for all plots and visualizations

---

## 📝 Notes

- This project was created to rigorously assess the statistical robustness of Kelly Criterion implementations, particularly challenging claims about aggressive leverage in academic literature.
- All analyses are based on historical data (2000–2018) and do not constitute investment advice.
- Replication requires R with `tidyquant`, `quadprog`, `sandwich`, and related packages.

---

## 📧 Questions?

For inquiries about this research, contact the authors via GitHub or email.

---

**Last Updated**: May 2026  
**Repository**: [syedhydari/kelly-literature-examination](https://github.com/syedhydari/kelly-literature-examination)
