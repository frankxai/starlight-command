# Ingest a handover — the subagent mechanism

> Forget the artifact's askClaude tab (the sandbox doesn't reliably expose it). The real, subscription-native ingest is a **Cowork subagent pinned to Haiku**. This Opus chat only dispatches; Haiku does the work. Proven 2026-06-16 on the Grok handover (graded 92, appended to sessions.jsonl).

## How you use it
Paste a handover into chat and say **"ingest"** (or nothing — a bare handover paste is enough). Claude dispatches a Haiku subagent that:
1. **Auto-detects** harness, repo(s), date, files, cost from the text — you never type the repo.
2. **Grades** 0–100 vs `evals/RUBRIC.md`, names the weakest dimension.
3. **Appends** one line to `command-center/handovers/sessions.jsonl` (append-only — robust, no full-file rewrite).
4. **Appends** one line to `PROGRESS.md`.
5. **Returns** the JSON row **+ a ready-to-paste next-task prompt** for that repo.

`sessions.jsonl` is the durable inbox; `/ops-sweep` folds it into `state.json` + `OPS-LEDGER.md` for the cockpit.

## The dispatch prompt (what Claude runs as a Haiku subagent)
```
Ingest ONE agent handover. Be terse. Do exactly this:
1. Parse the pasted handover. Auto-detect harness, repo(s), date, files, credits — do not ask.
2. Grade 0-100 vs rubric (correctness35/scope20/verification20/style15/handover10); name weakest dim.
3. Build ONE compact JSON line: {id:"s-<YYYYMMDD>-<harness3>",date,harness,repo,scope,files,credits,eval,weakest_dim,status,next}
4. APPEND it as a new line to command-center/handovers/sessions.jsonl (create if missing; never rewrite existing lines).
5. APPEND one line to command-center/PROGRESS.md under the header.
6. Return the JSON + a ready-to-paste NEXT-TASK prompt for the top item in `next`, scoped to a branch agent/<harness>/<short>.
Model: Haiku. No other files.
```

## Model routing (by surface, since the model is fixed per chat)
- **This Opus chat** = orchestrator. Dispatches subagents, makes architecture calls. Few tokens.
- **Haiku subagent** = every ingest/grade. ~tens of K tokens, pennies.
- **Sonnet subagent** = when you say "do it" — spawn a builder agent on a branch to actually execute a `next` item.

## Spawning work (not just logging it)
Same pattern, bigger job: "spawn an agent to fix R1" → Claude dispatches a **Sonnet** subagent on `agent/claude/<scope>` that does the work and emits a `/handover` — which you then ingest. The loop closes on itself.
