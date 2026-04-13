# Algorama Project Manager

Ship fast. One agent for solo founders using [Claude Code](https://docs.claude.ai/en/docs/claude-code/overview).

No PM process. No PO ceremonies. Just describe what you want to build — the agent handles the rest.

---

## Install

```bash
/plugin marketplace add algorama/algorama-project-manager
/plugin install algorama-project-manager@algorama
```

---

## Setup (one time per project)

Create `.claude/CLAUDE.md` with your project context — stack, what you're building, what you're NOT building, and your definition of done. See the [template below](#claudemd-template).

Everything else (`ship-log.md`, `.claude/CONTEXT.md`) is created automatically on first use.

---

## How it works

Five commands. One file.

```
your-project/
├── ship-log.md          ← all features and tasks (auto-created)
├── specs/               ← optional spec files for complex features
└── .claude/
    ├── CLAUDE.md        ← your project context (fill once)
    └── CONTEXT.md       ← session history (auto-maintained)
```

The agent reads `ship-log.md` at the start of every session. Tasks live inline under their feature — no cross-file linking, no IDs to track.

---

## The 5 commands

| Command | What it does |
|---|---|
| `/ship [idea]` | Idea → tasks → code. The whole flow in one command. |
| `/next` | One thing to do right now. No list, no alternatives. |
| `/done` | Close the session, save context for next time. |
| `/status` | Quick snapshot — what's building, what shipped, what's waiting. |
| `/ideas [idea]` | Capture something without committing to build it. |

That's the whole system.

---

## The fast path

Most of the time, you just type what you want:

```
/ship add user authentication
/ship fix the checkout redirect on mobile
/ship I want users to be able to export their data as CSV
```

The agent reads your `CLAUDE.md`, asks at most 2 questions, shows you a plan with a "ship when" condition and a task list, then starts coding immediately.

---

## When to use each command

**`/ship`** — start here. Any new feature, fix, or improvement. The agent validates it against your CLAUDE.md (scope, constraints, what you're NOT building), breaks it into tasks, and implements them one by one.

Add `--spec` for complex features where you want to think before building:
```
/ship add payment integration --spec
```
This generates `specs/FEAT-NNN-slug.md` with behaviors, edge cases, and states — you review it, then the agent builds from it.

**`/next`** — when you sit down and don't know where to start. Returns exactly one task from whatever's in progress.

**`/done`** — at the end of a session. Checks off what you completed, moves finished features to Done, and saves a note for next time.

**`/status`** — quick check on where everything stands. Under 20 lines, always.

**`/ideas`** — for things you don't want to forget but aren't ready to build. No format, no IDs, just a list.

```
/ideas dark mode
/ideas team accounts — wait until 10 paying users
/ideas              ← list everything and optionally promote one to /ship
```

---

## ship-log.md format

The agent maintains this file. You can edit it manually too — it's just markdown.

```markdown
# ship-log.md

## Building

### FEAT-001: user authentication [building]
> ship when: user can sign up and log in with email and password
> spec: specs/FEAT-001-user-auth.md  ← optional

- [x] set up Supabase auth
- [x] add sign-up form
- [ ] add login form
- [ ] redirect to dashboard after login

---

## Done

### FEAT-000: landing page [done] shipped:2026-04-01
> shipped: static page with waitlist form, deployed to Vercel

---

## Ideas

- dark mode
- CSV export
- team accounts — wait until 10+ users

---

## Killed

### FEAT-NNN: enterprise SSO [killed] 2026-04-10
> why: no enterprise customers yet, premature
```

**Features** are `FEAT-NNN`, assigned sequentially. **Tasks** are plain checkboxes — no IDs, no cross-file references.

---

## Migrating from v1 (pm/po agents)

If you were using the old PM and PO agents, the founder agent migrates automatically on first run.

When it detects `product-backlog.md` or `project-tasks.md` without a `ship-log.md`, it:
1. Reads both files
2. Builds `ship-log.md` — stories become features, tasks move inline
3. Tells you what it migrated
4. Leaves the old files as backup — you delete them when satisfied

---

## CLAUDE.md template

Copy this to `.claude/CLAUDE.md` in your project. Fill it in once. The agent reads it every session.

```markdown
# Project context

## Project
- **Name**: My App
- **Goal**: [what you're building and why — one sentence]
- **Status**: [where things stand — e.g. "early beta, 3 paying users"]

## Stack
- [frontend framework]
- [backend / database]
- [key third-party services]

## Who this is for
[Be specific. Not "users" — describe the person: their job, their pain, their context.]
Example: "Freelance designers who invoice 5–20 clients/month and lose time chasing payments"

## Core value
[The one thing this does better than the alternative — one sentence.]

## What success looks like
[A number, a behavior, a milestone.]
Example: "Users send their first invoice within 10 minutes of signing up"

## What we are NOT building
[Hard scope limits. The agent treats this as a block signal.]
- [thing out of scope, and why]
- [another out-of-scope thing]

## This month's goal
[What needs to be live by end of month? The agent biases /next and /ship toward this.]
Example: "Get first paying customer through onboarding without hand-holding"

## Definition of done
[What does "shipped" mean for this project?]
Example: "Deployed to production and smoke-tested on real data"

## Constraints
[What limits you? The agent uses this to avoid unrealistic suggestions.]
- [e.g. "Solo — one thing in progress at a time"]
- [e.g. "No paid APIs until first revenue"]
- [e.g. "Evenings only — 2 hours/day max"]

## Key decisions already made
[Closed debates. The agent won't re-open these.]
- [e.g. "Auth via Supabase — not building our own"]
- [e.g. "Mobile-first — desktop secondary"]


<!-- ─────────────────────────────────────────
     AUTOMATIC TASK TRACKING — do not remove
     ───────────────────────────────────────── -->

## Task tracking
After completing any unit of work, check `ship-log.md` for a matching task and close it.

Matching rules (apply in order):
1. **Keyword scan** — extract key nouns/verbs from what you built and match against task descriptions. Partial matches count.
2. **In-progress bias** — if a task is partially done, prefer it over untouched tasks.
3. **No match** — prompt: "I don't see a matching task — should I add one?"

To close: change `- [ ]` to `- [x]`.
```

The `## Task tracking` section enables Claude Code to close tasks automatically as it completes work. Keep it.

---

## License

MIT
