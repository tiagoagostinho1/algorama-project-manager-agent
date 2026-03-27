---
description: Tell me the ONE thing to do right now.
---

# /pm:next

Read `project-tasks.md` (at the project root). Check today's date. If the backlog has more than 20 tasks, consider only the first 20 (top = highest priority). Apply this priority order:
1. Blocked tasks waiting on a decision
2. Overdue tasks (`due:` date is before today)
3. Tasks due today
4. Tasks in progress
5. First backlog task by ID order

Pick the single most urgent task. Give ONE answer only — no lists, no alternatives.

## Output format
```
## ⚡ Do this now

**#003 task name**
Due: 2026-04-01 (overdue by 3 days)   ← omit if no due date
What: [task description if present, else 1 sentence from context]
When done: [logical next step]
```
