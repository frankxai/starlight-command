# Agentic Ops — the one architecture

> How every piece fits. Read this first. It reconciles three things that grew in parallel across harnesses into **one system with one write-path**.

## The problem it solves
You run 6 harnesses across 44 repos and 5 brands. Three overlapping "command center" efforts appeared: the ASPH ops ledger (Antigravity), this structured ledger + cockpit (Claude), and an Electron shell. Left alone they drift — the exact split-brain your own `AGENTS.md §4` warns against. This collapses them into layers of one system.

## Canonical layers — who owns what

| Layer | Artifact | Role | Status |
|---|---|---|---|
| **Narrative SoT** | `ops-ledger-repo/ops/OPS-LEDGER.md` + `ops/sessions/<date>.md` | The single source of truth. Fronts, risks, done, Linear map. Git-versioned, cheap (git-delta). | **canonical** |
| **Structured projection** | `command-center/state.json` | Machine-readable rollup the cockpit reads. Generated from the ledger + handovers. | **generated** |
| **Visual face** | `command-center/command-center-live.html` (Cowork live artifact) | The cockpit. Reads `state.json` + pulls Linear live + Haiku ingest. | **view** |
| **Capture daemon** | ASPH (Antigravity, in `ops-ledger-repo`) | Auto-detects sessions (git/terminal), feeds the ledger. | **writer (auto)** |
| **Per-agent emit** | `/handover` → `command-center/handovers/` | Each harness emits a parseable handover at session end. | **writer (manual)** |
| **Desktop shell** | `repos/starlight-command-center` (Electron) | Optional native window that can load the cockpit later. | **optional** |
| **Trackers / mirrors** | Linear (brandc), Obsidian `Ops/`, GitHub | Action surface, human glance, code truth. Projections, never the source. | **mirror** |

Rule: **edit upstream, never the projection.** Don't hand-edit `state.json` — change the work, run `/ops-sweep`, the projection regenerates.

## The one write-path

```
 harness works ──► /handover ──► command-center/handovers/<date>-<harness>.md
                                          │
            ASPH daemon (git/terminal) ───┤
                                          ▼
                                   /ops-sweep   (git-delta, Haiku — cheap)
                                          │
                      ┌───────────────────┼───────────────────┐
                      ▼                   ▼                   ▼
              OPS-LEDGER.md          state.json          Linear (gated)
              (narrative SoT)     (structured proj.)     + Obsidian mirror
                                          │
                                          ▼
                              command-center cockpit (live artifact)
                                  + live Linear overlay
```

## Commands — how they compose
- **`/session-start`** — agent reads ledger + reality.md + repo, declares scope on a branch. (start)
- **`/handover`** — agent emits the full structured handover. (end of every session)
- **`/ops-sweep`** — aggregates git deltas + new handovers → refreshes OPS-LEDGER + state.json + NEXT-PROMPTS, mirrors Obsidian, gates Linear. (end of working block)
- **Cockpit Ingest tab (Haiku)** — paste a single handover for an instant graded `state.json` row without a full sweep.

## Model routing (the cost lever)
- **Haiku** — `/ops-sweep`, handover ingest, eval grading, summaries. The plumbing.
- **Sonnet** — daily building, synthesis, mid-complexity code. Your default chat.
- **Opus** — architecture, governance, irreversible calls. This chat only.
Per chat the model is fixed — so route by *which surface you're in*, not by switching mid-conversation.

## Operating procedure
**Per session (any harness):** `/session-start` → work on one branch → `/handover`. Paste the handover into the cockpit Ingest tab (Haiku) or let the ASPH daemon catch it.

**End of working block:** `/ops-sweep` in `ops-ledger-repo`. Refreshes everything for ~free.

**Daily:** open the cockpit. Work the **Needs-you-now** rail top-down (today: R1 branda→brandb bridge, R2 hosting-provider domains).

**Weekly (Sun):** palace review reads OPS-LEDGER; promote winning prompts to the standard set; reconcile drift.

## Next consolidation moves (in order)
1. **Make `/ops-sweep` emit `state.json`** alongside OPS-LEDGER.md (one generator, two formats) — kills the last manual step.
2. **Point the ASPH daemon's output at `command-center/handovers/`** so auto-capture and manual `/handover` share one inbox.
3. **Command Center MCP** (FastMCP, ~5 tools: `claim_scope`, `log_session`, `log_prompt`, `grade_output`, `get_attention`) — the shared spine every harness writes to. Replaces copy-paste entirely.
4. **Electron shell loads the live cockpit** — one window, always on.

The system is correct the moment there is exactly one source of truth and everything else is generated from it. We're there in design; moves 1–2 make it true in practice.
