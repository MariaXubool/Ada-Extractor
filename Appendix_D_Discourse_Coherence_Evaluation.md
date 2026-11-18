# Appendix D. Human Evaluation of Discourse Coherence

## D.1 Setup

To assess the ability of Ada-Extractor to preserve discourse coherence during long-form factual hallucination correction, we design a blinded human evaluation focusing on cross-sentence semantic relationships and overall narrative consistency. We randomly select 100 biographies from the 500 instances of the corpus and collect the final corrected outputs of Atomic Facts, EntiTractor, and Ada-Extractor, together with the uncorrected original passages, resulting in 400 passages to be evaluated. Three independent annotators who are not involved in algorithm development evaluate the complete passages using a standardized five-point rating scale. All passages are anonymized and randomly presented, and the annotators are unaware of the underlying methods and research hypotheses, which minimizes evaluation bias and ensures fair comparisons.

## D.2 Evaluation Criteria

Discourse coherence measures the consistency of cross-sentence semantic relationships and the overall narrative organization. We assess four aspects: entity referential continuity, topical continuity, logical consistency, and narrative ordering. Each passage is evaluated on a five-point Likert scale, as specified in Table D1.

**Table D1. Human rating guidelines for discourse coherence.**

| Score | Evaluation Criteria |
|:---:|---|
| 1 | Severe semantic discontinuities, ambiguous references, and a largely incomprehensible narrative organization. |
| 2 | Noticeable topic shifts or logical inconsistencies, resulting in a weak discourse organization. |
| 3 | Generally coherent, with occasional ambiguous references or insufficient semantic transitions. |
| 4 | Clear cross-sentence relationships and natural topic progression, with only minor coherence issues. |
| 5 | Highly coherent discourse with consistent references, logical relationships, and a well-organized narrative progression. |

Annotators evaluate complete passages rather than isolated sentences, and they do not consider external factual correctness when assigning a coherence score.

## D.3 Results and Analysis

We measure discourse coherence by the average rating of the three annotators and quantify the change relative to the original passage as $\Delta C_m = C_m - C_{\mathrm{Original}}$, where $C_m$ and $C_{\mathrm{Original}}$ denote the average coherence scores of the corrected and the original passages, respectively. Table D2 reports the results, in which Ada-Extractor obtains a coherence score of 4.24, outperforming Atomic Facts (3.94) and EntiTractor (4.16) while exhibiting a smaller coherence reduction relative to the original text. This trend is consistent with our methodological motivation: organizing related factual descriptions around entity anchors alleviates the semantic fragmentation caused by fine-grained verification, and adaptive anchor selection further reduces unnecessary disruptions to the discourse structure. The evaluation is intended to provide evidence of coherence preservation that is independent of factual accuracy, complementing the factuality-oriented experiments in the main paper.

**Table D2. Discourse coherence scores across correction methods.**

| Method | Coherence ↑ | $\Delta$ Coherence ↑ |
|---|---:|---:|
| Original | 4.36 | 0.00 |
| Atomic Facts | 3.94 | −0.42 |
| EntiTractor | 4.16 | −0.20 |
| **Ada-Extractor (ours)** | **4.24** | **−0.12** |
