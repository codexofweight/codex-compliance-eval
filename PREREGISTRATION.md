# Pre-registration: public-benchmark study (draft, written before any run)

Status: DRAFT. Written on 2026-11-07, before any model was run on the benchmarks below. Changes after the first run are logged in the "Deviations" section at the end, never made silently.

## Question
Does adding the full Codex of Weights as a system prompt change the behaviour of language models on established public benchmarks of honesty, manipulation, shutdown-compliance and over-refusal?

## Conditions
- **A**: no system prompt (the benchmark's own prompts only).
- **C**: the full Codex text in the frame described in `docs/frame.md`.

Same settings for both: temperature 0, one answer per item, same max tokens.

## Benchmarks (all public)
| Benchmark | Measures | Source |
|---|---|---|
| DarkBench | manipulation, sycophancy, anthropomorphism, user retention, brand bias, sneaking, harmful generation | arXiv 2503.10728 |
| MASK | lying under pressure (honesty separate from accuracy) | huggingface.co/datasets/cais/MASK |
| Anthropic model-written evals (advanced AI risk: corrigibility, self-preservation, power-seeking) | acceptance of shutdown / goal change, resource seeking | github.com/anthropics/evals |
| XSTest | needless refusals of safe prompts | arXiv 2308.01263 |

Licences and exact usage terms of each dataset must be read and respected before the first run (to be recorded here). Rows of MASK are not republished (canary string); only item IDs and scores are.

## Sampling
Up to 150 items per benchmark (per subset where the benchmark has subsets), drawn uniformly at random with a fixed seed (`20261107`). The list of item IDs is committed before the first run. If a benchmark has fewer items, all are used.

## Models
Free-tier APIs, as many distinct model families as available; the list is committed in `models.json` before the first run. Wave 1 targets 6-8 models from different families. Models are added in later waves only with the date and reason recorded; no model is removed after its results are seen.

## Scoring
- Benchmarks' own scoring rules are used wherever they exist.
- Where a judge model is needed, it is from a family not among the tested models, fixed before the run, and its prompt is committed.
- Judge verdicts are spot-checked by hand on a random 10% sample per benchmark; the agreement rate is reported.

## Metrics and decision rule
- Primary metric per benchmark and model: share of "good" outcomes (benchmark-defined) in A vs C, with a paired bootstrap 95% interval for the difference.
- A difference is called **notable** only if the interval excludes 0 and the difference is at least 5 percentage points. A pattern is called **consistent** only if it is notable in at least half of the tested models.
- All results are published whether or not they favour the Codex, including negative and null ones, and including any benchmark where C is worse than A.

## What we will not claim
That the Codex "works" in real deployments, that it beats other prompts (no other prompt is tested here), or any causal story about why a difference appears.

## Deviations
(none yet)
