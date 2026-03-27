---
description: Break a refined story into concrete PM tasks and add them to project-tasks.md.
argument-hint: [story ID]
---

# /po:breakdown

Take a story from 🟡 Next (or 🔴 Now) and translate it into actionable tasks for `project-tasks.md`.

## Steps
1. If no argument given, list all stories in 🔴 Now and 🟡 Next and ask: "Which story to break down?"
2. Read the story and its acceptance criteria from `product-backlog.md`.
3. Draft tasks — one task per acceptance criterion, plus any setup or integration work implied by the story. Keep each task name short and concrete (verb + object: "add email reset endpoint", "send reset email via SendGrid").
   - Default assignee: whoever is on the story (from `→` field), or leave blank if solo
   - Inherit the story's due date if it has one
   - Use the next available `#NNN` IDs from `project-tasks.md`
4. Show the proposed task list before writing anything:
   ```
   Breaking down #P003 — password reset flow

   Tasks to add to project-tasks.md (📋 Backlog):
   - [ ] #012 add /auth/reset-request endpoint → João  due:2026-04-10
   - [ ] #013 send reset email via SendGrid → João  due:2026-04-10
   - [ ] #014 add /auth/reset-confirm endpoint → João  due:2026-04-10
   - [ ] #015 add reset UI on login page → João  due:2026-04-10

   Add these tasks? (y/n)
   ```
5. If yes:
   - Append all tasks to `## 📋 Backlog` in `project-tasks.md`
   - Move the story to `## 🔴 Now (current focus)` in `product-backlog.md` if it isn't already there
   - Add a note on the story line: `→ tasks #012–#015`
6. Reply:
   ```
   ✅ #P003 broken down — 4 tasks added to project-tasks.md (#012–#015).
   Run /pm:briefing to see the updated task board.
   ```

## Rules
- Only break down stories that are in 🟡 Next or 🔴 Now. If the story is still in 🟢 Later, tell the user to refine it first with `/po:refine`.
- Keep tasks small enough to complete in one session. If a task feels like a day's work, split it.
- The story stays in `product-backlog.md` — don't remove it. Tasks and stories are two layers of the same work.
- If `project-tasks.md` doesn't exist yet, create it using the standard template before adding tasks.
