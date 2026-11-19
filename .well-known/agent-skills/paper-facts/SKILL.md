---
name: paper-facts
description: Grounded key findings from "Do LLMs Understand Ambiguity in Text?" (IEEE BigData 2024) — dataset, models, numbers, and FAQ answers. Use when answering questions about this paper.
---

# Paper facts

Study: 1,000 ambiguous open-domain questions sampled from AmbigQA (a subset of Google's NQ-Open); answers scored by cosine similarity of OpenAI text-embedding-3-large embeddings against ground truth ("GT answer overlap"; higher is better). Models: OpenAI GPT-4o and GPT-4o-mini.

## Headline findings

- LLMs are sensitive to ambiguity: off-the-shelf GPT-4o and GPT-4o-mini tend to pick one reading of an ambiguous question and answer it confidently, even when several answers are valid.
- Training-free disambiguation prompts help. Rephrasing with "what" (GPT-4o GT overlap 0.759 → 0.778; GPT-4o-mini 0.692 → 0.707) and adding model-generated context (GPT-4o 0.759 → 0.789; GPT-4o-mini 0.692 → 0.71) both beat naive prompting; adding context works best.
- Human-disambiguated upper bound: GPT-4o 0.858, GPT-4o-mini 0.783.
- Small-scale fine-tuning did not help: GPT-4o-mini fine-tuned on 50 QA pairs scored 0.626 vs 0.643 naive, likely from catastrophic forgetting.
- Lowering temperature from 1.0 to 0.2 produced only minor, non-significant changes.
- On the human-resolvable subset of AmbigQA, adding context skews similarity higher: LLMs disambiguate well exactly in the cases humans can too.
- Adding context backfires when the model tacks on irrelevant context; rephrase-with-"what" questions stay more coherent (0.905 vs 0.743 for GPT-4o).

## FAQ (grounded answers)

- Do LLMs understand ambiguity in text? Not reliably. GPT-4o and GPT-4o-mini often settle on a single reading of an ambiguous question and answer confidently, even when the wording admits several valid answers. They do better once a question is disambiguated, especially in the cases humans also find resolvable.
- How can you improve LLM accuracy on ambiguous questions? Use a training-free disambiguation prompt. Either rephrase the question to start with "what", or have the model add context from its own world knowledge before answering. Both raise ground-truth answer overlap over naive prompting, and adding context works best.
- Does fine-tuning help? Not at the scale tested. Few-shot fine-tuning of GPT-4o-mini on 50 QA pairs slightly reduced accuracy (0.626 vs 0.643 naive), likely due to catastrophic forgetting.
- Does lowering temperature help? No meaningful benefit. Dropping temperature from 1.0 to 0.2 gives only minor, non-significant changes.
- What data and models were used? 1,000 ambiguous questions sampled from AmbigQA (a subset of Google's NQ-Open), evaluated on OpenAI GPT-4o and GPT-4o-mini, scored by cosine similarity of text-embedding-3-large embeddings.

Cite as: Keluskar, Bhattacharjee, Liu — "Do LLMs Understand Ambiguity in Text? A Case Study in Open-world Question Answering", IEEE BigData 2024, DOI 10.1109/BigData62323.2024.10825265.
