# PMIX Prediction Market Index — Design Document

## Document Info

**Version**: 1.0.0  
**Publisher**: OMEN  
**Release Date**: August 2026  
**Status**: Preliminary Design Complete  
**License**: MIT License  
**GitHub**: https://github.com/OMEN-PMIX/PMIX


## Table of Contents

### Part I: Foundational Definitions

1. Project Overview
2. Core Index Definitions
3. Index Family Versions
   - 3.1 PMCI (Consensus Index)
   - 3.2 PMVIX (Volatility Index)
   - 3.3 Cross-Type Contract Products

### Part II: Strategy & Applications

4. Trading Strategies
   - 4.1 PMCI-Based Strategies
   - 4.2 PMVIX-Based Strategies
   - 4.3 Combined Strategies (PMCI + PMVIX)
5. Strategy Indices

### Part III: Products & Distribution

6. Data Products
7. On-Chain Products
8. OTC Derivatives
9. Structured Products
10. Index Distribution Formats

### Part IV: Governance & Technology

11. Index Governance Rules
12. Version Transition Mechanism

### Part V: Appendices

13. Appendices
   - A. Calculation Examples
   - B. Glossary
   - C. Relationship with Existing Indices
   - D. Disclaimer


## Part I: Foundational Definitions


## 1. Project Overview

PMIX is a prediction market index project providing two core indices:

| Index | Full Name | Measures |
| :--- | :--- | :--- |
| **PMCI** | Prediction Market Consensus Index | Overall expected direction of events |
| **PMVIX** | Prediction Market Volatility Index | Degree of belief divergence in the market |

PMCI and PMVIX together form a complete state description of prediction markets.


## 2. Core Index Definitions

### 2.1 Underlying Data

| Item | Specification |
| :--- | :--- |
| Data Sources | Polymarket, Kalshi |
| Data Types | Bid-ask midpoint, order book depth, 24h volume, days to expiry |
| Contract Types | Binary, categorical, scalar (all supported) |
| Update Frequency | Every 30 seconds |

### 2.2 Weighting Methods

| Method | Formula | Description |
| :--- | :--- | :--- |
| Geometric Mean (Standard) | $$ w_i = \sqrt{\text{Depth}_i \times \text{Volume}_{24h,i}} $$ | Balances depth and volume |
| Equal Weight | $$ w_i = 1 $$ | Unaffected by liquidity |

### 2.3 Anomaly Handling

| Rule | Handling |
| :--- | :--- |
| Price deviates > 3σ from 30-min moving average | Exclude the contract |
| 24h trading volume < $1,000 | Exclude |
| Order book depth < $5,000 | Exclude |


## 3. Index Family Versions

### 3.1 PMCI (Consensus Index)

**Formula**:

$$
\text{PMCI} = \frac{\sum_i w_i \cdot q_i}{\sum_i w_i}
$$

**Versions**:

| Version | Theme Scope | Weighting | Data Source |
| :--- | :--- | :--- | :--- |
| **PMCI** | All themes | Geometric Mean | Aggregated |
| PMCI-Macro | Economy, Geopolitics, Politics | Geometric Mean | Aggregated |
| PMCI-Political | Elections, Policy | Geometric Mean | Aggregated |
| PMCI-Economic | CPI, Employment, GDP | Geometric Mean | Aggregated |
| PMCI-Geo | War, Conflict | Geometric Mean | Aggregated |
| PMCI-Tech | AI, Regulation | Geometric Mean | Aggregated |
| PMCI-Climate | Climate, Natural Disasters | Geometric Mean | Aggregated |
| PMCI-Crypto | Bitcoin, Ethereum | Geometric Mean | Aggregated |
| PMCI-EW | All themes | Equal Weight | Aggregated |
| PMCI-Poly | All themes | Geometric Mean | Polymarket |
| PMCI-Kalshi | All themes | Geometric Mean | Kalshi |

**Index Interpretation**:

| Value Range | Market State |
| :--- | :--- |
| 0–20 | Extremely unlikely to occur |
| 20–40 | Unlikely to occur |
| 40–60 | No clear tendency |
| 60–80 | Likely to occur |
| 80–100 | Extremely likely to occur |

### 3.2 PMVIX (Volatility Index)

**Formula**:

$$
\text{PMVIX} = \sqrt{365 \times \frac{\sum_i w_i \cdot q_i(1-q_i)}{\sum_i w_i}} \times 10.47
$$

**Versions**:

| Version | Corresponding PMCI | Annualization | Scaling |
| :--- | :--- | :--- | :--- |
| **PMVIX** | PMCI | 365 | 10.47 |
| PMVIX-Macro | PMCI-Macro | 365 | 10.47 |
| PMVIX-Political | PMCI-Political | 365 | 10.47 |
| PMVIX-Economic | PMCI-Economic | 365 | 10.47 |
| PMVIX-Geo | PMCI-Geo | 365 | 10.47 |
| PMVIX-Tech | PMCI-Tech | 365 | 10.47 |
| PMVIX-Climate | PMCI-Climate | 365 | 10.47 |
| PMVIX-Crypto | PMCI-Crypto | 365 | 10.47 |
| PMVIX-EW | PMCI-EW | 365 | 10.47 |
| PMVIX-Poly | PMCI-Poly | 365 | 10.47 |
| PMVIX-Kalshi | PMCI-Kalshi | 365 | 10.47 |
| PMVIX-252 | — | 252 | 12.59 |
| PMVIX-30 | — | 30 | 36.51 |
| PMVIX-7 | — | 7 | 75.59 |
| PMVIX-BP | — | 365 | 1.00 |

**Index Interpretation**:

| Value Range | Market State |
| :--- | :--- |
| 0–20 | Confident with strong consensus |
| 20–40 | Normal uncertainty |
| 40–60 | Rising uncertainty |
| 60–80 | High uncertainty |
| 80–100 | Extreme uncertainty / Crisis |

### 3.3 Cross-Type Contract Products

PMCI provides additional specialized indices for multi-choice and scalar contracts as supplements to the main index.

#### 3.3.1 Multi-Choice Contract Products

| Product | Formula | Description |
| :--- | :--- | :--- |
| PMCI Dispersion Index | $$ 1 - \sum_j q_{ij}^2 $$ | Measures dispersion across options in multi-choice contracts; higher values indicate greater divergence |
| PMCI Multi-Choice Volatility | Weighted variance treating each option as independent binary contract | Belief divergence for multi-choice contracts |

#### 3.3.2 Scalar Contract Products

| Product | Formula | Description |
| :--- | :--- | :--- |
| PMCI Scalar Expected Value | $$ \sum_k q_{ik} \cdot \text{mid}_k $$ | Expected outcome of scalar contracts |
| PMCI Scalar Volatility | $$ \sqrt{\sum_k q_{ik} \cdot (\text{mid}_k - \mu)^2} $$ | Expected volatility of scalar contract outcomes |
| PMCI Scalar Skewness | $$ \frac{\sum_k q_{ik} \cdot (\text{mid}_k - \mu)^3}{\sigma^3} $$ | Expected skewness; positive skew indicates higher probability of extreme high values |


## Part II: Strategy & Applications


## 4. Trading Strategies

### 4.1 PMCI-Based Strategies

| Strategy | Logic | Entry Conditions | Exit Conditions |
| :--- | :--- | :--- | :--- |
| Mean Reversion | Reverse at extremes | Short when PMCI > 70, long when PMCI < 30 | PMCI returns to 0.4–0.6 |
| Range Trading | Buy low, sell high | PMCI approaches 0.3 or 0.7 | Returns to range midpoint |
| Moving Average Trend | 50/200 MA crossover | 50 MA crosses above 200 MA | Reverse crossover |
| Trend Following | Break above / below 200 MA | Go long on breakout above 200 MA | Reverse breakout |
| Thematic Rotation | Relative strength across themes | Specific theme significantly outperforms others | Strength relationship reverses |
| Volatility Adjustment | Adjust exposure with PMVIX | Any time | Continuous adjustment |

### 4.2 PMVIX-Based Strategies

| Strategy | Logic | Entry Conditions | Exit Conditions |
| :--- | :--- | :--- | :--- |
| Tail Hedge | Long OTM Call | PMVIX at historical low percentile | PMVIX spikes above 60 or expiry |
| Volatility Premium Harvesting | Short calls to capture premium | PMVIX at historical mid-high percentile | Options expire or PMVIX spikes |
| Term Structure Trading | Exploit near-term vs far-term spread | Contango too wide | Spread converges |
| Straddle | Long Call + Put | PMVIX at historical low | After significant PMVIX movement |
| Strangle | Long OTM Call + OTM Put | PMVIX at historical low | After significant PMVIX movement |
| Skew Trading | Trade IV differences across strikes | Skew too wide | Skew normalizes |

### 4.3 Combined Strategies (PMCI + PMVIX)

| Strategy | Logic | Entry Conditions | Exit Conditions |
| :--- | :--- | :--- | :--- |
| Dual-Factor | Act when both conditions met | PMVIX < 30 + PMCI > 60 → long; PMVIX < 30 + PMCI < 40 → short | Either condition fails |
| Pairs Trading | Long one / short the other | Relationship deviates from historical average | Relationship reverts to mean |
| Dynamic Allocation | Switch among three assets | Based on relative strength | Continuous adjustment |


## 5. Strategy Indices

| Product | Strategy | Calculation |
| :--- | :--- | :--- |
| PMCI Moving Average Index | 50/200 MA | Daily simple moving average |
| PMCI RSI Index | Relative Strength Index | 14-day RSI |
| PMCI Volatility-Adjusted Index | PMCI + PMVIX | PMCI exposure inversely proportional to PMVIX |
| PMCI Thematic Rotation Index | Theme relative strength | Allocate to strongest theme by 30-day return |
| PMCI Trend Following Index | 200 MA breakout | Long on breakout, short on breakdown |
| PMCI Mean Reversion Index | Reverse at 0.5 deviation | Short when PMCI > 70, long when PMCI < 30 |
| PMCI Deviation Index | Absolute deviation from 0.5 | $$ \| \text{PMCI} - 50 \| \times 2 $$ |
| PMVIX Premium Strategy Index | Systematic short futures | Sell 30-day futures monthly |
| PMVIX Capped Premium Index | Short + long OTM Call | Short futures + OTM Call hedge |
| PMVIX Strangle Strategy Index | Short Call + Put, long OTM Call | Short OTM Call and Put simultaneously |
| PMVIX Tail Hedge Index | Long PMCI + Long PMVIX OTM Call | Simultaneously hold long and call |
| PMVIX Skew Index | IV difference across strikes | Calculate IV difference between 25-delta Put and Call |
| PMVIX Term Structure Index | Contango / Backwardation | Calculate near-term vs far-term spread |


## Part III: Products & Distribution


## 6. Data Products

| Product | Content | Update Frequency |
| :--- | :--- | :--- |
| PMCI Real-Time Data Feed | All PMCI version values | Every 30 seconds |
| PMCI Historical Database | Complete time series | Daily update |
| PMCI Thematic Sentiment Data | Thematic sub-indices | Every 30 seconds |
| PMCI Extreme Consensus Alert | Notification when PMCI > 70 or < 30 | Event-triggered |
| PMVIX Real-Time Data Feed | All PMVIX version values | Every 30 seconds |
| PMVIX Historical Database | Complete time series | Daily update |
| PMVIX Tail Risk Indicator | Percentile, extreme event probability | Every 30 seconds |
| PMVIX Volatility Warning | Spike speed alert | Event-triggered |
| PMCI + PMVIX Combined Data Feed | Real-time comparison | Every 30 seconds |
| Sentiment Dashboard | Web visualization interface | Real-time |


## 7. On-Chain Products

| Product | Mechanism | Technology |
| :--- | :--- | :--- |
| PMCI Oracle Price Feed | Push PMCI values on-chain | Chainlink / UMA |
| PMVIX Oracle Price Feed | Push PMVIX values on-chain | Chainlink / UMA |
| PMCI Index Token | ERC-20 token tracking PMCI | Smart Contract |
| PMVIX Insurance Pool | Stake USDC, payout when PMVIX spikes | Smart Contract |
| PMCI Mean Reversion Vault | Automated PMCI range trading | On-Chain Vault |


## 8. OTC Derivatives

| Product | Mechanism | Settlement |
| :--- | :--- | :--- |
| PMCI Forward Contract | Agree on future PMCI price | PMCI SOQ at expiry |
| PMCI Contract for Difference (CFD) | Trade price movement | Daily mark-to-market |
| PMVIX Volatility Forward | Agree on future PMVIX level | PMVIX SOQ at expiry |
| PMVIX Volatility Swap | Exchange fixed vs realized volatility | Realized volatility settlement |
| PMVIX Variance Swap | Exchange fixed vs realized variance | Realized variance settlement |
| PMCI/PMVIX Correlation Swap | Trade correlation between both | Realized correlation settlement |

**Special Opening Quote (SOQ)**:

| Item | Specification |
| :--- | :--- |
| PMCI SOQ | Volume-weighted average index value on expiry date at 9:30–9:45 AM ET |
| PMVIX SOQ | Volume-weighted average index value on expiry date at 9:30–9:45 AM ET |


## 9. Structured Products

| Product | Mechanism |
| :--- | :--- |
| PMCI Principal-Protected Note | 100% principal return + participation in PMCI movement |
| PMCI Principal-Protected Enhanced Note | Principal return + leveraged participation in PMCI movement |
| PMCI Autocallable Note | Early redemption + premium if observation > barrier |
| PMCI Range Accrual Note | Interest accrues when PMCI stays within range |
| PMCI Contingent Coupon Note | Coupon paid if observation > barrier |
| PMCI Jump Note | Leveraged participation if movement exceeds threshold at expiry |
| PMCI Dual-Directional Jump Note | Leveraged participation if deviation from 0.5 exceeds threshold |
| PMVIX Volatility Note | Linked to PMVIX |


## 10. Index Distribution Formats

| Format | Content | Update Frequency |
| :--- | :--- | :--- |
| REST API | JSON real-time values | Every 30 seconds |
| WebSocket Push | Real-time data stream subscription | Real-time |
| Web Dashboard | Real-time values and historical chart | Real-time |
| Historical Data Download | CSV / JSON | Daily update |


## Part IV: Governance & Technology


## 11. Index Governance Rules

| Rule | Description |
| :--- | :--- |
| Open Rules | All calculation rules are fully documented in this design document and public repository |
| Version Locking | Published versions cannot be retroactively modified |
| Reproducibility | Anyone can independently calculate using the formulas |
| Interruption Response | If data source API changes, suspend publication and explain publicly |
| Contract Type Extension | New contract types are incorporated per this framework |


## 12. Version Transition Mechanism

| Step | Description |
| :--- | :--- |
| Announcement | 30-day notice before new version release |
| Parallel Running | Old and new versions run simultaneously for 30 days |
| Labeling | Historical data marked with version number |
| Effective Date | New version applies only to data after announcement date |
| No Retroactive Changes | Published historical data is never retroactively modified |


## Part V: Appendices


## 13. Appendices

### Appendix A: Calculation Examples

| Contract | \( q \) | Depth | Volume | Weight |
| :--- | :--- | :--- | :--- | :--- |
| A | 0.50 | 100,000 | 50,000 | 70,710 |
| B | 0.60 | 80,000 | 60,000 | 69,282 |
| C | 0.70 | 50,000 | 40,000 | 44,721 |

**PMCI Calculation**:

$$
\text{PMCI} = \frac{70,710 \times 0.50 + 69,282 \times 0.60 + 44,721 \times 0.70}{184,713} \approx 58.6
$$

**PMVIX Calculation**:

$$
\text{PMVIX} = \sqrt{365 \times \frac{70,710 \times 0.25 + 69,282 \times 0.24 + 44,721 \times 0.21}{184,713}} \times 10.47 \approx 97.3
$$

**Interpretation**:
- PMCI ≈ 58.6: Market leans toward events occurring
- PMVIX ≈ 97.3: Extreme uncertainty, market deeply divided on near-term events

### Appendix B: Glossary

| Term | Definition |
| :--- | :--- |
| PMIX | Project name, Prediction Market Index |
| PMCI | Prediction Market Consensus Index |
| PMVIX | Prediction Market Volatility Index |
| SOQ | Special Opening Quote |
| OTM | Out-of-The-Money |
| ATM | At-The-Money |
| Contango | Far-term futures price > near-term futures price |
| Backwardation | Near-term futures price > far-term futures price |

### Appendix C: Relationship with Existing Indices

PMCI shares similar objectives with existing prediction market indices in measuring overall consensus direction. Key differences:

| Dimension | Existing Indices | PMCI |
| :--- | :--- | :--- |
| Weighting Method | Fixed weighting | Provides geometric mean and equal weight versions |
| Contract Types | Binary only | Supports binary, categorical, and scalar |
| Index System | Single index | Forms complete index system with PMVIX |
| Licensing | Proprietary | MIT open-source, calculation rules public and reproducible |

PMVIX and traditional volatility indices share the same calculation logic (extracting second-order information from market prices), but differ in underlying assets:

| Dimension | Traditional Volatility Index | PMVIX |
| :--- | :--- | :--- |
| Underlying Asset | Options market | Prediction market |
| Measured Object | Equity market volatility | Belief volatility |
| Update Frequency | Real-time | Real-time |
| Value Range | 0–100 | 0–100 |

### Appendix D: Disclaimer

This PMIX design document is for educational and research purposes only. All designs are independently developed and open-sourced under the MIT License. This document does not constitute investment advice. The indices depend on public data from Polymarket and Kalshi; if these data sources stop service, the indices may suspend publication. The author assumes no responsibility for any investment decisions or losses.
