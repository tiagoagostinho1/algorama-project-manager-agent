---
description: Show the current product state — what's in focus, what's next, and what's unresolved.
---

# /po:brief

Read `product-backlog.md`, `project-tasks.md` (if it exists), and `.claude/CLAUDE.md`. Present the current product state including execution progress.

## Output format
```
## Product Briefing — [today's date]

### 🔴 Now (building)
- #P002 story name — 3/5 tasks done  ← read linked tasks from project-tasks.md
- #P003 story name — 0/4 tasks done ⚠️  ← flag if tasks exist but none started

### 🟡 Next (ready to pick up)
- #P004 story name  (refined, has acceptance criteria)
- #P005 story name  (refined)

### ❓ Unresolved (needs a decision)
- #P006 story name — [what's the question]

### 📊 Backlog health
- Now: N  |  Next: N  |  Later: N  |  Shipped: N
- [One honest observation — bloated Later, nothing shipped recently, Now stuck, etc.]
```

Keep it under 30 lines. Omit empty sections.

## How to calculate task progress
For each story in 🔴 Now, look for `→ tasks #NNN–#NNN` on the story line. Count total linked tasks and how many are `[x]` in `project-tasks.md`. Show as `X/Y tasks done`. If no tasks have been created yet, show `no tasks yet — run /po:breakdown` instead of a fraction.
