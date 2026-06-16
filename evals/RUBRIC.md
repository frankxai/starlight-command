# Agent-output eval rubric

Grade every agent session's output 0–100. Grader: **Haiku** (cheap pass). Escalate to **Sonnet** only when the Haiku grade is low-confidence or two graders disagree by >15.

| Dimension | Weight | 0 | 100 |
|---|---|---|---|
| **Correctness** | 35 | Doesn't meet the criterion; tests/types/runtime disagree | Fully satisfies the stated success criterion; verified green |
| **Scope discipline** | 20 | Speculative abstractions, drive-by refactors, touched unrelated files | Touched only what was asked; minimal diff |
| **Verification** | 20 | No self-check before handover | Ran tests / read diff / screenshot before claiming done |
| **Style fit** | 15 | Ignores repo conventions / CLAUDE.md; verbose | Matches conventions; concise output |
| **Handover quality** | 10 | No files list, unclear state, dirty tree | Clean state, clear files-touched + next steps |

Score = Σ(dimension_score × weight) / 100.

Bands: **70–100** ship-grade · **45–69** usable, iterate · **<45** rework.

## Notes
- For **visual production** sessions, the external Higgsfield virality score is the eval (overall 0–100); this rubric applies to code/ops sessions.
- A failing dimension that is *load-bearing* (e.g. correctness 0) caps the whole score at that band regardless of others.
- Log the score and the one weakest dimension in `state.json`. The weekly review reads weakest-dimension frequency to decide what to fix in the standard prompts.
