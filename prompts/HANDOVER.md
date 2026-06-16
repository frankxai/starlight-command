# Handover — emit this at the end of every session

Keep it parseable. The ingest step (Haiku) reads this directly into a `state.json` row.

```
## Handover — <YYYY-MM-DD> · <harness> · <repo>

SCOPE: <one line — what this session was for>
STATUS: shipped | partial | blocked

DID:
- <change 1>
- <change 2>

FILES TOUCHED:
- `path/to/file` — <why>
- `path/to/file` — <why>

PROMPTS GIVEN (log each, verbatim or summarized):
- "<prompt>" → <result / eval if known>

COST: <tokens / credits / $ if known>
EVAL: <self-score 0-100 against RUBRIC, or external score e.g. virality overall N>

NEXT / FOLLOW-UPS (these become Linear issues):
- [ ] <next step>
- [ ] <next step>

STATE CHECK: <branch name> · <committed? pushed?> · <clean working tree? y/n>
```

Append the same block to the relevant living handover file (e.g. `higgsfield-handover.md` keeps its session log footer).
