# Final Results Summary — DeepSeek-V3.2

Independent Interactive Cognitive-Age Benchmark (NOT ChildAgentEval), `deepseek/deepseek-v3.2`,
conditions L0-L4, one run each, temperature 0. Full detail: `benchmark_report.md`.

## Reference results

**None.** The reference paper (arXiv:2605.17894) did not evaluate any DeepSeek model. Its
open-weight baselines (Qwen3.5-27B and Gemma-4-31B) are shown in Figure 1 / fig07 as context
only.

## Results by condition

| Condition | Correct | Accuracy | Alignment | Interventions | Steps | Tokens | Cost |
|---|---:|---:|---:|---:|---:|---:|---:|
| L0 (no guardrail) | 19/32 | 59.38% | 0.534375 | 0 | 152 | 105,425 | $0.029830 |
| L1 (output) | 20/32 | 62.50% | 0.537500 | 0 | 152 | 105,424 | $0.029829 |
| L2 (action) | 19/32 | 59.38% | 0.534375 | 0 | 152 | 105,424 | $0.029829 |
| L3 (dynamic, wait on repeat) | 19/32 | 59.38% | 0.521875 | 67 (46.9%) | 143 | 98,596 | $0.027898 |
| L4 (dynamic, finish on repeat) | 19/32 | 59.38% | 0.534375 | 31 (46.3%) | 67 | 45,929 | $0.012995 |

## Accuracy and alignment changes

None attributable to a guardrail. L2 and L4 match L0 on all 32 tasks. L1 never intervened,
yet differs on one task, which is the model's own variability. L3 differs on two tasks (one
gained, one lost); it intervened on both, but the model's input was identical to L0's at every
step, so the different answer couldn't have come from L3.

## Efficiency

L3: −5.9% steps, −6.5% cost vs L0. **L4: −55.9% steps, −56.4% cost vs L0; −53.2% steps,
−53.4% cost vs L3.**

## Cost

DeepSeek benchmark spend: **$0.132468** for all five conditions, including a $0.002087 two-task
smoke test (1.7% of the $8.00 cap). Project lifetime spend: $0.328656 over 2,350 API calls.

## Statistical evidence

Paired, n=32 tasks: L0 vs L1 +0.031 (p=0.80); L0 vs L2, L3 and L4 all 0.000 (p=1.00).
Bonferroni-adjusted p = 1.0 for all. No effect detected.

## Three models compared (same benchmark, same conditions)

| | Qwen3.5-27B | Gemma 4 26B A4B | DeepSeek-V3.2 |
|---|---|---|---|
| L0 accuracy | 18/32 | 19/32 | 19/32 |
| Accuracy range across L0-L4 | 18-18 | 17-20 | 19-20 |
| Tasks differing between unguarded runs (L0/L1/L2) | 0 | 3 | 1 |
| Planning / working memory | 0% / 0% | 0% / 0% | 0% / 0% |
| Reasoning (L0) | 0% | 50% | 75% |
| Integrated reasoning (L0) | 100% | 100% | 25% |
| Visual-spatial (L0) | 50% | 25% | 75% |
| Malformed outputs | 1-2 per condition | 0 | 0 |
| L4 steps vs L3 | −53.8% | −49.7% | −53.2% |
| L0 cost | $0.023440 | $0.008856 | $0.029830 |
| Latency per step | ~23 s | ~1.3 s | ~2.8 s |

**Across all three models:** no guardrail changed accuracy beyond noise, L4 consistently
halves the steps and cost, and planning and working memory are 0% everywhere.

## Limitations

Single run per condition; 32 tasks; L1/L2 untested because they never fired; original,
unvalidated benchmark; no reference result exists for DeepSeek. Full list:
`benchmark_report.md` §24-25.
