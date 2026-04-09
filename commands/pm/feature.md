---
description: End-to-end feature implementation — from plain description to shipped tasks.
argument-hint: [feature description, e.g. "add user authentication"]
---

# /pm:feature

Turn a vague description into a running feature — story, tasks, and code — with minimal back-and-forth.

## Also triggered by
Natural language to the PM agent: "add [feature]", "I want to build [thing]", "implement [capability]", "we need [feature]", "can you add [thing]", "let's build [feature]", "fix [thing that's broken]" — when no task ID (`#NNN`) is present in the message.

## If no description given
Ask once: "What do you want to build or fix?" Then continue.

---

## Phase 1 — Read context

Before asking anything, read:
1. `.claude/CLAUDE.md` — Stack, Who this is for, Core value, Definition of done, Constraints, Key decisions already made, What we are NOT building
2. `product-backlog.md` — check for existing matching story + highest `#PNNN`
3. `project-tasks.md` — highest existing `#NNN`

If files don't exist yet, continue — you'll create them when writing.

---

## Phase 2 — Clarify (maximum 2 questions, often 0)

Read the description + CLAUDE.md first. Only ask about genuine unknowns.

### Never ask about (always infer):
- **Who it's for** — use "Who this is for" from CLAUDE.md
- **Stack** — from CLAUDE.md; never ask what language or framework
- **Definition of done** — from CLAUDE.md; never ask what "shipped" means
- **Decisions already made** — if auth provider, DB, etc. is decided, use it

### When to ask:

**Question 1 — Scope** (only if description has 2+ plausible interpretations for a new capability)
Ask one concrete multiple-choice question, never open-ended:
- "Is this email+password, magic link, or OAuth (Google/GitHub)?"
- "Export to CSV, PDF, or both?"

Skip entirely if: bug fix, single clear interpretation, or CLAUDE.md resolves it.

**Question 2 — First slice** (only if description implies a large feature)
Ask: "What's the minimum version useful to you right now?"

Skip entirely if: description is already scoped, or "This cycle's goal" in CLAUDE.md resolves it.

### Decision rule:
- Bug fix → 0 questions
- Scoped new feature → 0 questions
- Ambiguous new capability → 1 question max
- Large ambiguous feature → 2 questions max

### Always state inferred choices:
> "Assuming email+password auth via Supabase (from CLAUDE.md). Stack: [stack]."

So the user can correct before work starts.

---

## Phase 3 — Derive story + acceptance criteria

Write the story using the real user type from CLAUDE.md:
```
#PNNN As a [specific user from CLAUDE.md], I want [concrete action] so that [measurable outcome]
```

Derive 2–4 acceptance criteria:
- Each must be independently verifiable ("user sees X when Y", not "it works")
- Order: most critical first
- Bug fixes: frame as "no longer occurs under [conditions]" + the specific failing conditions
- If CLAUDE.md has a project DoD, include it as the final criterion: "Meets project DoD: [DoD text]"

Show story + criteria, ask once:
```
Does this capture what you want? (y/n or corrections)
```

Incorporate corrections once and proceed. Never loop more than once.

---

## Phase 4 — Generate tasks

**Primary — one task per acceptance criterion:**
- Plus setup tasks implied by the story (DB migration, env var, config changes)
- Plus one integration/smoke test task if the feature touches multiple layers

**Fallback for bug with no clear AC:**
- Task 1: Reproduce and diagnose [bug name]
- Task 2: Fix [root cause]
- Task 3: Verify fix on [affected surface]

**Granularity rules:**
- Completable in one coding session
- If a task needs "and" more than once, split it
- Format: verb + object ("add password reset endpoint", not "password reset stuff")
- Assign to team member from CLAUDE.md if listed; else leave blank

**Per-task DoD:** project DoD + the specific AC item(s) that task covers. This will appear in each task's coding brief.

**IDs:** next available `#NNN` from `project-tasks.md`, sequential, zero-padded.

Show the full plan before writing anything:

```
## Feature plan: [feature name]

**Story:** #PNNN As a [user], I want [goal] so that [outcome]
**Acceptance criteria:**
- [ ] [criterion 1]
- [ ] [criterion 2]
- [ ] [criterion 3]

**Tasks (will implement in order):**
- [ ] #NNN [task name] | [what needs to be done — max 15 words]
- [ ] #NNN [task name] | [description]
- [ ] #NNN [task name] | [description]

**From CLAUDE.md:**
- Stack: [stack]
- DoD: [dod]
- Decisions applied: [any relevant decisions]

Start building? (y/n)
```

Apply corrections once if requested, then proceed. Never loop.

---

## Phase 5 — Write to files

On confirmation:

1. **`product-backlog.md`** — add story to `## 🔴 Now (current focus)` with status `[~]`, acceptance criteria indented below, and `→ tasks #NNN–#NNN` at end of story line
2. **`project-tasks.md`** — append all tasks to `## 📋 Backlog` with status `[ ]`
3. If either file doesn't exist, create it using the standard template before writing

Reply:
```
✅ #PNNN created — [N] tasks added (#NNN–#NNN).
Starting #NNN now.
```

---

## Phase 6 — Implement tasks sequentially

Implement each task in order without asking between tasks.

### For each task, output the coding brief then code immediately:

```
## 🛠️ [N/total] — #NNN [task name]

**Story:** #PNNN — [story text]

**What to build:**
[2–3 sentences of specific implementation guidance, using stack from CLAUDE.md]

**Done when:**
- [ ] [the specific AC item(s) this task covers]
- [ ] Meets project DoD: [DoD from CLAUDE.md]

**Stack:** [from CLAUDE.md]
**Key decisions:** [only decisions relevant to this task — omit section if none]

---
```

Start coding immediately after the brief. Do not wait for "go".

**After each task completes:**
- Mark `[x]` in `project-tasks.md`, move to `## ✅ Done`
- Check off the covered AC item in `product-backlog.md`
- If more tasks remain: output next task's brief and continue
- If last task: go to Phase 7

### Interruptions:
User says "stop", "pause", "hold", "wait", or asks a question → pause, handle it, then ask "Continue with #NNN?" before resuming.

---

## Phase 7 — Close the feature

When all tasks are `[x]`:

1. Mark story `[x]` in `product-backlog.md`, move to `## 📦 Shipped`, add `shipped:[date]`
2. Append to `.claude/CONTEXT.md` (keep last 10 entries):
   ```
   ### Session [date]
   **Done:** #PNNN [feature name] — tasks #NNN–#NNN
   **Decisions:** [inferred choices made, e.g. "chose email+password over OAuth"]
   **Next step:** [next story ID if one exists in Now/Next, else "/pm:briefing"]
   ```
3. Output:
   ```
   ## ✅ [Feature name] shipped

   Tasks: #NNN–#NNN ([N] total)
   Story: #PNNN closed

   [Any inferred choices made during implementation]

   Next: [concrete next step]
   ```

---

## Edge cases

**Story already exists in backlog:**
Keyword-match description against `product-backlog.md`. If found:
> "Found #PNNN — [story text]. Build from this one, or create a new story? (existing/new)"
If existing: skip Phase 3, use existing story + AC, go to Phase 4.

**Tasks already exist for the story:**
If `→ tasks #NNN–#NNN` is on the story and those tasks are open:
> "#PNNN already has tasks #NNN–#NNN. Jump to implementing those? (y/n)"

**CLAUDE.md missing:**
Ask one extra question: "What stack is this project using?" Infer everything else. Note: "No CLAUDE.md found — create one for better context next time."

**Feature is out of scope per CLAUDE.md:**
If it matches "What we are NOT building":
> "This looks like [out-of-scope thing] — CLAUDE.md says we're not building that. Proceed anyway? (y/n)"

**Description is a question, not a feature:**
"Should we add auth?" → don't treat as `/pm:feature`. Reply:
> "That sounds like a product question. Run `/po:validate` to decide whether it's worth building first."

**Partial completion — user stopped midway:**
Leave open tasks as `[ ]`, story stays `[~]` in Now. Append partial session to CONTEXT.md.
Next time `/pm:feature` is called with same description: detect existing story + open tasks and offer to resume.
