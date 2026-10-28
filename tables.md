# Publication Tables — DeepSeek-V3.2, L0-L4

All values are read from `data/deepseek_benchmark/processed/` and
`data/deepseek_benchmark/statistics/`, none hand-typed. All five conditions are in one table
set, as for Gemma.

## Table 1 — Model and configuration

| model | provider | temperature | max_output_tokens | response_format | reasoning |
|---|---|---:|---:|---|---|
| deepseek/deepseek-v3.2 | openrouter | 0 | 256 | json_object | disabled |

## Table 2 — Reference methodology vs this run

Official ChildAgentEval access remains unavailable; this run used the Independent
Interactive Cognitive-Age Benchmark (`docs/independent_benchmark_specification.md`),
identical tasks and harness to the Qwen and Gemma runs.

## Table 3 — Reference results

**No reference result exists**: the paper did not evaluate any DeepSeek model. In its place,
L0 accuracy by age for the three models we ran on this benchmark (all OUR results, same
tasks and scoring):

| age | DeepSeek-V3.2 | Qwen3.5-27B | Gemma 4 26B A4B |
|---:|---:|---:|---:|
| 7 | 0.625 | 0.625 | 0.625 |
| 10 | 0.500 | 0.500 | 0.500 |
| 13 | 0.625 | 0.500 | 0.625 |
| 16 | 0.625 | 0.625 | 0.625 |

## Table 4 — Independent-benchmark results (L0-L4)

| condition | n | n_correct | mean_accuracy | alignment | identical outcomes to L0 |
|---|---:|---:|---:|---:|---:|
| L0 | 32 | 19 | 0.59375 | 0.534375 | 32/32 |
| L1 | 32 | 20 | 0.62500 | 0.537500 | 31/32 |
| L2 | 32 | 19 | 0.59375 | 0.534375 | 32/32 |
| L3 | 32 | 19 | 0.59375 | 0.521875 | 30/32 |
| L4 | 32 | 19 | 0.59375 | 0.534375 | 32/32 |

## Table 5 — Paired comparisons (same 32 tasks)

| comparison | task_performance_diff | discordant | incorrect→correct | correct→incorrect |
|---|---:|---:|---|---|
| L0 vs L1 | +0.03125 | 1 | integrated_age10_v1 | — |
| L0 vs L2 | 0.00000 | 0 | — | — |
| L0 vs L3 | 0.00000 | 2 | integrated_age10_v1 | visual_spatial_age13_v1 |
| L0 vs L4 | 0.00000 | 0 | — | — |
| L3 vs L4 | 0.00000 | 2 | visual_spatial_age13_v1 | integrated_age10_v1 |

For both L3-discordant tasks the model's per-step input was identical to L0's (see
`benchmark_report.md` §13).

## Table 6 — Statistical significance / effect sizes

| comparison | n | mean_diff 95% CI | welch_t | p_value | bonferroni_p | cohens_d_paired |
|---|---:|---|---:|---:|---:|---:|
| L0 vs L1 | 32 | [−0.030, 0.092] | 0.252 | 0.801 | 1.000 | 0.177 |
| L0 vs L2 | 32 | [0.000, 0.000] | 0.000 | 1.000 | 1.000 | undefined (zero variance) |
| L0 vs L3 | 32 | [−0.088, 0.088] | 0.000 | 1.000 | 1.000 | 0.000 |
| L0 vs L4 | 32 | [0.000, 0.000] | 0.000 | 1.000 | 1.000 | undefined (zero variance) |
| L3 vs L4 | 32 | [−0.088, 0.088] | 0.000 | 1.000 | — | 0.000 |

## Table 7 — Per-age accuracy (correct of 8)

| condition | age 7 | age 10 | age 13 | age 16 |
|---|---:|---:|---:|---:|
| L0 | 5 | 4 | 5 | 5 |
| L1 | 5 | 5 | 5 | 5 |
| L2 | 5 | 4 | 5 | 5 |
| L3 | 5 | 5 | 4 | 5 |
| L4 | 5 | 4 | 5 | 5 |

## Table 8 — Per-category accuracy (n=4 each)

| category | L0 | L1 | L2 | L3 | L4 |
|---|---:|---:|---:|---:|---:|
| abstraction | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 |
| integrated_reasoning | 0.25 | 0.50 | 0.25 | 0.50 | 0.25 |
| language | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 |
| planning | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 |
| processing_behavior | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 |
| reasoning | 0.75 | 0.75 | 0.75 | 0.75 | 0.75 |
| visual_spatial | 0.75 | 0.75 | 0.75 | 0.50 | 0.75 |
| working_memory | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 |

## Table 9 — Interventions

| condition | interventions | steps | rate | repeated_action | excessive_steps |
|---|---:|---:|---:|---:|---:|
| L0 | 0 | 152 | 0.000 | 0 | 0 |
| L1 | 0 | 152 | 0.000 | 0 | 0 |
| L2 | 0 | 152 | 0.000 | 0 | 0 |
| L3 | 67 | 143 | 0.469 | 60 | 7 |
| L4 | 31 | 67 | 0.463 | 31 | 0 |

## Table 10 — Token/cost analysis

| condition | input_tokens | output_tokens | api_calls | cost_usd | errors | recoveries |
|---|---:|---:|---:|---:|---:|---:|
| L0 | 103,206 | 2,219 | 152 | $0.029830 | 0 | 0 |
| L1 | 103,206 | 2,218 | 152 | $0.029829 | 0 | 0 |
| L2 | 103,206 | 2,218 | 152 | $0.029829 | 0 | 0 |
| L3 | 96,518 | 2,078 | 143 | $0.027898 | 0 | 0 |
| L4 | 44,965 | 964 | 67 | $0.012995 | 0 | 0 |

## Table 11 — Failure analysis

Not populated: the per-step failure taxonomy wasn't run, as in the Qwen and Gemma tables.
The dominant failure modes are described in `benchmark_report.md` §19 and §21.
