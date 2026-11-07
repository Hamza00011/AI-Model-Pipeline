# AI model comparison - EDA summary

*Data: Artificial Analysis (artificialanalysis.ai), OpenRouter, Hugging Face Hub.* Pipeline run 1 (2026-11-02T09:44:43+00:00, source: live).

## Coverage

- **722** models after validation; **0** rows rejected by the Pydantic schema.
- Complete on all four dimensions (score, price, speed, parameter count): **28** (4%).
- Open weights (Hugging Face repo found): **161**; parameter counts known for **306** (42%).

How Artificial Analysis models were matched to OpenRouter after name normalization:

| OpenRouter match | models |
|---|---|
| exact | 369 |
| none | 335 |
| fuzzy | 11 |
| token_set | 7 |

## Score vs model size

- Spearman ρ = **0.54** (moderate, n = 28). A log-linear fit gives **+7.2** index points per 10× parameters (R² = 0.38).
- Models on the *smallest-for-their-score* frontier:

| model | params (B) | index |
|---|---|---|
| Granite 4.2 3B | 3.00 | 4.70 |
| Qwen3.8 27B (Xhigh) | 27.78 | 20.10 |
| Nanbeige4.3-33B | 33.00 | 20.40 |
| MiniMax-M3.1-Flash | 220.5 | 28.90 |
| GLM-5.3 (Max) | 753.3 | 31.70 |
| DeepSeek V4.1 Flash 1017 (Max) | 763.2 | 32.50 |

## Score vs price

- Spearman ρ (log price vs score) = **0.75** (strong, n = 86).
- Models on the *cheapest-for-their-score* frontier (nothing is both cheaper and better):

| model | index | $/1M | tok/s |
|---|---|---|---|
| Nemotron 3.5 Lightning | 5.50 | 0.10 | 287.1 |
| MiMo-V2.6-Flash | 26.70 | 0.17 | 35.62 |
| Claude Haiku 5.5 (Max) | 36.20 | 0.20 | 235.5 |
| GPT-6.1 Terra (Max) | 51.00 | 2.00 | 119.3 |
| GPT-6.1 Sol (Max) | 54.50 | 4.00 | 74.97 |
| Gemini 4 Carbon (High) | 59.40 | 8.00 | 90.59 |

- Best intelligence per dollar (models at or above the median score):

| model | index | $/1M | pts per $ |
|---|---|---|---|
| Claude Haiku 5.5 (Max) | 36.20 | 0.20 | 181 |
| Claude Haiku 5.5 (Xhigh) | 34.30 | 0.20 | 171 |
| DeepSeek V4.1 Flash 1017 (Max) | 32.50 | 0.53 | 62 |
| GPT-6.1 Terra (Max) | 51.00 | 2.00 | 26 |
| GPT-6.1 Terra (Xhigh) | 45.00 | 2.00 | 22 |
| GPT-6.1 Terra (High) | 42.30 | 2.00 | 21 |
| Gemini 3.8 Flash (High) | 30.50 | 1.50 | 20 |
| GPT-6.1 Terra (Medium) | 38.10 | 2.00 | 19 |
| Muse Spark 1.3 (Max) | 36.40 | 2.00 | 18 |
| GPT-6.1 Terra (Non-reasoning) | 30.40 | 2.00 | 15 |
| GLM-5.3 (Max) | 31.70 | 2.15 | 15 |
| GPT-6.1 Sol (Max) | 54.50 | 4.00 | 14 |

## Speed vs price

- Spearman ρ (log price vs tokens/s) = **-0.35** (moderate, n = 86).
- Models on the *cheapest-for-their-speed* frontier:

| model | tok/s | $/1M | index |
|---|---|---|---|
| Nemotron 3.5 Lightning | 287.1 | 0.10 | 5.50 |
| Nemotron 3.5 Super VL | 471.1 | 0.28 | 13.60 |

## Benchmark correlations

- Mean pairwise Spearman ρ across 24 benchmark pairs: **0.83** (range 0.49 to 0.97).
- Least-correlated pairs (most distinct signal):

| benchmark A | benchmark B | ρ | shared n |
|---|---|---|---|
| HLE | MATH-500 | 0.49 | 190 |
| HLE | AIME | 0.51 | 190 |
| MMLU-Pro | HLE | 0.68 | 342 |
| SciCode | AIME | 0.73 | 16 |
| HLE | LiveCodeBench | 0.73 | 340 |

- Most-correlated pairs (largely redundant):

| benchmark A | benchmark B | ρ | shared n |
|---|---|---|---|
| MMLU-Pro | GPQA | 0.97 | 348 |
| MATH-500 | AIME | 0.96 | 193 |
| Intelligence idx | HLE | 0.93 | 86 |

## Trends over time

By release year (models with a release date and score):

| year | models | best | median | best open | median $/1M |
|---|---|---|---|---|---|
| 2026 | 92 | 59.40 | 29.95 | 32.50 | 2.00 |

- Cheapest model with index ≥ 30 so far: **$2.15** at first appearance (2026-08) vs **$0.20** now (10.7× cheaper).

- Best overall index: **59.4**; best open-weights index: **32.5** (gap +26.9).

- `model_snapshots` stores each pipeline run's score/price/speed, so true time-series of *the same model* accumulate as you re-run the ETL on a schedule; the charts above use release dates.

## Missing-data analysis

| column | % missing |
|---|---|
| coding_index | 100.0 |
| math_index | 100.0 |
| intelligence_index | 87.30 |
| aime | 73.10 |
| math_500 | 72.00 |
| scicode | 66.20 |
| params_b | 57.60 |
| livecodebench | 52.50 |
| mmlu_pro | 51.70 |
| context_length | 46.40 |
| gpqa | 14.50 |
| hle | 6.20 |
| tokens_per_sec | 0.00 |
| price_blended_usd_per_m | 0.00 |
| ttft_s | 0.00 |
| release_date | 0.00 |

Missingness by group (% missing). Gaps are not random: they depend on whether a model is open (parameter counts) and on whether OpenRouter lists it (context length):

| group | price_blended_usd_per_m | tokens_per_sec | context_length | params_b | hle |
|---|---|---|---|---|---|
| Closed / not on Hugging Face | 0.00 | 0.00 | 59.70 | 73.60 | 7.10 |
| Open weights (Hugging Face repo found) | 0.00 | 0.00 | 0.00 | 1.90 | 3.10 |

- Earliest model with an HLE score: 2023-03-14; latest model without one: 2026-10-29 - the date ranges overlap, so some gaps are not explained by the benchmark's start date alone.

- Price cross-check on 342 models priced by both sources: median OpenRouter/AA ratio **1.00**, 78% within ±25%.

- Parameter-count provenance: {'missing': 416, 'hf': 158, 'name': 148}.

## Figures

- `figures\01_score_vs_size.png`
- `figures\02_score_vs_price.png`
- `figures\03_speed_vs_price.png`
- `figures\04_benchmark_correlations.png`
- `figures\05_trends_over_time.png`
- `figures\06_missing_data.png`
- `figures\07_best_value.png`

## Caveats

- **"Open weights" is inferred** from a Hugging Face repo id on OpenRouter or in `data/manual_hf_ids.csv`; an open model OpenRouter does not list is counted as closed unless added manually.
- **Parameter counts are total, not active.** Mixture-of-experts models look large on the size axis; `active_params_b` is parsed from names like `235B A22B` where available.
- Hugging Face `safetensors.total` can mis-count quantized checkpoints; prefer full-precision repos in the manual map.
- Name matching is conservative by design: ambiguous names stay unmatched rather than risk a wrong join.
- Prices are list prices at fetch time; reasoning models use many more output tokens per task than the price suggests.
- Correlations and fits describe this sample of benchmarked models, not causal effects.
