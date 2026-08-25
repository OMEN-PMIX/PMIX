# PMIX Prediction Market Index

**Version**: 1.0.0  
**License**: MIT License  
**GitHub**: https://github.com/OMEN-PMIX/PMIX

---

## Introduction

PMIX is a prediction market index project providing two core indices:

| Index | Full Name | Measures |
| :--- | :--- | :--- |
| **PMCI** | Prediction Market Consensus Index | Overall expected direction of events |
| **PMVIX** | Prediction Market Volatility Index | Degree of belief divergence in the market |

Together, they form a complete state description of prediction markets.

---

## Index Family

### PMCI (Consensus Index)

A weighted average of real-time prices of multiple contracts in prediction markets, reflecting the market's overall judgment on the "likelihood of events occurring."

**Versions**: Standard, Macro, Political, Economic, Geopolitical, Tech, Climate, Crypto, Equal Weight, Polymarket, Kalshi

### PMVIX (Volatility Index)

A weighted compression of the "degree of belief divergence" across multiple contracts in prediction markets into a continuous value, reflecting the market's collective uncertainty.

**Versions**: Standard, Macro, Political, Economic, Geopolitical, Tech, Climate, Crypto, Equal Weight, Polymarket, Kalshi, Trading Day Annualized, Monthly Annualized, Weekly Annualized, Basis Point

---

## Product Ecosystem

| Category | Count | Description |
| :--- | :--- | :--- |
| Index Versions | 26 | 11 PMCI + 15 PMVIX |
| Data Products | 10 | Real-time data feeds, historical databases, alert systems, dashboard |
| On-Chain Products | 5 | Oracle price feeds, index tokens, insurance pools, vaults |
| OTC Derivatives | 6 | Forwards, CFDs, volatility swaps, variance swaps, correlation swaps |
| Structured Products | 8 | Principal-protected notes, autocallable notes, jump notes, etc. |
| Strategy Indices | 13 | Moving average, RSI, skew, term structure, etc. |
| Trading Strategies | 15 | PMCI / PMVIX based strategies |

**For detailed specifications, see: [Design Document](./WHITEPAPER_en.md)**

---

## Core Formulas

### PMCI

$$
\text{PMCI} = \frac{\sum_i w_i \cdot q_i}{\sum_i w_i}
$$

### PMVIX

$$
\text{PMVIX} = \sqrt{365 \times \frac{\sum_i w_i \cdot q_i(1-q_i)}{\sum_i w_i}} \times 10.47
$$

### Weighting Method (Standard)

$$
w_i = \sqrt{\text{Depth}_i \times \text{Volume}_{24h,i}}
$$

---

## Index Interpretation

### PMCI

| Value | Market State |
| :--- | :--- |
| 0–20 | Extremely unlikely to occur |
| 20–40 | Unlikely to occur |
| 40–60 | No clear tendency |
| 60–80 | Likely to occur |
| 80–100 | Extremely likely to occur |

### PMVIX

| Value | Market State |
| :--- | :--- |
| 0–20 | Confident with strong consensus |
| 20–40 | Normal uncertainty |
| 40–60 | Rising uncertainty |
| 60–80 | High uncertainty |
| 80–100 | Extreme uncertainty |

---

## Repository Structure

```
PMIX/
├── WHITEPAPER.md          # Design Document
├── README.md              # Project Overview
└── LICENSE                # MIT License
```

---

## License

MIT License © 2026 OMEN

---

## Disclaimer

This PMIX design document is for educational and research purposes only. This document does not constitute investment advice. The indices depend on public data from Polymarket and Kalshi; if these data sources stop service, the indices may suspend publication. The author assumes no responsibility for any investment decisions or losses.
