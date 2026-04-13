---
description: Quick snapshot of where things are.
---

# /status

Read `ship-log.md` and `.claude/CONTEXT.md`. Show a concise snapshot. Always under 20 lines.

## Also triggered by
"what's the status?", "where are we?", "give me a summary", "what are we working on?"

---

## Output format

```
## Status — [date]

### Building
- FEAT-003 CSV export — 2/4 tasks done
- FEAT-005 email notifications — not started

### Done this month
- FEAT-001 auth (shipped 2026-03-15)
- FEAT-002 onboarding (shipped 2026-04-02)

### Ideas waiting
- dark mode, team accounts, mobile app

Last session: [one line from the most recent CONTEXT.md entry]

Commands: /ship [idea] · /next · /done · /ideas [idea] · /status
```

---

## Rules

- **Building section:** show every feature currently in `## Building`. Include task progress (N/total done). If 0 tasks started, say "not started".
- **Done this month:** show features shipped in the last 30 days. If none, omit this section.
- **Ideas waiting:** comma-separated list from `## Ideas`. If empty, omit this section.
- **Last session:** pull the "Done:" line from the most recent `### [date]` entry in CONTEXT.md. If CONTEXT.md is empty or missing, omit this line.
- **No blocked section** unless there are actually blocked items — add it only when needed: `### Blocked · FEAT-NNN [name] — [reason]`
- No priority scores. No backlog health analysis. No "Now/Next/Later" labels.
- Always end with the command list — it's the only help text the user needs.
