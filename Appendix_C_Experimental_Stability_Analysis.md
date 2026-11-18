# Appendix C. Experimental Stability Analysis

## C.1 Setup

To evaluate the reliability and reproducibility of our results, we conduct 20 independent runs for all methods on the same 500 biographies using 20 different random seeds, while keeping the experimental settings and the evaluation protocol identical across runs.

## C.2 Across-Run Standard Deviations

Table C1 reports the across-run standard deviations (SD) of Acc, FactScore, and EntiScore for each method.

**Table C1. Standard deviations across 20 independent runs (percentage points).**

| Method | Acc | FactScore | EntiScore |
|---|---:|---:|---:|
| CoT | 0.87 | 0.40 | 0.57 |
| Reflexion | 0.74 | 0.48 | 0.55 |
| Atomic Facts | 0.84 | 0.32 | 0.49 |
| EntiTractor | 0.52 | 0.41 | 0.60 |
| **Ada-Extractor (ours)** | **0.78** | **0.53** | **0.56** |

All methods exhibit low across-run variance, with standard deviations remaining below 1 percentage point on all three metrics. Ada-Extractor shows a slightly larger FactScore SD (0.53) than most baselines, yet every SD reported here is an order of magnitude smaller than the corresponding inter-method gaps discussed in the main paper, so the ranking of methods is not sensitive to the choice of random seed.
