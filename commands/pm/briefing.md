---
description: Show current project state — blocked, in progress, and up next.
---

# /pm:briefing

Read `project-tasks.md`, `product-backlog.md` (if it exists), and the last entry in `.claude/CONTEXT.md`. Present the current state grouped by priority.

## Output format
```
## Briefing — [today's date]

### ⚠️ Blocked
- #004 task — reason

### 🔄 In Progress
- #002 task → person  due:2026-04-01  [#P003 password reset]  ← story context if task links to a story
- #006 task  [no story]

### 📋 Up next (top 3)
- #001 task  due:2026-03-30 ⚠️  ← append ⚠️ if overdue
- #005 task  [#P005 onboarding]

### 🗺 Product pulse
- Now: #P003 password reset (2/4 tasks done), #P005 onboarding (0/3 tasks done)
- Next story ready: #P007 dark mode
```  ← omit "Product pulse" entirely if product-backlog.md doesn't exist

### Last session
[one line summary from CONTEXT.md]
```

Keep it under 25 lines.

## How to link tasks to stories
A task links to a story when the story line in `product-backlog.md` contains `→ tasks #NNN–#NNN` or `→ tasks #NNN, #NNN`. Match task IDs to find the parent story and show it in brackets. If no match, show `[no story]` only if there are orphan tasks worth flagging — otherwise omit.
