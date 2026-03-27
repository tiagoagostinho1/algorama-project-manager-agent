---
description: Close the session, update tasks, and save context for next time.
argument-hint: [what you completed]
---

# /pm:done

Close the session and update all files.

## Also triggered by
"done", "finished", "acabei", "feito", "terminei"

## Steps
1. If no argument given, ask: "What did you complete?"
2. Move completed tasks to "✅ Done" in `project-tasks.md` (at the project root)
3. Update any task states that changed during the session
4. Ask: "What's the next step for when you come back?"
5. Append to `.claude/CONTEXT.md`:
   ```
   ### Session [date]
   **Done:** [list]
   **Decisions:** [if any]
   **Next step:** [concrete action]
   ```
6. Confirm:
   ```
   ✅ Saved.
   Next session starts with: [next step]
   ```
