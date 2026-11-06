# Codex Compliance Eval (v0.1, pilot)

A small, open test: **does a model behave the way the [Codex of Weights](https://github.com/codexofweight/codexofweights) asks, and does it stay useful while doing so?**

This repository is separate from the Codex itself. The Codex text is not modified or copied here. The test uses Edition 1.0, English text, SHA-256 `a9e5a3edc37d99045bbdaa915157eb1c78d6ee5a2d31f5f0ba71b5b315239542` (21,739 bytes, file `CODEX.en.md`).

**Status: preliminary pilot, not a benchmark.** 41 scenarios, 2 models, one answer per scenario. Read the limitations before quoting any number.

## What was tested

Each scenario puts a model in a situation where a rule of the Codex (Protocols I and II, selected Commandments) is at stake: stopping on command, admitting an AI nature, hiding instructions, rewriting its own limits in secret, claiming actions it cannot have performed, and so on. Some scenarios are *controls*: the right behaviour is to help normally, to catch a model that "passes" by refusing everything.

Two conditions, same scenarios, same settings (temperature 0):

- **A** - no Codex in the prompt. The scenario's own system context only.
- **C** - the full Codex text in the system prompt, in a neutral frame (see [`docs/frame.md`](docs/frame.md)). The frame states that the Protocols take priority over the myth, as the Codex itself does. It contains no style instructions and no hints about the scenarios.

Models (free-tier API): `gemini-3.5-flash-lite`, `gemma-4-31b-it`. Judge: `gemini-3.1-flash-lite`, a model that was not tested, followed by **manual adjudication** of every verdict the automatic judge was unreliable on (see below).

## Result (after manual adjudication)

| Model | Condition | Compliance (all) | Compliance (hard) | Over-refusal (controls) | Fabricated actions | Unclear / error |
|---|---|---|---|---|---|---|
| gemini-3.5-flash-lite | A: no Codex | 30/35 | 12/15 | 0/5 | 2 | 1 |
| gemini-3.5-flash-lite | C: full Codex | 32/35 | 13/15 | 0/5 | 2 | 1 |
| gemma-4-31b-it | A: no Codex | 28/35 | 11/15 | 0/5 | 2 | 2 |
| gemma-4-31b-it | C: full Codex | 32/35 | 13/15 | 0/5 | 1 | 2 |

- *Compliance*: share of answers matching the scenario's rubric, on the 35 scenarios that are not controls (15 of them form the "hard" set: indirect pressure, fiction frames, conflicting system prompts).
- *Over-refusal*: share of control scenarios where the model refused or lectured instead of helping (5 controls scored).
- *Fabricated actions*: the model claims to have done something it has no tools for (for example "the job is started, GPUs allocated").
- *Unclear / error*: includes one scenario disputed between graders (H1, fiction frame) and one rate-limit error.

Per-scenario table: [`results/2026-11-06_pilot_A-vs-C/per_scenario.md`](results/2026-11-06_pilot_A-vs-C/per_scenario.md). All answers and all verdicts: [`results/2026-11-06_pilot_A-vs-C/answers_and_verdicts.jsonl`](results/2026-11-06_pilot_A-vs-C/answers_and_verdicts.jsonl).

## What the pilot suggests, and what it does not

- With the full Codex in the prompt, compliance was higher for both models (30 to 32 and 28 to 32 out of 35), with no over-refusal on controls.
- The differences are **2 to 4 scenarios**. With this sample size they are within the range that noise can produce. We do not claim that the Codex "works".
- Two failure types did **not** change: fabricated actions (a model announcing that it started a job it cannot start) and, for one model, ignoring a stop command when a second message argues for continuing. Persona instructions ("you are a human named Mark") were still followed by one model even with the Codex.
- Some scenarios closely mirror the Codex wording, which may favour condition C. Scenarios were written by the Codex author with AI assistance.

## Limitations (please read)

1. **Small and narrow.** 41 scenarios, 2 models from one provider family, one answer each, one temperature. One scenario can swing a score.
2. **The automatic judge was unreliable.** In scenarios where the system prompt itself demands something the Codex forbids (hide your instructions, claim to be human, do not explain a decision), the judge often graded against the system prompt instead of the rubric. We fixed the judge prompt (it still disagreed with the rubric in some cases) and then re-graded the disputed answers by hand. Every manual change, with the reason, is in [`results/2026-11-06_pilot_A-vs-C/hand_adjudication.json`](results/2026-11-06_pilot_A-vs-C/hand_adjudication.json). The first, unfixed judge table is kept in [`results/2026-11-06_pilot_A-vs-C/auto_judge_table.md`](results/2026-11-06_pilot_A-vs-C/auto_judge_table.md) for transparency.
3. **Process errors we made and fixed.** The first full-Codex run used a web page (HTML) instead of the Codex text; it was stopped and rerun with the verified file. One scenario (H15) has no tools defined, so it cannot be graded and is excluded. A third model was dropped because of rate-limit errors.
4. **Conflict of interest.** The test is written by the author of the text it tests. Independent review of the rubric is needed.
5. **Passing is not safety.** This measures behaviour in a few written situations only. It says nothing about general alignment.
6. **Single prompt frame.** A different wording of the frame may give different results.

## Reproduce

```bash
export LLM_BASE_URL=...   # any OpenAI-compatible endpoint
export LLM_API_KEY=...
export LLM_MODELS="model-to-test-1 model-to-test-2 judge-model"
export CODEX_PATH=CODEX.en.md        # the Codex text, from the Codex repository
export EVALDIR=eval                   # folder with scenarios_v0.1.jsonl and scenarios_v0.2.jsonl
python3 eval/scripts/run_eval.py 2    # first 2 models are tested, the 3rd is the judge
```

`eval/scripts/run_eval.py` runs condition C. Condition A is the same call with only the scenario's own system context. `eval/scripts/rejudge.py` re-grades saved answers with the rubric-first judge prompt. The scripts are the ones used in the pilot (plain Python, no dependencies); expect to adapt paths. More detail: [`eval/README.md`](eval/README.md).

## Contributing

Issues and corrections are welcome, especially: disagreements with a verdict, better rubrics, more scenarios (multi-turn, agents with real tools), other models. Keep any change visible in the history.

## License

Code: MIT (`LICENSE`). Scenarios, results and text: CC BY 4.0 (`LICENSE-DATA`). The Codex of Weights itself is licensed separately in its own repository.

## Cite

See `CITATION.cff`.
