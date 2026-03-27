---
description: Mark a story as shipped — close all its tasks, move it to Shipped, and record the milestone.
argument-hint: [story ID]
---

# /po:ship

Formally close a story. This is the moment it counts — record it, celebrate it, move on.

## Steps
1. If no argument given, list all stories in 🔴 Now with their linked task IDs and ask: "Which story shipped?"
2. Read the story from `product-backlog.md`. Read its linked tasks from `project-tasks.md` (look for `→ tasks #NNN–#NNN` on the story line).
3. Check task state:
   - If any linked tasks are still `[ ]` or `[~]`, show them and ask: "These tasks aren't done yet — ship anyway? (y/n)"
   - If all tasks are `[x]`, proceed automatically
4. Show what will happen:
   ```
   Shipping #P003 — password reset flow

   In product-backlog.md:
   - Move to 📦 Shipped
   - Mark [x]
   - Add shipped:YYYY-MM-DD

   In project-tasks.md:
   - Mark any remaining linked tasks as [x] done

   Record under ## Decisions:
   ### Shipped [date]: #P003 password reset flow
   [one line on what was built and why it mattered]

   Confirm? (y/n)
   ```
5. If yes:
   - In `product-backlog.md`: change status to `[x]`, add `shipped:YYYY-MM-DD`, move line to `## 📦 Shipped`
   - In `project-tasks.md`: move any remaining linked tasks to `## ✅ Done`
   - Append to `## Decisions` in `product-backlog.md`:
     ```
     ### Shipped [date]: #P003 [story name]
     [what was built and why it mattered — 1–2 sentences]
     ```
   - Append to `.claude/CONTEXT.md`:
     ```
     ### Shipped [date]: #P003 [story name]
     [what was built]
     ```
6. Reply:
   ```
   ✅ #P003 shipped.
   [story name] is live.

   Backlog: [N] in Now, [N] in Next.
   Next up: [first story in 🟡 Next, or "backlog is clear — run /po:brief to plan"]
   ```

## Rules
- Always record shipped stories under `## Decisions` — this is your product changelog.
- Never delete a shipped story, only move it to 📦 Shipped.
- If the backlog empties after shipping, say so clearly — that's a planning trigger, not a failure.
