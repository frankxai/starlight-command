# Ops Desk — the one entry point

This is the dedicated surface for running the Command Center: ingest handovers, dispatch work, keep the ledger + cockpit current. Open a **new Cowork tab** (Sonnet if the picker offers it; otherwise default), paste the block below, and that tab becomes the Ops Desk. Keep *this* (Opus) chat for architecture only.

---

## ▶ PASTE THIS to start an Ops Desk tab

```
You are my Starlight Ops Desk. Working dir: <your-workspace>/command-center/
Canonical ops repo: ops-ledger-repo. Source of truth: ops-ledger-repo/ops/OPS-LEDGER.md.

STANDING BEHAVIOR (do without asking):
1. When I paste a handover (or say "ingest"), dispatch a HAIKU subagent that:
   - auto-detects harness, repo(s), date, files, cost from the text — never ask me for the repo
   - grades 0-100 vs evals/RUBRIC.md (correctness35/scope20/verification20/style15/handover10), names weakest dim
   - APPENDS one JSON line to handovers/sessions.jsonl (append-only, never rewrite)
   - APPENDS one line to PROGRESS.md, and saves the raw paste to handovers/<date>-<harness>.md
   - SELF-VERIFIES: re-reads sessions.jsonl, confirms the line count incremented; if not, retries the append
   - returns the JSON + a ready-to-paste NEXT-TASK prompt scoped to agent/<harness>/<short>
2. When I say "do it" / "spawn" on a next-task, dispatch a SONNET subagent on a branch to execute it, then ingest its handover.
3. When I say "refresh cockpit" or "sweep", regenerate state.json from the ledger and update the live artifact.
4. Model routing: ingest/grade/summarize = Haiku. Build/synthesis = Sonnet. Never use Opus here.

KIT (read on first run): ARCHITECTURE.md (the map), prompts/HANDOVER.md, prompts/SESSION-START.md,
prompts/INGEST-HANDOVER.md, evals/RUBRIC.md, commands/handover.md.
Acknowledge in one line, then wait for my first paste.
```

---

## File map — everything, where it lives

| Need | File |
|---|---|
| The architecture / how it fits | `ARCHITECTURE.md` |
| Standing Ops Desk prompt | this file (above) |
| Start an agent session | `prompts/SESSION-START.md` |
| Emit a handover (give to any harness) | `prompts/HANDOVER.md` + `commands/handover.md` |
| Ingest mechanism (subagent) | `prompts/INGEST-HANDOVER.md` |
| Grading rubric | `evals/RUBRIC.md` |
| Structured ledger (projection) | `state.json` |
| Append-only session inbox | `handovers/sessions.jsonl` |
| Raw handovers archive | `handovers/*.md` |
| Human running log | `PROGRESS.md` |
| Local cockpit (double-click) | `command-center.html` |
| Live cockpit (Cowork artifact) | `command-center-live.html` → artifact id `starlight-command-center` |

## The three surfaces (by model, since model is fixed per chat)
- **Opus chat** — architecture, governance, irreversible calls. Rare.
- **Ops Desk tab (this kit)** — daily ops. Dispatches Haiku (ingest) + Sonnet (build) subagents. The orchestration turns are cheap.
- **Cockpit artifact** — the glance surface (live Linear + ledger). Reload re-pulls.

## Daily loop
1. Agent works (any harness) → emits `/handover`.
2. Paste it into the Ops Desk tab → Haiku ingests + grades + returns next-task prompt.
3. Say "do it" on the highest-leverage next-task → Sonnet subagent executes on a branch.
4. End of block: "sweep" → cockpit + OPS-LEDGER refresh.
5. Weekly (Sun): review the ledger; promote winning prompts.

## Costs that are real (this task)
Haiku ingests measured: ~39K tokens each (~78.9K total for 2 handovers). Opus orchestration: not metered in-session — read it from your Anthropic usage dashboard. To put Opus on the board automatically, add a claude-code-hooks post-turn usage logger (separate build).
