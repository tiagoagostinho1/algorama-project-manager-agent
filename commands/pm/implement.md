---
description: Look up a task and give Claude Code everything it needs to implement it.
argument-hint: [task ID, e.g. #003]
---

# /pm:implement

Read `project-tasks.md`, `product-backlog.md` (if it exists), and `.claude/CLAUDE.md`. Find the task by ID and produce a coding brief that Claude Code can act on immediately.

## If no ID given
List all tasks in Backlog and In Progress sections. Ask which one to implement.

## Steps

1. Find the task line in `project-tasks.md` by ID (e.g. `#003`).
   - If not found: say "Task #003 not found in project-tasks.md." and stop.
   - If already `[x]` (done): say "Task #003 is already marked done." and stop.

2. Scan `product-backlog.md` (if it exists) for a story that links to this task — look for `→ tasks #NNN` or `→ tasks #NNN–#NNN` on a story line where the requested task ID falls in range. If found, read that story's text and any acceptance criteria indented under it.

3. Read `.claude/CLAUDE.md` for: **Stack**, **Definition of done**, **Key decisions already made**.

4. Mark the task `[~]` in `project-tasks.md` and move it to `## 🔄 In Progress` if it isn't there already.

5. Output the coding brief (format below).

## Output format

```
## 🛠️ Implementing #NNN — [task name]

**What to build:**
[task description from | field; if missing, write 1–2 sentences inferred from the task name]

**Story context:** #PNNN — [story text]   ← omit entire line if no linked story found
**Acceptance criteria:**                   ← omit entire section if no story or no criteria
- [ ] [criterion 1]
- [ ] [criterion 2]

**Stack:** [from CLAUDE.md — omit if CLAUDE.md missing]
**Definition of done:** [from CLAUDE.md — omit if missing]
**Key decisions:** [only decisions relevant to this task — omit section if none apply or CLAUDE.md missing]

---
Start coding. When done, mark #NNN complete in project-tasks.md and run /pm:done.
```

## Natural language triggers
Also respond to: "implement #NNN", "build task #NNN", "start #NNN", "code #NNN", "work on #NNN", "build #NNN", "let's do #NNN"
