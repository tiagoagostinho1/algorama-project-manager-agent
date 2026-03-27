---
description: Tell me the ONE thing to do right now.
---

# /pm:next

Read `project-tasks.md` (at the project root) and `product-backlog.md` (if it exists). Check today's date. If the backlog has more than 20 tasks, consider only the first 20 (top = highest priority). Apply this priority order:
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
Story: #P002 password reset flow  ← include if task links to a story; omit if no story
Due: 2026-04-01 (overdue by 3 days)   ← omit if no due date
What: [task description if present, else 1 sentence from context]
When done: [logical next step — next task in the same story if one exists]
```

## When the task backlog is empty
If there are no tasks in Backlog or In Progress (only Done/Blocked):
```
## ⚡ Backlog is clear

Nothing left to build right now.
→ Run /po:brief to see what story to pick up next.
→ Run /po:breakdown [ID] to turn a story into tasks.
```
Do not invent tasks. Do not suggest generic work. Just hand off to the PO.
