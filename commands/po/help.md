---
description: List all available PO commands.
---

# /po:help

```
/po:brief            — Current product state (Now with task progress, Next, unresolved)
/po:story [txt]      — Add a feature idea or story (+ sync to ClickUp/Jira/etc. if connected)
/po:validate [ID]    — Challenge a story before building it — problem, evidence, urgency
/po:refine [ID]      — Sharpen a story and move it to Next (adds acceptance criteria)
/po:breakdown [ID]   — Break a refined story into tasks in project-tasks.md
/po:prioritize       — Re-order the backlog by value vs. effort
/po:ship [ID]        — Mark a story as shipped, close tasks, record the milestone
/po:help             — This list
```

Files used:
- `product-backlog.md` — stories and product decisions (project root, created automatically if missing)
- `.claude/CLAUDE.md` — project context (edit this once per project)
- `.claude/CONTEXT.md` — session history

PM commands (execution-level tasks): `/pm:help`
