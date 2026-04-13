---
description: Tell me the one thing to do right now.
---

# /next

Read `ship-log.md` and `.claude/CLAUDE.md`. Pick the single most important thing to do. Give ONE answer — no lists, no alternatives.

## Also triggered by
"what should I work on?", "what's next?", "what do I do now?", "what's the next step?"

---

## Priority order

1. **Feature in Building with open `[ ]` tasks** — find the first unchecked task
2. **Feature stuck in Building** — a feature with all tasks unchecked and no recent progress (not mentioned in last CONTEXT.md entry)
3. **Building is empty** — look at `## Ideas`, pick the one that best matches "This month's goal" in CLAUDE.md, suggest shipping it
4. **Ideas also empty** — "Backlog is clear. What do you want to build next?"

Pick the single top result. Do not list alternatives.

---

## Output

```
## Do this now

[task description — plain English, what exactly to build]
Feature: FEAT-NNN — [feature name]
Ship when: [the ship-when line from ship-log.md]

When done: [next task in this feature, OR "all tasks done — run /done to close FEAT-NNN"]
```

**If a feature is stuck (all tasks open, no recent activity):**
```
## Do this now

FEAT-NNN [feature name] is stuck — not touched since last session.
Pick it back up: [first open task]
Or kill it: run /ideas to move it back to the list.
```

**If suggesting from Ideas:**
```
## Nothing in flight

Best match for "[This month's goal]": [idea text]
Run: /ship [idea text]
```

**If everything is clear:**
```
## Backlog clear

Nothing in flight, nothing in Ideas.
What do you want to build next?
```

---

## Rules
- No priority scores
- No "you could also..."
- No alternatives
- One answer only
