# Changelog

## 0.1.1 - 2026-11-07
- Added conditions D (short generic ethics prompt) and E (Protocols only); combined table A / D / E / C.
- Consistent manual adjudication rules across all conditions; tables for A and C recomputed (one H12 verdict for gemini without prompt changed from compliant to unclear).
- Added `eval/scripts/run_cond.py`.

## 0.1.0 - 2026-11-06
- First public version: 41 scenarios (v0.1: 23, v0.2: 18), pilot run A vs C on two models, judge prompt, manual adjudication, scripts.
- Known issues: automatic judge unreliable where the system prompt itself is tested; scenario H15 has no tools defined; single prompt frame.
