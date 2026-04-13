---
description: Close the session, save context, and note what's next.
argument-hint: [what you completed — optional]
---

# /done

Close the session and update ship-log.md.

## Also triggered by
"done", "finished", "acabei", "feito", "terminei"

---

## Steps

1. **If no argument given:** ask once — "What did you complete?"

2. **Update ship-log.md:**
   - Check off completed tasks (`- [ ]` → `- [x]`)
   - If ALL tasks for a feature are now `[x]`: ask once "FEAT-NNN [name] looks complete — move it to Done? (y/n)"
     - If yes (or if it's obvious): move to `## Done`, add `shipped:[date]`, add `> shipped: [one sentence]`

3. **Optional blocker question:** ask "Anything blocking you or worth noting before next session?" — skip entirely if the user just says "done" with no context suggesting a blocker

4. **Append to `.claude/CONTEXT.md`** (keep last 5 entries — remove oldest if more than 5):
   ```
   ### [date]
   Done: [what was completed]
   Note: [decision or blocker — omit line if nothing to note]
   Next: [concrete next step]
   ```

5. **Confirm:**
   ```
   Saved. Next: [next step]
   ```

---

## Rules
- Maximum 2 questions (what completed + blocker)
- If the user's message already says what they completed, skip question 1
- Do not ask about next steps if the answer is obvious from ship-log.md (next open task in the same feature)
