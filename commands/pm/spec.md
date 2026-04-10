---
description: Generate a spec document for a feature before building it — behaviors, edge cases, states, and AC in one place.
argument-hint: [story ID or feature description, e.g. #P003 or "add user auth"]
---

# /pm:spec

Generate a structured spec file for a story or feature. The spec captures every behavior, edge case, and constraint before any code is written. Claude Code reads this file during implementation instead of guessing intent.

## Also triggered by
Natural language to the PM agent: "write a spec for [feature]", "spec out [feature]", "I want to spec [thing] before building", "create a spec for #PNNN", "spec first then build [thing]"

## If no argument given
Ask: "What do you want to spec? Give me a story ID (e.g. #P003) or a short description."

---

## Steps

### 1. Determine the story

**If a story ID is given (e.g. #P003):**
- Read `product-backlog.md` and find that story + its acceptance criteria
- Use the story text and existing AC as the starting point

**If a description is given (no story ID):**
- Read `.claude/CLAUDE.md` and `product-backlog.md` first
- Check if a matching story already exists (keyword match)
  - If found: use it, note which story ID
  - If not found: create the story now (see Story creation below) and assign the next `#PNNN`

**Story creation (when no existing story):**
- Write story using the real user from CLAUDE.md: `As a [specific user], I want [action] so that [outcome]`
- Add to `## 🟢 Later` in `product-backlog.md` with status `[ ]`
- Confirm with user: "Created #PNNN — [story text]. Generating spec now."
- **MCP sync check** (only when creating a new story): scan for tools starting with `mcp__clickup`, `mcp__jira`, `mcp__linear`, `mcp__asana`, `mcp__trello`, `mcp__notion`, `mcp__todoist`, `mcp__monday`, `mcp__basecamp`, `mcp__github`, `mcp__gitlab`. If found: ask "Also create #PNNN in [Tool]? (y/n)" and sync if yes.

### 2. Check for existing spec

Look for `specs/PNNN-*.md` matching the story ID.

- **Found:** Show "Spec already exists at specs/PNNN-slug.md. Update it, review it, or start fresh? (update/review/new)"
  - `update` → read existing spec, ask what changed, update in place
  - `review` → display the spec, ask for edits, then stop
  - `new` → overwrite with a freshly generated spec
- **Not found:** Proceed to generation

### 3. Generate the spec

Read `.claude/CLAUDE.md` for: Stack, Who this is for, Definition of done, Key decisions already made, What we are NOT building.

Generate the spec content (see format below). Fill every section — do not leave sections empty. If you don't have enough information for a section, infer from context and mark inferred items with `(inferred)` so the user knows to review.

### 4. Write the spec file

- Create `specs/` directory if it doesn't exist
- Filename: `specs/PNNN-[slug].md` where slug is the story name lowercased, spaces replaced with hyphens, max 5 words
  - Example: `specs/P003-password-reset-flow.md`
- Write the spec

### 5. Link spec to story

Update the story line in `product-backlog.md` to add `spec:specs/PNNN-slug.md` at the end:
```
- [ ] #P003 As a returning user... → spec:specs/P003-password-reset-flow.md
```

### 6. Show and ask

Display the spec in full. Then ask:
```
Spec written to specs/PNNN-slug.md

Review it and edit as needed — especially the edge cases and open questions.
When ready: start building from this spec? (y/n)
```

- If yes: run `/pm:feature #PNNN` — the spec will be read automatically
- If no: stop. User edits the spec manually, then runs `/pm:feature` or `/pm:implement` when ready.

---

## Spec format

```markdown
# Spec: [feature name]
> Story: #PNNN — [story text]
> Spec created: [date]
> Status: draft | ready | in-progress | shipped

## Problem
[1–2 sentences: what user pain this solves, why it matters now]

## Users affected
[Who experiences this — be specific, use the real user from CLAUDE.md]

## Behaviors

### Happy path
- [Step 1: what the user does]
- [Step 2: what happens in response]
- [Step 3: what the user sees/gets]
(be exhaustive — list every interaction, not just the happy summary)

### Edge cases
- **Empty input:** [what happens if the user submits nothing]
- **Invalid input:** [what happens if the value is wrong format/type]
- **Duplicate:** [what happens if the item already exists]
- **Concurrent:** [what happens if two users do the same thing at once — skip if not relevant]
- **Dependency failure:** [what happens if an external service / DB call fails]
- **Permission:** [what happens if the user doesn't have access — skip if not relevant]
- **Mobile:** [any differences on small screens — skip if not relevant]

### States
- **Empty state:** [what the UI/data looks like before any interaction]
- **Loading state:** [what the user sees while waiting]
- **Error state:** [what the user sees when something fails — include the error message text]
- **Success state:** [what the user sees when it works]

### Out of scope
- [Thing this spec explicitly does NOT cover]
- [Another thing — prevents scope creep during implementation]

## Acceptance criteria
- [ ] [Verifiable: "user sees X when Y", not "it works"]
- [ ] [Verifiable]
- [ ] [Verifiable]
(2–4 items max — these become the tasks)

## Technical notes
[Only what's non-obvious from the stack in CLAUDE.md]
- **Data shape:** [key fields and types if non-trivial]
- **API contract:** [endpoint, method, request/response shape if new]
- **Performance:** [any latency or size constraints]
- **Security:** [auth, validation, rate limiting concerns]
(omit sections that don't apply)

## Open questions
- [ ] [Something unresolved that implementation may surface — needs a decision]
(leave empty if none — don't force it)
```

---

## Rules

- Never leave a section skeleton with no content — fill it or mark it `(inferred)` so the user knows to verify
- Edge cases section must always have at least: empty input, invalid input, dependency failure
- States section must always have: loading, error, success
- Out of scope must have at least 1 item — even for small features
- Open questions should be real unknowns, not rhetorical — if you know the answer, answer it in Technical notes instead
- If CLAUDE.md is missing: proceed with what you know, add a note at the top of the spec: `> Note: No CLAUDE.md found — stack and user details may need review`
