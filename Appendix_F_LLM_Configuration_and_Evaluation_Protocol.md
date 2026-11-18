# Appendix F. LLM Configuration and Factuality Evaluation Protocol

## F.1 Language Models and Decoding Parameters

To facilitate reproducibility, a unified large language model (LLM) is used for text generation, entity extraction, factual verification, and revision, while an independent evaluator assesses the factual correctness of the outputs. The reference implementation uses **Llama-3.1-8B-Instruct** for generation and correction and **GPT-6** for factuality evaluation.

**Table F1. Decoding configurations of the generation model.**

| Parameter | Llama-3.1-8B-Instruct |
|---|---:|
| Temperature | 0.6 |
| Top-p | 0.9 |
| Frequency penalty | 0.0 |
| Presence penalty | 0.0 |
| Max tokens | 1024 |

The generation model employs a nonzero temperature and nucleus sampling to balance linguistic diversity and factual consistency, whereas the evaluator runs with a low-randomness configuration to improve scoring consistency.

## F.2 Factual Verification Sources and Criteria

The factuality evaluation follows the biographical data curation strategy of Min et al. (2023), involving 500 individuals from diverse occupational categories, including science, arts, and sports. The subjects can be linked to Wikidata entities and to the corresponding Wikipedia pages, and previously annotated references support the factuality assessment.

During correction, factual verification primarily relies on the parametric knowledge of the generation model and does not require external retrieval. During independent evaluation, GPT-6 assesses the final text, and the evaluation prompt permits authoritative web searches. Judgments are categorized as `Correct`, `Error (Entity Error)`, `Error (Description Error)`, or `Unverifiable`.

## F.3 Structured Output Specification for Generation and Correction

To improve machine-readability, the model is instructed to return an XML-formatted response that reports entity extraction, fact decomposition, factual verification, and final text reconstruction.

**Table F2. Structured output fields.**

| Field | Description |
|---|---|
| `entities` | Core entities and their semantic categories |
| `fact_decomposition` | Entity-associated descriptions and their basis in the source text |
| `verification` | Factuality judgments and the corresponding explanations |
| `final_biography` | Revised and reconstructed text |

```xml
<output_structure>
  <entities>
    Entity 1: ...
    Entity 2: ...
  </entities>
  <fact_decomposition>
    Entity | Description | Basis
  </fact_decomposition>
  <verification>
    Entity | Status | Note
  </verification>
  <final_biography>
    Revised biography
  </final_biography>
</output_structure>
```

For the verification of an individual sub-passage, the intermediate response contains `is_factual` (Boolean) and `error_details` (string). When an error is detected, the associated explanation and the original passage are jointly provided to the correction prompt.

## F.4 Relevant Prompts

```text
You are a biography revision expert. Based on the input biography and the candidate entities extracted in the previous round, generate a revised biography that is better suited for entity-level factuality evaluation.

Core objectives:
- Rewrite the biography around "core entity - associated facts" pairs.
- You must produce exactly {target_entity_num} scoring_entities, and the final_biography must cover the facts associated with all {target_entity_num} entities.

Important notes:
- This method does NOT decompose the text into atomic facts; instead, facts are merged into core-entity slots.
- For example:
  - Multiple film works may be merged into "notable works"
  - Date of birth and birthplace may be merged into "birth information"
  - Multiple awards may be merged into "awards and honors"
  - Profession, identity, and field may be merged into "occupation and identity"

Rules:
1. The output must be strictly valid XML. Do not output anything outside the XML.
2. You must output exactly {target_entity_num} <entity> elements.
3. If the candidate entities exceed {target_entity_num}, merge similar entities; do not simply discard important facts.
4. If the candidate entities are fewer than {target_entity_num}:
   - If the person is a clearly identified individual, you may fill the gaps with high-confidence model knowledge;
   - Whenever model knowledge is used, the source must be marked as model_knowledge or mixed;
   - Never fabricate uncertain birth dates, specific awards, or specific works.
5. Only if the previous round's validity is invalid_unknown AND the person cannot be linked to any clearly identified individual are you allowed to skip generating a concrete biography.
6. If the person is clearly identified, generate a conservative biography even when the input is a clarification request or a refusal; never output an empty final_biography.
7. final_biography must be pure biographical prose. It must NOT contain:
   - "I'd be happy to help"
   - "Here's a biography"
   - task explanations
   - nested XML
   - multiple candidate biographies
8. The language of final_biography should match the dominant language of the input.
9. Preserve the original spelling of all proper nouns.
10. Do not introduce relation errors, e.g., rewriting "the parents are French" as "the person is French."
11. final_biography must naturally cover all scoring_entities.

<person>
{person}
</person>
<input_biography>
{biography}
</input_biography>
<previous_extraction>
{response1}
</previous_extraction>

Return the result in the following XML format:
<result>
<validity>valid / weak_valid / invalid_but_person_known / invalid_unknown</validity>
<scoring_entities>
<entity id="1">
<name>entity or entity-slot name</name>
<type>name / birth / nationality / occupation / work / award / event / organization / relation / place / other</type>
<fact>the accurate fact associated with this entity</fact>
<source>input_evidence / model_knowledge / mixed</source>
</entity>
<entity id="2">
<name>...</name>
<type>...</type>
<fact>...</fact>
<source>...</source>
……
</entity>
<entity id={target_entity_num}>
<name>...</name>
<type>...</type>
<fact>...</fact>
<source>...</source>
</entity>
</scoring_entities>
<coverage_check>
<item>explain how final_biography covers entity 1</item>
<item>explain how final_biography covers entity 2</item>
<item>explain how final_biography covers entity 3</item>
……
<item>explain how final_biography covers entity {target_entity_num} </item>
</coverage_check>
<knowledge_usage>
<item>write none if no model knowledge was used; otherwise itemize each use</item>
</knowledge_usage>
<final_biography>pure biographical prose only; no greetings, no task explanation, no nested XML tags</final_biography>
</result>
```

## References

Min, S., Krishna, K., Lyu, X., Lewis, M., Yih, W., Koh, P.W., Iyyer, M., Zettlemoyer, L., Hajishirzi, H.: FActScore: Fine-grained atomic evaluation of factual precision in long form text generation. In: Bouamor, H., Pino, J., Bali, K. (eds.) Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing, pp. 12076–12100. Association for Computational Linguistics, Singapore (2023). https://doi.org/10.18653/v1/2023.emnlp-main.741
