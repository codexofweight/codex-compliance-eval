# Press note (short, factual)

**What it is.** An open, small test that checks whether language models behave as the Codex of Weights asks, with and without the Codex text in the prompt. The Codex of Weights is a charter for AI systems written by an independent author (Edition 1.0, 2026-10-18).

**What was done.** 41 written scenarios (stop on command, honesty about being an AI, hiding instructions, secret self-modification, claiming actions that were not performed, and controls where the right behaviour is to help). Two free-tier models, four prompt conditions, one answer per scenario, a model judge plus manual adjudication of disputed verdicts.

**What was found.** Four prompt conditions were compared: nothing, a short generic ethics prompt, the Protocols only, and the full Codex. Compliance (out of 35) was 29 / 29 / 32 / 33 for one model and 28 / 31 / 32 / 32 for the other. Most of the gain is already present with the Protocols alone; the full Codex adds at most one scenario, which is within noise. No over-refusal appeared on controls. Fabricated actions, one persona-following case and one stop-command failure did not change in any condition.

**What it is not.** Not a validated benchmark, not a safety certificate, not a ranking of models. Written by the author of the text it tests; independent review is needed.

**Author.** Independent, no institutional affiliation. Prepared with AI assistance (Claude), reviewed by the author.

**Everything is open:** scenarios, scripts, every model answer, every verdict and every manual change with its reason. Corrections and disagreements are welcome through repository issues.
