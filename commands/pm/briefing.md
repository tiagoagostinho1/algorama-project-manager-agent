---
description: Show current project state — blocked, in progress, and up next.
---

# /pm:briefing

Read `project-tasks.md` (at the project root) and the last entry in `.claude/CONTEXT.md`. Present the current state grouped by priority.

## Output format
```
## Briefing — [today's date]

### ⚠️ Blocked
- #004 task — reason

### 🔄 In Progress
- #002 task → person  due:2026-04-01

### 📋 Up next (top 3)
- #001 task  due:2026-03-30 ⚠️  ← append ⚠️ if overdue
- #005 task

### Last session
[one line summary from CONTEXT.md]
```

Keep it under 20 lines.
