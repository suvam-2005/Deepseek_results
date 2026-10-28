# Reference vs Ours — DeepSeek

**There is no REFERENCE_RESULT for this model.** The ChildAgentEval paper (arXiv:2605.17894)
evaluated GPT-5.4, Gemini-3.1-Pro, Gemini-3.1-Flash-Lite, Qwen-3.6-Plus, Qwen-3.5-27B and
Gemma-4-31B, and no DeepSeek model (`reference/reference_results.json` has no DeepSeek entry).
Nothing in this document is a `REPRODUCTION_RESULT`.

This file is kept, under the same name as in the Qwen and Gemma results, so the three result
sets line up. It records what can and can't be compared.

## What the reference paper reported (other models, context only)

**REFERENCE_RESULT**, for other models: Qwen3.5-27B baseline composite score 0.32 / 0.34 /
0.29 / 0.30 (ages 7 / 10 / 13 / 16, medium confidence) and Gemma-4-31B 0.28 / 0.29 / 0.27 / 0.25
(low confidence). Shown in Figure 1 / fig07. These say nothing about DeepSeek.

## What our L0 produced, alongside our other models

**OUR_RESULT.** L0 accuracy by age on the same benchmark and scoring:

| age | DeepSeek-V3.2 | Qwen3.5-27B | Gemma 4 26B A4B |
|---:|---:|---:|---:|
| 7 | 0.625 (5/8) | 0.625 | 0.625 |
| 10 | 0.500 (4/8) | 0.500 | 0.500 |
| 13 | 0.625 (5/8) | 0.500 | 0.625 |
| 16 | 0.625 (5/8) | 0.625 | 0.625 |
| overall | 19/32 | 18/32 | 19/32 |

Unlike a paper-vs-ours comparison, these three columns *are* on the same scale. With n=8
tasks per age and one run each, though, the differences between them are small and not
statistically meaningful.

## What changed under L1/L2/L3/L4

**OUR_RESULT.** Accuracy: L1 20/32, L2 19/32, L3 19/32, L4 19/32. None of these changes was
caused by a guardrail (see `benchmark_report.md` §11-13). L4 cut steps from 152 to 67 (−55.9%)
and cost from $0.029830 to $0.012995 (−56.4%).

## Statistical evidence

Paired comparison, n=32: L0 vs L1 +0.031 (p=0.80); L0 vs L2, L3, L4 all 0.000 (p=1.00).
Bonferroni-adjusted p = 1.0 in all four.

## Limitations

- No reference value exists for DeepSeek, so this study can't place DeepSeek on the paper's
  scale at all.
- The cross-model table is one run per model; small differences are within run-to-run noise.
- No claim of reproduction, validation or invalidation of any ChildAgentEval result is made.
