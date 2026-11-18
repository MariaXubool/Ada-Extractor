# Appendix B. Empirical Analysis of Entity-Anchor Concentration

## B.1 Setup

We sample 96 coherent passages from Simple English Wikipedia (20231101 snapshot), spanning 4 domains × 4 length bins × 6 passages (150–899 words). Each passage undergoes exactly two independent LLM calls: Round 1 returns the Top-3 entity anchors and an adaptive anchor count $\hat{k}$; Round 2, blind to Round 1, extracts all entities, atomic facts, and fact–entity links. Entity–fact matching is performed deterministically in Python, and the random baseline is the expected coverage of three uniformly drawn entities (200 Monte-Carlo draws per passage). $C_3$ denotes the fraction of atomic facts covered by the Top-3 anchors, and $D_F$ the number of facts per 100 words.

## B.2 Coverage and Concentration by Length

**Table B1. Aggregation by length bin (n = 24 per bin).**

| Length (words) | E | F | $F_3$ | $C_3$ (%) | Random 3 (%) | $\hat{k}$ |
|---|---:|---:|---:|---:|---:|---:|
| 150–249 | 8.2 | 16.8 | 10.1 | 65.3 | 44.8 | 2.96 |
| 250–399 | 18.4 | 29.5 | 18.8 | 66.5 | 28.5 | 2.92 |
| 400–599 | 22.9 | 37.2 | 23.5 | 62.5 | 24.6 | 3.29 |
| 600–899 | 25.2 | 51.7 | 29.9 | 56.8 | 19.0 | 3.29 |

Across all 96 passages, the Top-3 anchors cover **62.8%** of the atomic facts on average (random baseline 29.2%; gain of +33.6 percentage points), and the pattern holds in every domain. Concentration strengthens with passage length: the random baseline drops from 44.8% to 19.0%, while the anchor gain grows monotonically from +20.5 to +38.0 percentage points (Spearman $\rho = 0.232$, $p = 0.023$). In contrast, the HHI decreases with length ($\rho = -0.303$, $p = 0.003$), indicating that facts in longer passages are distributed over a larger number of entities.

## B.3 Correlation Analysis

**Table B2. Spearman correlations (n = 96).**

| Relationship | $\rho$ | $p$ |
|---|---:|---:|
| $\hat{k}$ ~ word count | 0.288 | 0.004 |
| $\hat{k}$ ~ HHI | −0.247 | 0.018 |
| $\hat{k}$ ~ entity count E | 0.125 | 0.224 |
| $C_3$ ~ HHI | 0.413 | <0.001 |
| $C_3$ − random ~ word count | 0.232 | 0.023 |
| HHI ~ word count | −0.303 | 0.003 |

The adaptive anchor count $\hat{k}$ correlates significantly with word count ($\rho = 0.288$, $p = 0.004$) and with the number of effective semantic centers, and negatively with HHI ($\rho = -0.247$, $p = 0.018$). Its correlation with entity count E is not significant ($\rho = 0.125$, $p = 0.224$), and no significant association with fact density is observed. This suggests that the number of anchors is governed by semantic organization rather than surface size, consistent with our design. Moreover, $C_3$ correlates positively with HHI ($\rho = 0.413$, $p < 0.001$): the more strongly facts concentrate on a few entities, the higher the coverage achieved by the Top-3 anchors.

