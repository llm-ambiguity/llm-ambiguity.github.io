---
name: disambiguate-questions
description: Resolve ambiguous questions before answering, using two training-free, token-level prompts from the IEEE BigData 2024 paper on LLM ambiguity (arXiv:2411.12395). Use when a question could admit several valid readings.
---

# Disambiguate ambiguous questions

Off-the-shelf LLMs tend to pick one reading of an ambiguous question and answer it confidently. Two training-free prompts measurably improve ground-truth answer overlap for GPT-4o (0.759 → 0.789) and GPT-4o-mini (0.692 → 0.71), evaluated on 1,000 ambiguous questions from AmbigQA.

## Strategy 1 — rephrase with "what"

Rewrite the question to begin with *what*, specifying the entity, person, or timeframe the "what" resolves. Rephrased questions were ~1.45x longer on average and raise GT answer overlap (GPT-4o 0.759 → 0.778, GPT-4o-mini 0.692 → 0.707). Rephrased questions also stay more coherent than context-enriched ones (0.905 vs 0.743 for GPT-4o).

Prompt (from the paper; replace "2018" with the current year at time of use):

```
Rewrite this question replacing all questions with a what, but retain
the meaning by specifying what entity or what person or what timeframe
the "what" is answering. Also, specify the current year is 2018 if
needed to answer a time-based question.
Question: {x}
```

## Strategy 2 — add context (works best)

Ask the model to generate disambiguating context from its own world knowledge, then append the original question. Context-enriched questions ran about 14.2x longer than the originals.

Prompt (from the paper; replace the date with the current date at time of use):

```
Add extra information to the following question. Also specify the
current month and year is January 2018, so answer questions accordingly.
Your aim is to disambiguate what it is asking.
Question: {x}
```

Caveat from the paper: adding context backfires when the model tacks on irrelevant context; check that added context actually narrows the question before answering.

Source: Keluskar, Bhattacharjee, Liu — "Do LLMs Understand Ambiguity in Text? A Case Study in Open-world Question Answering", IEEE BigData 2024, DOI 10.1109/BigData62323.2024.10825265.
