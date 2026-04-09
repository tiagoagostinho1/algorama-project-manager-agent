---
description: Reconcile last coding session against open tasks — propose and apply state changes.
argument-hint: [optional: session date, e.g. 2026-04-09]
---

# /pm:sync

Use this after a coding session to catch task updates the main agent may have missed.

## Also triggered by
"sync tasks", "update tasks from session", "reconcile tasks"

## Steps

1. Read `project-tasks.md` — collect all `[ ]` (to-do) and `[~]` (in-progress) tasks. These are the only candidates.
2. Read `.claude/CONTEXT.md`:
   - If no argument given: use the most recent `### Session [date]` or `### PO Session [date]` block.
   - If a date argument given (e.g. `/pm:sync 2026-04-08`): find `### Session 2026-04-08` specifically.
   - If no CONTEXT.md or no session entry found: say "No session found in .claude/CONTEXT.md. Run /pm:done first to record what you completed." and stop.
   - If the **Done:** list in the session is empty: say "Last session has no completed work recorded. Nothing to sync." and stop.
3. For each item in **Done:** and **Next step:** from the session, attempt to match against open tasks:
   - **Exact ID match** — if the session text references `#NNN`, match that task directly (high confidence)
   - **Keyword match** — extract key nouns/verbs from the session phrase; match against task name and description; partial overlap counts (medium confidence)
   - **In-progress bias** — when confidence is equal, prefer `[~]` tasks over `[ ]` tasks
4. Classify each session item into one of:
   - **Close** — high-confidence match → mark `[x]`
   - **Start** — mentioned as in-progress but task is still `[ ]` → mark `[~]`
   - **Uncertain** — plausible match but not clear → show both and ask user
   - **No match** — nothing found → prompt to add a task
5. Present the full proposal (see Output format). **Do not write anything yet** — wait for user confirmation.
6. On confirmation: apply all approved changes to `project-tasks.md`. Update checkboxes and move tasks to the correct sections.
7. Confirm:
   ```
   ✅ Synced. [N] task(s) updated.
   ```

## Output format

```
## /pm:sync — [date]

### Ready to close
- [x] #003 fix login redirect  ← matched "fixed the login redirect issue" in session

### Ready to start
- [~] #007 write migration script  ← mentioned as started in session

### Needs your call
- Session said: "finished the auth thing"
  Candidates: #002 auth middleware | #005 auth token refresh
  → Which matches? (2, 5, both, skip)

### No match found
- "cleaned up the config files" — no open task found. Add one? (y/n)
```

Omit any section with no entries. If all sections are empty, say "Everything already looks synced."

## Edge cases

- If `project-tasks.md` has no open tasks: say "All tasks are already done or the file is empty."
- If CONTEXT.md session format is unexpected (no **Done:** field): say "Couldn't parse the last session — paste what you completed and I'll match manually."
- Tasks already marked `[x]` are never re-processed — closed tasks are not candidates.
