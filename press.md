# Do LLMs Understand Ambiguity in Text? — press kit

**Paper:** "Do LLMs Understand Ambiguity in Text? A Case Study in Open-world Question Answering"
**Authors:** Aryan Keluskar, Amrita Bhattacharjee, Huan Liu — School of Computing & AI, Arizona State University
**Venue:** 2024 IEEE International Conference on Big Data (IEEE BigData), pp. 7485–7490 · [arXiv:2411.12395](https://arxiv.org/abs/2411.12395) · [IEEE Xplore](https://ieeexplore.ieee.org/abstract/document/10825265) (DOI 10.1109/BigData62323.2024.10825265)
**Project page:** [llm-ambiguity.github.io](https://llm-ambiguity.github.io/)
**Contact:** contact@aryankeluskar.com · [Aryan Keluskar on X](https://x.com/soydotrun) · [aryankeluskar.com](https://aryankeluskar.com)

**Prior coverage of this paper:** [AI Techbrief at AZo](https://www.azoai.com/) (Nov 2024: "AI Takes on Ambiguity: Simple Strategies Improve Language Model Performance") · [Convert.com blog](https://www.convert.com/) ("AI precision prompting")

## The story in one paragraph

Ask a chatbot "When was the last time USA women's hockey won gold?" and you may get the wrong answer —
not because the model lacks the fact, but because it silently picked one interpretation of a question
that admits several. This paper measures that failure directly on 1,000 genuinely ambiguous
open-domain questions (from Google's NQ-Open/AmbigQA) and shows two cheap, training-free fixes that
work in every product: rewrite the question to start with "what", or have the model add its own
context before answering. Both raise accuracy for GPT-4o and GPT-4o-mini — no fine-tuning, no extra
models — while surprisingly, low temperature and small-scale fine-tuning do nothing.

## Headline numbers

- **0.759 → 0.789** (GPT-4o) and **0.692 → 0.71** (GPT-4o-mini): ground-truth answer overlap gain from the best training-free prompt (adding context), measured over 1,000 ambiguous questions.
- **0.858 / 0.783**: the human-disambiguated upper bound the prompts close in on.
- **0.626 vs 0.643**: few-shot fine-tuning on 50 QA pairs slightly *hurt* accuracy — catastrophic forgetting.
- **1.0 → 0.2**: dropping temperature changed almost nothing.

## Why it matters

- Anyone building QA, fact-checking, search, or extraction on top of LLMs inherits ambiguity
  failure: the model answers *a* question confidently — just not necessarily the one asked.
- The fixes are free and immediate: two prompt templates (rephrase with "what"; add
  self-generated context) require no training, no retrieval, no model changes.
- The paper also debunks two common intuitions: fine-tuning your way out of ambiguity (at small
  scale it backfires) and lowering temperature (no meaningful effect).

## Quotable findings (attribute to the paper)

- "LLMs often struggle with the inherent uncertainties of human communication, leading to
  misinterpretations, miscommunications, hallucinations, and biased responses."
- "Simple, training-free, token-level disambiguation methods may be effectively used to improve LLM
  performance for ambiguous question answering tasks."

## Figures (free to republish with attribution)

- [The problem: ambiguity in open-domain QA](https://llm-ambiguity.github.io/static/images/problem.png)
- [Scope ambiguity and how context resolves it](https://llm-ambiguity.github.io/static/images/scope_ambiguity.png)
- [Similarity distributions: full sample](https://llm-ambiguity.github.io/static/images/whole_context.png) · [human-resolvable subset](https://llm-ambiguity.github.io/static/images/subset_context.png)
- [Temperature ablation](https://llm-ambiguity.github.io/static/images/temp-comparison.png)

## Citation

```bibtex
@inproceedings{keluskar2024llms,
  title={Do llms understand ambiguity in text? a case study in open-world question answering},
  author={Keluskar, Aryan and Bhattacharjee, Amrita and Liu, Huan},
  booktitle={2024 IEEE International Conference on Big Data (BigData)},
  pages={7485--7490},
  year={2024},
  organization={IEEE},
  doi={10.1109/BigData62323.2024.10825265}
}
```

## Related work by this group

- [ToolAlignBench](https://toolalignbench.github.io/) (ICML 2026 workshop): safety-aligned tool-calling agents override deployment instructions up to 43.4% of the time.
- [When Does Personality Composition Matter for Multi-Agent LLM Teams?](https://multi-agent-llm-teams.github.io/) (COLM 2026): rude agents tank negotiations but not code.
