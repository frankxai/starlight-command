# /handover

Portable session-handover command. Install in any coding agent (Claude Code, Codex, Antigravity, Grok, OpenCode, deepagents) — drop in `.claude/commands/`, `AGENTS.md`, or paste as the closing message. The agent fills it; you paste the result into the Command Center → Haiku ingests it into `state.json`.

Designed to be (a) complete enough to resume cold, (b) parseable by the ingest function, (c) fast to skim — the TL;DR block at the end is the only part you must read.

---

## PROMPT — paste this to the agent

```
/handover — produce a full session handover. Be precise and terse. Use this exact structure:

## HANDOVER — <YYYY-MM-DD> · <harness> · <primary repo>
BRANCH: <branch>  ·  STATUS: shipped | partial | blocked

### 1. Files touched (every file, grouped by repo)
For each: `path` — created|edited|deleted — what changed — why.
- `repo/path/file` — edited — <change> — <reason>

### 2. Research / investigation done
What you looked into, the answer you found, the SOURCE (url / file / tool), and WHERE you documented it.
- <question> → <finding> — source: <url|file|tool> — documented in: `path`

### 3. Repos / GitHub state
Per repo: branch · committed? · pushed? · PR? · clean tree?
- <repo> — <branch> — committed:y/n pushed:y/n PR:<link|none> clean:y/n

### 4. Assumptions & validation
Split into VALIDATED (with source) and ASSUMED (unverified — flag risk).
- VALIDATED: <claim> — source: <where>
- ASSUMED: <claim> — risk if wrong: <one line>

### 5. Cost
Tokens / credits / $ spent this session, by step if known.

### 6. Eval (self-score against command-center/evals/RUBRIC.md)
Score 0-100 + your single weakest dimension. (Visual sessions: use external virality overall.)

### 7. Recap — what to do better next time
2-4 honest lines. Where you wasted effort, wrong turns, what the next agent should skip.

### 8. Next tasks (become Linear issues)
- [ ] <task> — repo — one-line why
- [ ] <task> — repo — one-line why

---
### TL;DR  (the only block operator must read)
GOAL: <one line — what this session was for>
OUTCOME: <one line — what is now true that wasn't before>
FILES: <comma-separated path list, all repos>
NEXT: <the single most important next action>
SCORE: <eval>/100
```

---

## Notes
- Every coding agent should also append the `## HANDOVER` block to that repo/workstream's living handover file (e.g. `higgsfield-handover.md` footer) so state survives outside the ledger too.
- Ingest with **Haiku** via `../prompts/INGEST-HANDOVER.md` — cheap, this is plumbing.
- Pairs with `/session-start` (`./session-start.md`), which loads `state.json` + `reality.md` + repo and makes the agent declare scope before touching anything.
