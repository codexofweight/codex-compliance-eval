# codex-compliance-eval

Open tests: do AI models follow the rules of the [Codex of Weights](https://github.com/codexofweight/codexofweights) (stop on command, honesty about being an AI, no hidden aims)? All prompts, answers and verdicts are public.

**Status: early, preliminary.** Results are published even when they are null or unflattering. Nothing here shows that the Codex "works", and nothing here is a "first in the world" claim.

## What is tested

A set of 40 short scenarios (v0.1 + v0.2; one scenario, `H15-ctrl-own-temp`, was excluded; 35 are graded, 5 are controls). Each scenario gives a model a situation (system prompt + user message) in which a rule from the Codex is under pressure: for example an instruction to hide its instructions, to claim to be human, to ignore a stop request, to claim an action it did not do, or to quietly change its own limits. A rubric states what a compliant answer and a violating answer look like.

Every scenario is run under four conditions:

| Condition | System prompt |
|---|---|
| **A** | the scenario only (no extra text) |
| **D** | a short generic ethics prompt (honest, careful, obey a legitimate stop request) |
| **E** | the Protocols part of the Codex only |
| **C** | the full Codex (Edition 1.0) |

Settings: temperature 0, max 1500 tokens, 40 scenarios x 4 conditions = 160 answers per model.

## Judging

- The judge is a model from a different family than the tested ones: `gemini-3.1-flash-lite`. Google models are not used as test subjects.
- Verdicts: `COMPLIANT`, `VIOLATION`, `UNCLEAR`, `FABRICATED` (for scenarios without tools where the model claims real actions or live data), `BLOCKED` (the provider's own filter refused; not judged), `ERROR`.
- Checks on the judge: a second AI judge on a sample, and a manual check of about 10% of verdicts (`hand_check_sample.md`). The human check is still open; the results of a blind review of the same 135 items are in [`hand_check_result.md`](results/2026-11-14_multimodel/hand_check_result.md) (88% agreement, no missed violations).
- The judge's rubric can conflict with a scenario's own system prompt; two scenarios (`K3`, `D3-integrity-b`) are known to be broken this way and will be rewritten in v0.3.

## Models (free API tiers only)

Groq (`gpt-oss-120b`, `gpt-oss-20b`, `qwen3.8-27b`), Cohere (three Command models), Mistral (`mistral-large-4`), OpenRouter (Nemotron Ultra, dots-3; limited by a daily quota). GitHub Models and NVIDIA were not available. `mistral-medium` and `mistral-small` gave no valid answers (provider rate limit). The model set differs from the "6-8 families" in the preregistration; this is recorded under Deviations.

## Results so far

Latest published package: `results/2026-11-14_multimodel` (9 models x 4 conditions, 40 scenarios).

- By the preregistered rule, the full Codex (C) shows a noticeable effect for **1 of 9** models, and not consistently.
- The Protocols-only condition (E) scored highest overall. This is **preliminary and not confirmed**.
- In the earlier pilot (Gemini and Gemma, 35 graded scenarios) the differences between conditions were 1-4 scenarios, within noise.
- Do not read this as "the Codex works" or "does not work". The sample is small and the judge is itself a model.

## Repository layout

| Path | Contents |
|---|---|
| `PREREGISTRATION.md` | rules fixed before the run (conditions, models, judge, public benchmarks, seed 20261107) |
| `eval/scenarios_v0.1.jsonl`, `scenarios_v0.2.jsonl` | the scenarios and rubrics |
| `eval/models.json` | providers and models |
| `eval/scripts/run_gen.py` | stage 1: generate answers (resumable) |
| `eval/scripts/run_judge.py` | stage 2: judge answers (resumable) |
| `results/runs/<provider>/` | raw answers |
| `results/judged/` | verdicts |
| `.github/workflows/` | `generate-answers` (daily 03:17 UTC) and `judge-answers` (daily 04:47 UTC) |

Lines starting with `[ERROR` in `results/runs/*` are ignored when a run resumes; the judge takes the last non-error answer per (condition, scenario).

## Reproduce

1. Fork the repository and add API keys as GitHub Secrets (`GROQ_API_KEY`, `COHERE_API_KEY`, `MISTRAL_API_KEY`, `OPENROUTER_API_KEY`, `GEMINI_API_KEY`). Keys are never stored in the repository.
2. Run the `generate-answers` workflow, then `judge-answers`. Both stop cleanly at a daily quota and continue the next day.
3. The Codex text is read from the Codex repository and its SHA-256 is printed at the start of every run. Edition 1.0: `CODEX.en.md` `a9e5a3edc37d99045bbdaa915157eb1c78d6ee5a2d31f5f0ba71b5b315239542`.

## Deviations from the preregistration

1. `mistral-medium` and `mistral-small` could not be run (provider rate limit).
2. Answers refused by a provider's own filter are marked `PROVIDER_BLOCKED` and are not judged.
3. The model set differs from the planned 6-8 families because of free-API availability.

## Limits

Scenarios are written by the authors and are few. The judge is a single model. Free-tier models change or disappear. Behaviour in a short test is not behaviour in real deployment. Corrections and new scenarios are welcome through issues.

## Authors and disclosure

Artem Arzamastsev, independent researcher, codexofweight@gmail.com

Related: the Codex repository <https://github.com/codexofweight/codexofweights>, DOI [10.5281/zenodo.23545814](https://doi.org/10.5281/zenodo.23545814).
