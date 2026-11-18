# Appendix E. Dataset Expansion and Cross-Domain Generality

## E.1 Setup

To verify the generality of Ada-Extractor, we extend the main experiment to other long-text domains—scientific and technical exposition, historical event narration, and sports and culture—sampling 500 subjects per domain from the public title lists of each domain. The evaluation protocol and the metric definitions are identical to those of the main experiment.

## E.2 Pooled Cross-Domain Results

Ada-Extractor ranks first on all three metrics: an Acc of 75.45%, exceeding the runner-up by 7.59 percentage points, together with a FactScore of 94.79% and an EntiScore of 94.59%.

**Table E1. Pooled cross-domain results.**

| Method | Acc (%) | FactScore (%) | EntiScore (%) |
|---|---:|---:|---:|
| CoT | 41.82 | 94.26 | 90.00 |
| Reflexion | 62.16 | 93.88 | 93.25 |
| Atomic Facts | 60.55 | 91.74 | 93.59 |
| EntiTractor | 67.86 | 90.04 | 93.69 |
| **Ada-Extractor (ours)** | **75.45** | **94.79** | **94.59** |
