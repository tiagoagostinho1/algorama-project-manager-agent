---
description: List all available PM commands.
---

# /pm:help

```
/pm:feature [desc]   — End-to-end: description → story → tasks → code → done
/pm:spec [desc|ID]   — Generate a spec before building (behaviors, edge cases, states)
/pm:briefing         — Current project state (tasks + story context)
/pm:next             — What to do right now (1 thing, hands off to PO if empty)
/pm:done             — Close session + save context
/pm:task [txt|#ID]   — Add a task, or view full detail for an existing one
/pm:implement [ID]   — Look up a task and give Claude Code everything it needs to build it
/pm:sync             — Reconcile last session against open tasks — propose and apply updates
/pm:unblock [ID]     — Resolve a blocked task — decision, workaround, or spike
/pm:review           — Weekly review: done, slipped, stale, overdue
/pm:standup          — Weekly report ready to share
/pm:help             — This list
```

Files used:
- `project-tasks.md` — task list (project root, created automatically if missing)
- `.claude/CONTEXT.md` — session history
- `.claude/CLAUDE.md` — project context (edit this once per project)
