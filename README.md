# Courtside

> NBA player props forecasting and market-pricing engine.
> **Source code is private** — this repository hosts the architecture, methodology, and validated backtest results.

## What it does

Probabilistic forecasting layer over NBA player prop markets. For a given matchup, Courtside predicts the distribution of a player's stat (points, assists, rebounds, etc.), prices the bookmaker's line against that distribution, and surfaces +EV wagers via tiered confidence gating.

## Tech stack

- **Modeling:** Normal CDF + Poisson distributions, temperature-scaled calibration
- **Compute:** Python (Pandas, NumPy, XGBoost) with a Rust + PyO3 hot path (Rayon parallelism) for backtest acceleration
- **Data:** SQLite store; 4.8M+ odds snapshots across 4 sportsbooks, 8 stat categories, 223 game days
- **Validation:** Walk-forward CV, 151K+ backtest observations, 253K+ closing-line records

## Architecture

```
   Sportsbook Scrapers  ──▶  Odds Snapshots DB (4.8M)
                                       │
                                       ▼
   Feature Engineering Layer  ──▶  Player Stat Models
                                       │
                                       ▼
   Probability Distribution  ──▶  Line Pricer  ──▶  EV Computer
                                                        │
                                                        ▼
                                  Tiered Signal Gating (bin-level filtering)
                                                        │
                                                        ▼
                                  Wager Selection (+EV only)
```

## Results — walk-forward validation

| Metric | Value |
|---|---|
| Wagers (gated) | 1,138 |
| Hit rate | **69.7%** |
| ROI | **+33%** |
| Backtest observations | 151K+ |
| Closing-line records | 253K+ |
| Odds snapshots | 4.8M+ |
| Sportsbooks covered | 4 |
| Stat categories | 8 |
| Game days | 223 |

### Charts

> _Add screenshots from the live system here:_
>
> - [ ] Calibration plot — predicted vs. observed probability across 10 bins
> - [ ] ROI by confidence bin (bar chart)
> - [ ] Cumulative profit curve over walk-forward windows
> - [ ] Hit-rate convergence vs. sample size
> - [ ] Coverage map: stat-category × sportsbook heatmap

## Why source is private

The private repository protects:
- Sportsbook scraper implementations
- Signal-gating thresholds and bin definitions
- Calibration parameters and feature weights

Happy to walk through implementation details and design decisions in an interview.

## Contact

Ronil Basu — Rutgers University–New Brunswick, BS Data Science
[linkedin.com/in/ronil-basu](https://linkedin.com/in/ronil-basu) · ronilbasu@gmail.com
