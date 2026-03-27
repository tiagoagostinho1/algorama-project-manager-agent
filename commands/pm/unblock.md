---
description: Resolve a blocked task — identify what's needed, who acts, and what happens next.
argument-hint: [task ID or description]
---

# /pm:unblock

Blocked tasks don't unblock themselves. This command forces a decision.

## Steps
1. If no argument given, list all tasks in `## ⚠️ Blocked` from `project-tasks.md` and ask: "Which task to unblock?"
2. Read the blocked task and its reason (the text after `— BLOCKED:`).
3. Classify the block into one of three types and surface it:

   **Type A — Needs a decision**
   "This is blocked because a choice hasn't been made. Who makes this decision, and what are the options?"
   - Ask: "What are the options?" then "Which do you choose?"
   - Once decided: record the decision in `.claude/CONTEXT.md` under "Project decisions", unblock the task

   **Type B — Waiting on someone/something external**
   "This is blocked on [person/thing]. Is there a workaround, or does this have to wait?"
   - If workaround exists: rewrite the task to use the workaround, unblock it
   - If it must wait: ask "Should this move to the PO backlog as ❓ Under Discussion?" — if yes, add a corresponding story entry and flag the link
   - Set or update the `due:` date if there's a known resolution date

   **Type C — Technical uncertainty**
   "This is blocked because we don't know how to do it yet. What would it take to find out?"
   - Reframe as a spike task: "investigate [X]" — a time-boxed task to resolve the uncertainty
   - Replace the blocked task with the spike, keeping the original as a follow-up

4. Once the block type is handled, update `project-tasks.md`:
   - Change `[!]` to `[~]` (in progress) or `[ ]` (back to backlog) depending on whether work can resume now
   - Remove the `— BLOCKED: reason` text
   - Move to the correct section

5. Reply:
   ```
   ✅ #004 unblocked.
   Action taken: [what was decided or changed]
   Next step: [what happens now with this task]
   ```

## Rules
- Never unblock a task by just removing the blocked status without resolving the root cause. That's hiding the problem.
- If the block can't be resolved right now, it's still useful to classify it and set a follow-up date.
- If unblocking requires a product decision (e.g. "should we even do this?"), suggest `/po:validate` before proceeding.
