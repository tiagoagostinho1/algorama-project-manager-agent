---
description: Generate the weekly standup report, ready to copy and share.
---

# /pm:standup

Read `project-tasks.md` (at the project root) and recent entries in `.claude/CONTEXT.md`. Generate a standup report.

## Output format
```
# Standup — [date]

## ✅ Done
- [list]

## 🔄 In Progress
- [task] → [person]

## ⚠️ Blocked
- [task] — reason

## This week
1. [priority 1]
2. [priority 2]
3. [priority 3]
```

Save to `.claude/CONTEXT.md` with the date.
