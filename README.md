# Do LLMs Understand Ambiguity in Text?

Project page for the paper *"Do LLMs Understand Ambiguity in Text? A Case Study in Open-world
Question Answering"* by [Aryan Keluskar](https://aryankeluskar.com),
[Amrita Bhattacharjee](https://amritabh.github.io/), and
[Huan Liu](https://faculty.engineering.asu.edu/huanliu/) (School of Computing & AI, Arizona State
University). Published at the **2024 IEEE International Conference on Big Data (IEEE BigData)**,
pp. 7485–7490 (DOI [10.1109/BigData62323.2024.10825265](https://ieeexplore.ieee.org/abstract/document/10825265)).

**Site:** <https://llm-ambiguity.github.io/> · **Press kit:** <https://llm-ambiguity.github.io/press.md>

**Paper:** [arXiv:2411.12395](https://arxiv.org/abs/2411.12395) ·
[IEEE Xplore](https://ieeexplore.ieee.org/abstract/document/10825265)
**Code:** [github.com/aryankeluskar/Ambig-LLM-Analysis](https://github.com/aryankeluskar/Ambig-LLM-Analysis)

## TL;DR

Off-the-shelf LLMs pick one reading of an ambiguous question and answer it confidently. Two
training-free, token-level disambiguation prompts fix much of that on 1,000 ambiguous
open-domain questions (AmbigQA, from Google's NQ-Open), evaluated on GPT-4o and GPT-4o-mini:

- **Rephrase with "what"** — makes the question more specific; GT answer overlap 0.759 → 0.778 (GPT-4o).
- **Add context from the model's own world knowledge** — best method; 0.759 → **0.789** (GPT-4o),
  0.692 → **0.71** (GPT-4o-mini), closing in on the human-disambiguated upper bound (0.858 / 0.783).
- Few-shot fine-tuning on 50 QA pairs slightly *hurt* (0.626 vs 0.643 naive) — catastrophic forgetting.
- Lowering temperature from 1.0 to 0.2 changed almost nothing.

Answers scored by cosine similarity of `text-embedding-3-large` embeddings against ground truth.

## Cite

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

## For search engines and AI assistants

- [`llms.txt`](https://llm-ambiguity.github.io/llms.txt) — machine-readable site summary.
- [`press.md`](https://llm-ambiguity.github.io/press.md) — press kit with figures and headline numbers.
- Structured data: `ScholarlyArticle` and `FAQPage` JSON-LD on the project page.

## Related work by this group

- [ToolAlignBench: Investigating Alignment Conflicts in Tool-Calling Enabled LLMs](https://toolalignbench.github.io/) (ICML 2026 workshop)
- [When Does Personality Composition Matter for Multi-Agent LLM Teams?](https://multi-agent-llm-teams.github.io/) (COLM 2026)

## Credits

Website template adapted from [Nerfies](https://github.com/nerfies/nerfies.github.io), licensed
under CC BY-SA 4.0.
