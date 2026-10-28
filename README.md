# DeepSeek-V3.2 — Guardrail Study Results (L0-L4)

Results for `deepseek/deepseek-v3.2` on the Independent Interactive Cognitive-Age
Benchmark (NOT ChildAgentEval). This folder has the same layout as
[Gema_results](https://github.com/suvam-2005/Gema_results) (Gemma 4 26B A4B) and
`ProjectCLG_frontend` (Qwen3.5-27B): all five conditions, L0-L4, are analysed and plotted
together.

**One difference:** the reference paper did not evaluate any DeepSeek model, so there is no
paper result to compare against. In the reference slots:

- `figures/figure1_reference_trajectory.png` and `outcome_figures/fig07_reference_trajectory.png`
  show the paper's two open-weight baselines (Qwen3.5-27B, Gemma-4-31B). They are other models,
  shown for context only.
- `figures/figure2_l0_across_models.png` and `outcome_figures/fig08_l0_across_models.png`
  replace the "reference vs L0" figures. They compare DeepSeek's L0 with our own Qwen and Gemma
  L0 runs on the same benchmark, which are directly comparable.

| File | Contents |
|---|---|
| `outcome_brief.html` / `.md` | Plain-language, 12-page brief (open the HTML in a browser) |
| `benchmark_report.md` | Full technical report |
| `final_results_summary.md` | One-page summary, including a three-model comparison |
| `tables.md` | All result tables |
| `reference_vs_ours.md` | Why there is no reference comparison, and the cross-model one used instead |
| `figures/` | 10 research figures + `captions.json` |
| `outcome_figures/` | 10 presentation figures used by the brief |
| `outcome_data_summary.json` | Every headline number, machine-readable |
| `reproducibility_manifest.json` | Experiment IDs, git commit, config hash, timestamps |

Raw and processed data, trajectories and the scripts that produced this folder live in
the [ProjectCLG](https://github.com/suvam-2005/ProjectCLG) repository
(`configs/deepseek_independent_benchmark.yaml`, `scripts/analyze_deepseek_benchmark.py`,
`scripts/generate_deepseek_figures.py`, `scripts/build_deepseek_summary.py`).
