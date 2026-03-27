---
description: Weekly review — what got done, what slipped, what's stale.
---

# /pm:review

Read `project-tasks.md` and the last 3 entries in `.claude/CONTEXT.md`. Check today's date.

## Steps
1. **Done this week** — tasks moved to ✅ Done in recent sessions
2. **Slipped** — tasks that were In Progress last session but are still In Progress (no movement)
3. **Stale** — tasks In Progress with no update for more than 7 days (use session dates in CONTEXT.md to infer)
4. **Overdue** — tasks with `due:` date before today that are not done
5. **What's next** — top 3 backlog tasks by priority order

## Output format
```
## 📋 Weekly Review — [date]

### ✅ Done this week
- #003 task name

### ⚠️ Slipped (was in progress, still not done)
- #002 task name | description  — [days since last session]

### 🧟 Stale (no movement in 7+ days)
- #001 task name | description

### 🔴 Overdue
- #004 task name  due:2026-03-20 (overdue by N days)

### 🔜 Up next
1. #005 task
2. #006 task
3. #007 task
```

Omit any section that has no entries. Keep the whole output under 30 lines.
