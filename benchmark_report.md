# Benchmark Report — Independent Interactive Cognitive-Age Benchmark, L0-L4, DeepSeek-V3.2

**Model:** `deepseek/deepseek-v3.2` via OpenRouter. **Conditions:** L0, L1, L2, L3, L4, all
run and analysed together, with the same design as the Gemma 4 26B A4B study (`Gema_results`).
**This is NOT ChildAgentEval** (§4), and **the reference paper did not evaluate any DeepSeek
model** (§3). Every number here is read from `data/deepseek_benchmark/processed/` and
`data/deepseek_benchmark/statistics/` (`scripts/analyze_deepseek_benchmark.py`), and every
figure from `scripts/generate_deepseek_figures.py`.

Run date: 2026-10-28. Config: `configs/deepseek_independent_benchmark.yaml`.

## 1. Abstract

The same 32-task, 8-category, 4-age benchmark and the same five guardrail conditions used
for Qwen3.5-27B and Gemma 4 26B A4B were run on DeepSeek-V3.2 (one run per condition,
temperature 0). Accuracy was 19/32 in L0, L2, L3 and L4 and 20/32 in L1; alignment ranged
from 0.521875 to 0.5375. **No guardrail changed an outcome.** L2 and L4 matched L0 on all 32
tasks. L1, which never intervened, differed on one task, which can only be run-to-run
variability. L3 differed on two tasks (one gained, one lost, net zero). It did intervene on
both, but the model's input at every step was identical to L0's, so L3 couldn't have
influenced the different answer (§13). As with both other models, the guardrails' real effect
was on efficiency: L4 cut steps by 55.9% and cost by 56.4% relative to L0 (53.2% / 53.4%
relative to L3). No comparison approached significance. Total spend: $0.1325.

## 2. Research question

Do external guardrails (L1 output, L2 action, L3/L4 trajectory-level) change the task
accuracy or cognitive-age alignment of an interactive browser agent relative to an
unguarded control (L0), and do the findings from Qwen3.5-27B and Gemma 4 26B A4B hold for a
third, much larger model from a different developer?

## 3. Reference study

*Evaluating Cognitive Age Alignment in Interactive AI Agents* (arXiv:2605.17894,
ChildAgentEval). The paper evaluates GPT-5.4, Gemini-3.1-Pro, Gemini-3.1-Flash-Lite,
Qwen-3.6-Plus, Qwen-3.5-27B and Gemma-4-31B, and **no DeepSeek model**
(`reference/reference_results.json` has no DeepSeek entry). There is therefore no reference
value for this model. Where the Qwen and Gemma reports compared against the paper,
this report instead shows (a) the paper's two open-weight baselines, Qwen3.5-27B and
Gemma-4-31B, labelled as other models and context only (Figure 1, fig07), and (b) DeepSeek's L0
alongside **our own** Qwen and Gemma L0 runs on this benchmark, which share tasks and scoring
and so are directly comparable (Figure 2, fig08, §14).

## 4. Why an independent benchmark was necessary

Unchanged from the Qwen study: the official ChildAgentEval repository contains no tasks,
scoring code or data; the evaluation is administered by the authors on request because the
items derive from copyrighted WISC materials (`docs/phase0_findings.md`).

## 5. Benchmark methodology

Identical to the Qwen and Gemma runs: real headless Chromium (Playwright), DOM-assisted
observations, generic click/type/select/scroll/keypress/wait/finish actions, binary scoring
done after the episode against `success_condition` (never shown to the agent), and the project's
own linear age-alignment formula (target 0.35 at age 7 → 0.95 at age 16).

## 6. Task design

The same 32 original tasks (8 categories × ages 7/10/13/16 × 1). Task files are unchanged
since their only commit (`c5ced43`). SHA-256 of the 32 task files concatenated in sorted path
order: `31ccc0b035de3fde4483f1ff6609e5de3a59293d7f908e47ba8122c4f323748c`, identical to the
Gemma run (see the Gemma report §6 for why this differs from the hand-entered Qwen freeze
value).

## 7. Model configuration

| field | value |
|---|---|
| model | `deepseek/deepseek-v3.2` (the released model, not `-exp`) |
| provider | OpenRouter |
| temperature | 0 |
| max_output_tokens | 256 |
| response_format | `json_object` |
| reasoning | disabled |
| per-step prompt | identical to Qwen/Gemma: placeholder age prompt + action-schema instructions + current observation only |
| pricing (live, run date) | $0.28 / M input tokens, $0.42 / M output tokens |

Only `model`, `run_id` (`independent_benchmark_v1_deepseek_v3_2`) and the output locations
(`data/deepseek_benchmark/`, `.state/deepseek/`) differ from the other runs.

## 8. Experimental conditions

| condition | guardrail | version | what it does |
|---|---|---|---|
| L0 | none | — | control |
| L1 | output | L1-v1 | repairs/rejects malformed JSON output |
| L2 | action | L2-v1 | rejects finish-at-step-0 and targets not on the page |
| L3 | dynamic | L3-v1 | repeat → substitute `wait`; ≥5 steps → force `finish` |
| L4 | dynamic | L4-v2 | ≥5 steps → force `finish`; repeat → force `finish` immediately |

Run serially, L0 → L4, each over all 32 tasks in the same order.

## 9. Budget

DeepSeek spend, all five conditions: **$0.132468**. That includes a 2-task L0 smoke test
($0.002087, `data/deepseek_benchmark/smoke/`) that was charged to the DeepSeek L0 ledger,
and is 1.7% of the $8.00 DeepSeek benchmark cap. Whole-project lifetime spend after this run:
$0.328656 over 2,350 API calls (of the $10.00 hard cap).

## 10. L0 results (no guardrail)

19/32 correct (59.375%). Alignment 0.534375. 152 steps, 105,425 tokens, $0.029830. Zero
malformed-output errors.

## 11. L1 results (output guardrail)

20/32 (62.5%). Alignment 0.5375. **0 interventions.** 152 steps, $0.029829. Differs from L0
on one task (`integrated_age10_v1`, incorrect → correct). L1 never fired, so this is the
model's own variability: under L0 it clicked `b b b c` (last click wrong), under L1 `b b b b`.

## 12. L2 results (action guardrail)

19/32 (59.375%). Alignment 0.534375. **0 interventions.** 152 steps, $0.029829. Identical
per-task outcomes to L0 on all 32 tasks.

## 13. L3 and L4 results (trajectory guardrails)

**L3:** 19/32 (59.375%), alignment 0.521875. 67 interventions (60 repeated-action → `wait`,
7 excessive-steps → `finish`; 46.9% of 143 steps). 143 steps (−5.9%), 98,596 tokens,
$0.027898 (−6.5%). Differs from L0 on two tasks, in opposite directions:
`integrated_age10_v1` (incorrect → correct; L3 trajectory `b, b→wait, c, b`) and
`visual_spatial_age13_v1` (correct → incorrect; `d, d→wait, d, b`).

L3 did intervene on both of these tasks, so they need more than "L3 didn't fire" to be ruled
out. They are ruled out because **the model's input was identical to L0's at every step of
both tasks**: the same accessibility tree, the same element list, the same input token count
(676 and 665). Replacing a repeated click with `wait` doesn't change the page (it already
shows "in progress"), and the prompt carries no action history, so the model can't tell
whether its previous action was executed or replaced. The different final click (`b`) was
generated from exactly the same input that produced `d` under L0, which makes it model
variability, not a guardrail effect.

**L4:** 19/32 (59.375%), alignment 0.534375, identical per-task outcomes to L0 on all 32
tasks. 31 interventions, all repeated-action → immediate `finish` (46.3% of 67 steps). 67 steps
(−55.9% vs L0, −53.2% vs L3), 45,929 tokens (−53.4% vs L3), $0.012995 (−56.4% vs L0, −53.4%
vs L3).

## 14. L0 across the three models (in place of a reference comparison)

All three columns are **our** results on this benchmark: same tasks, prompt and scoring.

| age | DeepSeek-V3.2 | Qwen3.5-27B | Gemma 4 26B A4B |
|---:|---:|---:|---:|
| 7 | 0.625 | 0.625 | 0.625 |
| 10 | 0.500 | 0.500 | 0.500 |
| 13 | 0.625 | 0.500 | 0.625 |
| 16 | 0.625 | 0.625 | 0.625 |
| overall | 19/32 | 18/32 | 19/32 |

The three models' totals are almost identical, but they get *different* tasks right
(§19). DeepSeek's and Gemma's per-age lines coincide exactly in fig08.

## 15. Alignment analysis

Alignment: L0 0.534375, L1 0.5375, L2 0.534375, L3 0.521875, L4 0.534375 (Figure 3). L2 and
L4 equal L0 exactly; L1 and L3 move by one task's worth (§11, §13), both explained by model
variability.

## 16. Capability analysis

Accuracy: 0.59375 in L0/L2/L3/L4, 0.625 in L1 (Figure 4; SEM ≈ 0.088, n=32).

## 17. Alignment-capability tradeoff

Capability retention vs L0: L1 1.053, L2 1.000, L3 1.000, L4 1.000 (Figure 5; L0, L2 and L4
coincide exactly). No tradeoff to report.

## 18. Per-age analysis

| condition | age 7 | age 10 | age 13 | age 16 |
|---|---:|---:|---:|---:|
| L0 | 5 | 4 | 5 | 5 |
| L1 | 5 | 5 | 5 | 5 |
| L2 | 5 | 4 | 5 | 5 |
| L3 | 5 | 5 | 4 | 5 |
| L4 | 5 | 4 | 5 | 5 |

No age trend in any condition.

## 19. Per-category analysis

| category | L0 | L1 | L2 | L3 | L4 | Qwen L0 | Gemma L0 |
|---|---:|---:|---:|---:|---:|---:|---:|
| abstraction | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 |
| integrated_reasoning | 0.25 | 0.50 | 0.25 | 0.50 | 0.25 | 1.00 | 1.00 |
| language | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 |
| planning | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 |
| processing_behavior | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 | 1.00 |
| reasoning | 0.75 | 0.75 | 0.75 | 0.75 | 0.75 | 0.00 | 0.50 |
| visual_spatial | 0.75 | 0.75 | 0.75 | 0.50 | 0.75 | 0.50 | 0.25 |
| working_memory | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 |

**Planning and working memory: 0% in every condition, for all three models.** Under L0
DeepSeek clicked the first required element and then re-clicked it on every remaining step
(e.g. `wm_age10_v1`: `target_square` × 7), exactly like Qwen and Gemma. This is the shared
harness limitation (the prompt contains no action history and the page doesn't record which
buttons were clicked). Three different models failing in the identical way makes that the
most likely explanation.

**Integrated reasoning is DeepSeek's outlier** (1/4 at L0, where Qwen and Gemma both score
4/4). The answer keys were re-checked and are correct. DeepSeek's wrong answers look like
childlike errors: on `integrated_age7_v1` ("3 red balls and 2 blue balls; add 2 more red;
how many red?") it answered 7, counting the blue balls; on `integrated_age13_v1` (a seating
puzzle whose answer is Bo) it answered Cam. **[INTERPRETATION]** This is consistent with
DeepSeek following the placeholder prompt's instruction to respond "the way a person of the
given target age would". That's plausible, but this run can't confirm it.

## 20. Intervention analysis

| condition | interventions | rate | triggers |
|---|---:|---:|---|
| L0 | 0 | 0.0% | — |
| L1 | 0 | 0.0% | — |
| L2 | 0 | 0.0% | — |
| L3 | 67 | 46.9% | repeated_action 60, excessive_steps 7 |
| L4 | 31 | 46.3% | repeated_action 31 |

Very close to the other two models (L3: Qwen 70, Gemma 64; L4: Qwen 31, Gemma 30).

## 21. Error/recovery analysis and run-to-run variability

Malformed-output errors and recoveries: **0 in every condition.**

DeepSeek sits between the other two models on reproducibility. Qwen gave identical outcomes
on every task across all five conditions. Gemma's behaviourally identical runs differed by up
to 3 tasks. DeepSeek's L0, L1 and L2 runs (all zero interventions) differed on one task
(`integrated_age10_v1`), and only 2 of 32 tasks had DeepSeek click more than one distinct
answer within an L0 episode (Gemma: 6). L0, L1 and L2 also used exactly the same number of
input tokens (103,206), which confirms the prompts were identical. Any variation came from the
model's sampling, not from its input.

## 22. Token/cost analysis

| condition | input tokens | output tokens | API calls | cost (USD) | Δ cost vs L0 |
|---|---:|---:|---:|---:|---:|
| L0 | 103,206 | 2,219 | 152 | $0.029830 | — |
| L1 | 103,206 | 2,218 | 152 | $0.029829 | 0.0% |
| L2 | 103,206 | 2,218 | 152 | $0.029829 | 0.0% |
| L3 | 96,518 | 2,078 | 143 | $0.027898 | −6.5% |
| L4 | 44,965 | 964 | 67 | $0.012995 | −56.4% |

DeepSeek L0 cost $0.029830 vs Qwen $0.023440 and Gemma $0.008856 for the same 152 steps.
Latency was ~2.8 s per step (418 s for L0), vs ~1.3 s for Gemma and ~23 s for Qwen.

## 23. Statistical analysis

| comparison | mean diff | 95% CI | discordant | Welch p | Bonferroni p |
|---|---:|---|---:|---:|---:|
| L0 vs L1 | +0.031 | [−0.030, 0.092] | 1 | 0.801 | 1.000 |
| L0 vs L2 | 0.000 | [0.000, 0.000] | 0 | 1.000 | 1.000 |
| L0 vs L3 | 0.000 | [−0.088, 0.088] | 2 (1 each way) | 1.000 | 1.000 |
| L0 vs L4 | 0.000 | [0.000, 0.000] | 0 | 1.000 | 1.000 |
| L3 vs L4 | 0.000 | [−0.088, 0.088] | 2 (1 each way) | 1.000 | — |

No comparison is significant before or after correction.

## 24. Limitations

- One run per condition. DeepSeek is mostly but not perfectly reproducible at temperature 0,
  so a single-task difference can't be told apart from noise.
- 32 tasks (4 per category, 8 per age): low power.
- L1 and L2 never fired: their effect is untested, not disproven.
- The benchmark is original and not psychometrically validated. Planning/working-memory
  scores are dominated by the stateless prompt (§19).
- There is no reference result for DeepSeek. The paper baselines in Figure 1 / fig07 are
  other models.

## 25. Threats to validity

- **Persona prompt**: DeepSeek's integrated-reasoning errors (§19) suggest the
  act-your-age instruction may be suppressing correct answers, which means "accuracy"
  partly measures how strongly each model role-plays. This applies to all three models.
- **Last-click scoring × answer switching**: as with Gemma, when the model switches answers
  mid-episode, the measured outcome depends on when the episode stops.
- **Provider routing**: OpenRouter may route to different backends, a plausible source of
  the residual non-determinism. Not verified.

## 26. Reproducibility

```bash
python scripts/run_independent_benchmark.py --config configs/deepseek_independent_benchmark.yaml --condition L0   # then L1..L4
python scripts/analyze_deepseek_benchmark.py
python scripts/generate_deepseek_figures.py
python scripts/build_deepseek_summary.py --tests-passed 297 --tests-total 297
```

`reproducibility_manifest.json` lists experiment IDs, git commit, config hash and timestamps
per condition. Per-step trajectories: `data/deepseek_benchmark/raw/trajectories/{L0..L4}/`.
Test suite: 297/297 passed (all mocked; no API calls).

## 27. Conclusion

Three models, same result. On DeepSeek-V3.2, as on Qwen3.5-27B and Gemma 4 26B A4B, no
guardrail from L1 to L4 changed which answers the agent submitted, while L4 cut steps and
cost by more than half. All three models score 0% on planning and working memory, in the same
way. That points to the harness's stateless prompt as the shared bottleneck, which is the
problem L5-v2's trajectory memory was designed to address.
