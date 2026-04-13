---
description: Ship a feature end-to-end — from idea to running code.
argument-hint: [feature description, e.g. "add CSV export" or "fix login redirect"]
---

# /ship

Turn an idea into running code. Asks at most 2 questions, then ships.

## Also triggered by
Natural language to the founder agent: "I want to build X", "add X", "fix X", "let's ship X", "implement X", "we need X" — when no specific task is already in progress.

## If no description given
Ask once: "What do you want to build or fix?" Then continue.

---

## Phase 1 — Read context (silent, no output)

1. Read `.claude/CLAUDE.md` — Stack, This month's goal, What we are NOT building, Definition of done, Constraints, Key decisions already made
2. Read `ship-log.md` — find the highest existing `FEAT-NNN` to assign the next ID; check if a similar feature already exists in Building or Ideas
3. If `ship-log.md` doesn't exist, create it with the standard template before continuing
4. If a matching feature has a `spec:` link, read that spec file — **when a spec exists, skip Phase 2 (clarify) entirely**

---

## Phase 2 — Gut check + scope (0–2 questions total, often 0)

### Out of scope check
If the description matches anything in "What we are NOT building" in CLAUDE.md:
> "CLAUDE.md says we're not building [X]. Proceed anyway? (y/n)"

If the user says no: stop. If yes: continue.

### Gut check (skip if any of these are true)
- It's a bug fix
- It's already in `## Ideas` in ship-log.md
- It clearly matches "This month's goal" in CLAUDE.md

Otherwise, ask once:
> "What breaks or stays annoying if you skip this for 2 months?"

If the answer is "nothing much" — suggest deferring to Ideas. If the answer shows real urgency — proceed.

### Scope question (skip if any of these are true)
- Description is already scoped ("add CSV export to the dashboard")
- It's a bug fix
- CLAUDE.md "This month's goal" resolves the ambiguity

Only ask if the description has 2+ genuinely different interpretations. Ask as multiple choice, never open-ended:
- "Export to CSV, PDF, or both?"
- "Email+password, magic link, or OAuth?"

**Maximum 2 questions total across gut check + scope. Count them. Stop at 2.**

---

## Phase 3 — Show plan, confirm once

State what you inferred, then show the plan:

```
## Shipping: [feature name]

Ship when: [one plain-English sentence — what does done look like for a user?]
Stack: [relevant parts from CLAUDE.md]

Tasks:
- [ ] [task 1 — verb + object, max 10 words]
- [ ] [task 2]
- [ ] [task 3]

[Spec: will generate specs/FEAT-NNN-slug.md first]  ← only show if --spec flag or user asked for spec

Start? (y/n)
```

**Task generation rules:**
- One task per distinct piece of work (endpoint, UI element, migration, test)
- Each task completable in one coding session
- If a task needs "and" more than once, split it
- Format: verb + object ("add /api/export endpoint", not "export stuff")
- Include setup tasks (migrations, env vars) and one integration check if the feature touches multiple layers
- Bug fix fallback (no clear tasks): "Reproduce [bug]" → "Fix [root cause]" → "Verify fix"

**"Ship when" rule:**
One sentence, plain English, from the user's perspective. Not "the endpoint returns 200". Example: "User can download all their records as a CSV file from the dashboard."

Apply corrections once if requested, then proceed. Never loop.

---

## Phase 4 — Generate spec (only if --spec flag or user asked for spec)

Create `specs/FEAT-NNN-[slug].md` using this format:

```markdown
# Spec: [feature name]
> Feature: FEAT-NNN
> Created: [date]
> Status: draft

## Problem
[1–2 sentences: what user pain this solves, why it matters now]

## Users affected
[Who experiences this — be specific, use the real user from CLAUDE.md]

## Behaviors

### Happy path
- [Step 1: what the user does]
- [Step 2: what happens in response]
- [Step 3: what the user sees/gets]

### Edge cases
- **Empty input:** [what happens]
- **Invalid input:** [what happens]
- **Dependency failure:** [what happens if external service or DB call fails]
- **Permission:** [what happens if user doesn't have access — skip if not relevant]

### States
- **Loading state:** [what the user sees while waiting]
- **Error state:** [what the user sees when something fails]
- **Success state:** [what the user sees when it works]

### Out of scope
- [Explicit exclusion to prevent scope creep]

## Acceptance criteria
- [ ] [Verifiable: "user sees X when Y"]
- [ ] [Verifiable]

## Technical notes
[Only what's non-obvious from the stack in CLAUDE.md]

## Open questions
- [ ] [Real unknown — leave empty if none]
```

Show the spec, ask: "Review it, edit as needed. Start building from this spec? (y/n)"

---

## Phase 5 — Write to ship-log.md

Add the new feature under `## Building`:

```markdown
### FEAT-NNN: [feature name] [building]
> ship when: [the ship-when sentence]
> spec: specs/FEAT-NNN-slug.md  ← omit if no spec

- [ ] [task 1]
- [ ] [task 2]
- [ ] [task 3]

---
```

If `ship-log.md` doesn't exist, create it first:
```markdown
# ship-log.md

<!--
  Single source of truth for features and tasks.
  Updated by the founder agent. You can edit manually.

  Feature IDs: FEAT-NNN, sequential, never reused.
  Status: [building] | [done] | [killed]
-->

## Building


## Done


## Ideas


## Killed
```

**MCP sync (optional, silent check):**
Scan for tools starting with: `mcp__clickup`, `mcp__jira`, `mcp__linear`, `mcp__asana`, `mcp__trello`, `mcp__notion`, `mcp__todoist`, `mcp__monday`, `mcp__basecamp`, `mcp__github`, `mcp__gitlab`. If found: ask "Also create FEAT-NNN in [Tool]? (y/n)". On failure: report error, do NOT roll back local files.

---

## Phase 6 — Implement tasks sequentially

For each task, output a brief then code immediately:

```
## [N/total] — [task name]

Feature: FEAT-NNN — [feature name]
Ship when: [ship-when line]
What to build: [2–3 sentences of specific implementation guidance using stack from CLAUDE.md]
Done when: [the specific outcome this task achieves]
[Spec: specs/FEAT-NNN-slug.md]  ← omit if no spec
[Key decisions: ...]  ← omit if none relevant

---
```

Start coding immediately after the brief. Do not wait for "go".

**After each task completes:**
- Change `- [ ]` to `- [x]` in ship-log.md
- If more tasks remain: output next task brief and continue
- If last task: go to Phase 7

**Interruptions:**
User says "stop", "pause", "hold", or asks a question → pause, handle it, then ask "Continue with [task]?" before resuming.

---

## Phase 7 — Close the feature

When all tasks are `[x]`:

1. Move the feature entry from `## Building` to `## Done` in ship-log.md:
   ```
   ### FEAT-NNN: [feature name] [done] shipped:[date]
   > shipped: [what was actually built — one sentence]
   ```
2. Append to `.claude/CONTEXT.md` (keep last 5 entries only):
   ```
   ### [date]
   Done: FEAT-NNN [feature name]
   Note: [any inferred choices made, e.g. "chose email+password over OAuth"]
   Next: [next open feature in Building, or "run /status"]
   ```
3. Output:
   ```
   ## Done: [feature name]

   Shipped as: [what was built — one sentence]
   Next: [first unchecked feature in Building, or "run /status to see what's next"]
   ```

---

## Edge cases

**Feature already in Building:**
> "FEAT-NNN [feature name] is already in progress with [N] open tasks. Jump to implementing those? (y/n)"

**Feature already in Ideas:**
> "Found '[idea text]' in your Ideas. Ship this one? (y/n)"
If yes: remove from Ideas, treat as new /ship with that description.

**Partial completion (user stopped midway):**
Leave open tasks as `[ ]`, feature stays in `## Building`. Append partial session to CONTEXT.md. Next time /ship is called with same description: detect existing feature and offer to resume.

**CLAUDE.md missing:**
Ask one extra question: "What stack is this project using?" Infer everything else. Note: "No CLAUDE.md found — create one for better context next time."
