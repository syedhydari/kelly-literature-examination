# Kelly Criterion | Expansion and Reassessment

**May 2026 — STAT GR 5261 Statistical Methods in Finance:** Columbia class project examining Kelly Criterion literature and implementation claims via Monte Carlo validation, replication logic, constraint diagnostics, and HAC inference.

## Project Context

- **Date:** May 2026
- **Course:** STAT GR 5261 — Statistical Methods in Finance
- **Institution:** Columbia University
- **Project Type:** Graduate class project / literature examination
- **Author:** Syed Bashir Hydari et al.
- **Role:** Engine/source code, final paper, presentation, speaker
- **Research Area:** Kelly Criterion, portfolio optimization, estimation risk, fractional sizing, constraint effects, and statistical inference

## Overview

This project examines the practical limits of Kelly Criterion portfolio optimization, with emphasis on estimation noise, leverage sensitivity, drawdown risk, and the statistical robustness of aggressive Kelly variants.

The central finding is that apparent outperformance from aggressive Kelly multiples can be driven by **constraint collapse**: under long-only budget constraints, Full, Double, and Triple Kelly may collapse onto the same feasible portfolio, making naive performance comparisons misleading.

## Methods

- Monte Carlo simulation under known GBM parameters
- Replication and reassessment of core Kelly portfolio literature
- Long-only and unconstrained Kelly optimization
- EuroStoxx 50 and Dow Jones robustness checks
- Newey-West HAC tests on daily return differences
- Mean-vs-median terminal wealth analysis
- Drawdown and leverage sensitivity diagnostics

## Key Result

Formal testing weakens the case for aggressive Kelly variants. Newey-West HAC tests show that the main statistically significant gap is Half Kelly vs Full Kelly, while Kelly-vs-MinVar comparisons fail to reject. When leverage constraints are lifted, unconstrained Kelly variants suffer severe drawdowns and unstable out-of-sample behavior.

The practical conclusion is conservative: fractional Kelly is more defensible than aggressive Kelly multiples when estimation noise, constraints, and drawdown risk are taken seriously.

## Reproducibility Note

This repository contains selected academic materials from a Columbia Statistical Methods in Finance project. It is not investment advice and does not contain production trading infrastructure.
