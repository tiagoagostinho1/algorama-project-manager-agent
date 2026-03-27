---
description: List all available PM commands.
---

# /pm:help

```
/pm:briefing   — Current project state
/pm:next       — What to do right now (1 thing)
/pm:done       — Close session + save context
/pm:task [txt] — Add a new task
/pm:review     — Weekly review: done, slipped, stale, overdue
/pm:standup    — Weekly report ready to share
/pm:help       — This list
```

Files used:
- `project-tasks.md` — task list (project root, created automatically if missing)
- `.claude/CONTEXT.md` — session history
- `.claude/CLAUDE.md` — project context (edit this once per project)
