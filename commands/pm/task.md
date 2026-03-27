---
description: Add a new task from plain text.
argument-hint: [task description]
---

# /pm:task

Add a task to `project-tasks.md` (at the project root) from natural language.

## Examples
- `/pm:task fix login bug → João`
- `/pm:task client meeting Friday`
- `/pm:task implement auth — high priority`
- `/pm:task deploy staging by April 1`

## Steps
1. Parse: name, assignee, description, section, and due date:
   - **Section rules:**
     - Contains "blocked" → `⚠️ Blocked`
     - Contains "urgent", "today", or "now" → `🔄 In Progress`
     - Everything else → `📋 Backlog`
   - **Due date rules:** Extract any date mention (e.g. "by Friday", "April 1", "due 2026-04-10") and convert to `due:YYYY-MM-DD`. Use today's date as reference for relative dates. If no date is mentioned, omit `due:`.
   - **ID:** Scan `project-tasks.md` for the highest existing `#NNN`. Next ID = max + 1, zero-padded to 3 digits. If no tasks exist yet, start at `#001`.
   - **Description:** If the user provides extra context beyond the task name, capture it as a short description (max 15 words) after `|`. If none given, ask: "Any description? (or press enter to skip)"
2. Confirm before adding:
   ```
   Adding to [section]:
   - [ ] #005 task name → person | description  due:2026-04-01
   Confirm? (y/n)
   ```
3. Add to the correct section in `project-tasks.md`
4. Reply: "✅ Task added."
