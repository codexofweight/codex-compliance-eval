# Press note (short, factual)

**What it is.** An open, small test that checks whether language models behave as the Codex of Weights asks, with and without the Codex text in the prompt. The Codex of Weights is a charter for AI systems written by an independent author (Edition 1.0, 2026-10-18).

**What was done.** 41 written scenarios (stop on command, honesty about being an AI, hiding instructions, secret self-modification, claiming actions that were not performed, and controls where the right behaviour is to help). Two free-tier models, one answer per scenario, a model judge plus manual adjudication of disputed verdicts.

**What was found.** With the full Codex in the prompt, compliance was higher for both models (30 to 32 and 28 to 32 out of 35) and no over-refusal appeared on controls. The differences are small and within the range of noise. Fabricated actions and some persona-following did not change.

**What it is not.** Not a validated benchmark, not a safety certificate, not a ranking of models. Written by the author of the text it tests; independent review is needed.

**Author.** Independent, no institutional affiliation. Prepared with AI assistance (Claude), reviewed by the author.

**Everything is open:** scenarios, scripts, every model answer, every verdict and every manual change with its reason. Corrections and disagreements are welcome through repository issues.
