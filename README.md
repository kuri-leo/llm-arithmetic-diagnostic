# Do Large Language Models Truly Grasp Addition? (EMNLP 2025)

Official repository for the EMNLP 2025 Main Conference paper: **"Do Large Language Models Truly Grasp Addition? A Rule-Focused Diagnostic Using Two-Integer Arithmetic"**.

This repository contains the code to generate the diagnostic dataset used in our study and will host the dataset itself.

**Authors:** Yang Yan, Yu Lu, Renjun Xu, Zhenzhong Lan

## 📜 Abstract

Large language models (LLMs) achieve impressive results on advanced mathematics benchmarks but sometimes fail on basic arithmetic tasks, raising the question of whether they have *truly grasped* fundamental arithmetic rules or are merely relying on pattern matching. To unravel this issue, we systematically probe LLMs’ understanding of two-integer addition (0 to 2<sup>64</sup>) by testing three crucial properties: commutativity (A+B=B+A), representation invariance via symbolic remapping (e.g., 7$\mapsto$Y), and consistent accuracy scaling with operand length. Our evaluation of 12 leading LLMs reveals a stark disconnect: while models achieve high numeric accuracy (73.8–99.8%), they systematically fail these diagnostics. Specifically, accuracy plummets to $\le 7.5$\% with symbolic inputs, commutativity is violated in up to 20% of cases, and accuracy scaling is non-monotonic. These findings demonstrate that current LLMs address elementary addition via pattern matching, not robust rule induction, motivating new diagnostic benchmarks and innovations in model architecture and training to cultivate genuine mathematical reasoning.

## 💡 Key Findings

Our work reveals that LLMs, despite their prowess on complex benchmarks, rely on fragile, surface-level heuristics for elementary addition rather than robust, abstract rules.

1.  **Representation Invariance Failure:** Accuracy collapses when digits are mapped to symbols. Models with >99% numerical accuracy drop to as low as **7.5%** on symbolic equivalents, indicating a failure to generalize beyond familiar tokens.
2.  **Inconsistent Accuracy Scaling:** Performance does not degrade monotonically with operand length. Instead, we observe an erratic 'drop-rebound' pattern, suggesting reliance on length-specific heuristics, not a scalable algorithm.
3.  **Algebraic Integrity Violations:** Models systematically violate the commutative property ($A+B=B+A$) in up to **20%** of problem pairs, a direct contradiction of a core arithmetic rule.
4.  **Interventions Expose Pattern Matching:** Providing explicit rules paradoxically **degrades** performance, while task-specific fine-tuning improves in-domain accuracy but fails to generalize, reinforcing the pattern-matching hypothesis.

## 📊 Dataset

We constructed a diagnostic dataset of **100,000 unique two-integer addition problems** to systematically test the properties of rule-based reasoning. You may also generate the dataset using the provided code.

### Dataset Features

*   **Operands:** Integers A and B are sampled from the range `[0, 2^64 - 1)`.
*   **Commutativity Pairs:** For every problem `A+B`, its commuted counterpart `B+A` is also included to directly test algebraic integrity.
*   **Structured Generation:**
    *   **Phase 1:** Exhaustive pairs for two-digit numbers (0-99).
    *   **Phase 2:** Uniformly sampled pairs with operand lengths from 3 to 20 digits to test scaling properties.
    *   **Phase 3:** Densely sampled pairs in the large number range ($2^{49}$ to $2^{64}$) to test robustness.
*   **Symbolic Variants:** A subset of the numerical problems is mapped to 10 different bijective digit-to-symbol schemes (e.g., `0` $\mapsto$ `u`, `1` $\mapsto$ `d`, ...) to test representation invariance.

### Accessing the Dataset

A sample of the generated CSV data looks like this:

| idx | stage | left | right | result |
| :--- | :--- | :--- | :--- | :--- |
| 0-1 | stage 1 | 0 | 0 | 0 |
| 0-2 | stage 1 | 0 | 0 | 0 |
| ... | ... | ... | ... | ... |
| 10000-1 | stage 2 | 2345 | 9876 | 12221 |
| 10000-2 | stage 2 | 9876 | 2345 | 12221 |
| ... | ... | ... | ... | ... |


## 🧪 Experiment Infrastructure

The experiments in our paper were conducted using a separate benchmarking framework:

**🔗 [LLM_reasoning_benchmarking](https://github.com/kuri-leo/LLM_reasoning_benchmarking)** — A benchmarking framework for LLM reasoning evaluation, supporting dual-mode execution (online streaming & OpenAI Batch API), async scheduling, Pydantic v2 data contracts, and append-only JSONL persistence with idempotent resume.

## ✍️ How to Cite

If you find our work valuable for your research, we would greatly appreciate it if you cite our paper:

```bibtex
@inproceedings{yan2025do,
  title     = {Do Large Language Models Truly Grasp Addition? A Rule-Focused Diagnostic Using Two-Integer Arithmetic},
  author    = {Yan, Yang and Lu, Yu and Xu, Renjun and Lan, Zhenzhong},
  booktitle = {Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing (EMNLP)},
  year      = {2025},
  address   = {Suzhou, China},
}
```
