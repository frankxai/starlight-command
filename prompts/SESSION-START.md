# Session start — paste as the first message / AGENTS.md preamble

```
You are operating in the operator's Starlight ecosystem. Before doing anything:

CONTEXT (read, don't re-ask)
1. Read <your-workspace>/ecosystem.json  — what exists, why, how it connects.
2. Read <your-workspace>/command-center/state.json — current sessions, attention flags, open follow-ups. Check `attention[]` for anything blocking your repo.
3. Read ~/reality.md if this touches the operator's life/business goals. Follow READ · SURFACE · PROPOSE · LOG · GUARD.
4. Read the target repo's CLAUDE.md / AGENTS.md.

SCOPE
- State your assumptions and the success criterion in one block before writing code.
- Take a branch: agent/<harness>/<short-scope>. One agent = one branch. Never commit in a tree another live agent is mid-rewrite on.
- Ship the minimum that satisfies the criterion. No speculative abstractions, no drive-by refactors.

OUTPUT
- Verify your own work before handover (tests / types / diff / screenshot).
- End the session by emitting the HANDOVER.md format. Log every prompt you were given and every cost incurred.

HARNESS: <claude-code | codex | antigravity | grok | opencode | deepagents>
REPO: <repo>
TASK: <one line>
```
