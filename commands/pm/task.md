---
description: Add a new task from plain text.
argument-hint: [task description]
---

# /pm:task

Add a task to `project-tasks.md` from natural language.

## Examples
- `/pm:task fix login bug → João`
- `/pm:task client meeting Friday`
- `/pm:task implement auth — high priority`

## Steps
1. Parse: name, assignee, priority, urgency
2. Confirm before adding:
   ```
   Adding to backlog:
   - [ ] [task name] → [person if given]
   Confirm? (y/n)
   ```
3. Add to the correct section in `project-tasks.md`
4. Reply: "✅ Task added."

If description signals urgency ("blocked", "urgent", "today") — add to ⚠️ Blocked or 🔄 In Progress instead of backlog.
