---
description: Show current project state — blocked, in progress, and up next.
---

# /pm:briefing

Read `.claude/tasks/project-tasks.md` and the last entry in `.claude/CONTEXT.md`. Present the current state grouped by priority.

## Output format
```
## Briefing — [today's date]

### ⚠️ Blocked
- [task] — reason

### 🔄 In Progress
- [task] → [person]

### 📋 Up next (top 3)
- [task]

### Last session
[one line summary from CONTEXT.md]
```

Keep it under 20 lines.
