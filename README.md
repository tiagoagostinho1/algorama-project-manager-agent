# Algorama Project Manager

Lightweight project manager for [Claude Code](https://docs.claude.ai/en/docs/claude-code/overview).

Keeps full context across sessions using local markdown files. No external tools, no integrations, no setup beyond filling in your project context.

---

## Install

```bash
# 1. Add the marketplace
/plugin marketplace add algorama/algorama-project-manager

# 2. Install the plugin
/plugin install algorama-project-manager@algorama
```

---

## Setup (one time per project)

**1.** Create `.claude/CLAUDE.md` with your project context (see [template below](#claudemd-template)).

**2.** That's it. All other files (`project-tasks.md`, `product-backlog.md`, `.claude/CONTEXT.md`) are created automatically on first use.

> Already have a `.claude/CLAUDE.md`? The agents will detect it and append the required `## Task tracking` section automatically — your existing content is never overwritten.

---

## Agents

Two agents, one for each layer of the work:

| Agent | Role | File |
|---|---|---|
| **pm** | Execution — tracks tasks, sessions, and what's in progress | `agents/pm.md` |
| **po** | Product — decides what to build, in what order, and why | `agents/po.md` |

Use the PM agent for daily work. Use the PO agent when you need to think about the product — what to build next, whether an idea is worth it, or how to sequence the roadmap.

---

## PM commands

| Command | What it does |
|---|---|
| `/pm:briefing` | Current state — tasks with story context, product pulse |
| `/pm:next` | One thing to do right now — hands off to PO if backlog is empty |
| `/pm:done` | Close session, update tasks, save context |
| `/pm:task [text]` | Add a task from natural language |
| `/pm:implement [ID]` | Look up a task — gives Claude Code everything needed to start coding |
| `/pm:sync` | Reconcile last coding session against open tasks — propose and apply updates |
| `/pm:unblock [ID]` | Resolve a blocked task — decision, workaround, or spike |
| `/pm:review` | Weekly review — done, slipped, stale, overdue |
| `/pm:standup` | Weekly report ready to share |
| `/pm:help` | List all PM commands |

## PO commands

| Command | What it does |
|---|---|
| `/po:brief` | Current product state — Now with task progress, Next, unresolved |
| `/po:story [text]` | Add a feature idea or user story |
| `/po:validate [ID]` | Challenge a story before building — problem, evidence, urgency |
| `/po:refine [ID]` | Sharpen a story and add acceptance criteria |
| `/po:breakdown [ID]` | Break a refined story into tasks in project-tasks.md |
| `/po:prioritize` | Re-order backlog by value vs. effort |
| `/po:ship [ID]` | Mark a story as shipped, close tasks, record the milestone |
| `/po:help` | List all PO commands |

---

## Task format

```
- [ ] #001 fix login bug → João | JWT token expires silently  due:2026-04-01
- [~] #002 write tests → Alice
- [x] #003 deploy to staging
- [!] #004 update API docs — BLOCKED: waiting on spec from design
```

| Field | Required | Description |
|---|---|---|
| `[ ]` / `[~]` / `[x]` / `[!]` | Yes | Status: to-do / in progress / done / blocked |
| `#001` | Yes | Auto-assigned ID — never changes |
| task name | Yes | Short name |
| `→ person` | No | Assignee |
| `\| description` | No | What actually needs to be done (max 15 words) |
| `due:YYYY-MM-DD` | No | Due date — used by `/pm:next` and `/pm:review` |

Tasks are added via `/pm:task` or edited manually — both work.

---

## How it works

```
your-project/
├── project-tasks.md           ← PM: execution tasks (auto-created if missing)
├── product-backlog.md         ← PO: stories and product decisions (auto-created if missing)
└── .claude/
    ├── CLAUDE.md              ← your project context (fill once)
    └── CONTEXT.md             ← session history (auto-created on first use)
```

`CLAUDE.md` is read automatically by Claude Code at the start of every session. `CONTEXT.md` is appended by the agent at the end of each session and trimmed to the last 10 entries.

---

## CLAUDE.md template

Copy this to `.claude/CLAUDE.md` in your project and fill it in once. Both the PM and PO agents read this file — each section is labeled so you know which agent uses it and why.

```markdown
# Project context

<!-- ═══════════════════════════════════════════
     SHARED — read by both PM and PO agents
     Fill this in first. It grounds everything.
     ═══════════════════════════════════════════ -->

## Project
- **Name**: My App
- **Goal**: [what you're building and why — one sentence]
- **Status**: [where things stand right now — e.g. "early beta, 3 paying users"]

## Team
- [Name] — [role]
- [Name] — [role]
<!-- Solo? Just put your own name. -->

## Stack
- [frontend framework]
- [backend / database]
- [key third-party services]


<!-- ═══════════════════════════════════════════
     FOR YOUR PO — product direction
     The PO agent uses this to validate stories,
     prioritize the backlog, and say no to the
     wrong things. Be honest and specific here.
     ═══════════════════════════════════════════ -->

## Who this is for
[Be specific. Not "users" — describe the person: their job, their pain, their context.]
Example: "Freelance designers who invoice 5–20 clients/month and lose time chasing payments"

## Core value
[The one thing this product does better than anything else — one sentence.]
Example: "Turns a 20-minute invoicing process into 2 minutes"

## What success looks like
[How you know it's working — a number, a behavior, a feeling.]
Example: "Users send their first invoice within 10 minutes of signing up"

## What we are NOT building
[Hard scope boundaries. The PO uses this as a kill signal in /po:validate.]
- [thing you will not build, and why]
- [another thing out of scope]

## Release rhythm
[How often do you ship? The PO uses this to size stories and set urgency.]
Example: "Continuous — ship as soon as a story is done"
Example: "Weekly — every Friday"
Example: "Monthly — end of month release"

## This cycle's goal
[What are you trying to have live by the end of this month/sprint?]
Example: "Get the first paying customer through onboarding without hand-holding"
<!-- Update this every cycle. It's the PO's north star for prioritization. -->


<!-- ═══════════════════════════════════════════
     FOR YOUR PM — execution context
     The PM agent uses this to run sessions,
     assess urgency, and know what "done" means
     for this project specifically.
     ═══════════════════════════════════════════ -->

## Definition of done
[What does "shipped" mean here? The PM uses this to close tasks correctly.]
Example: "Deployed to production and smoke-tested"
Example: "Merged to main, deployed, and user-facing copy updated"

## Constraints
[What limits what you can do? The PM uses this to avoid suggesting unrealistic next steps.]
- [e.g. "Solo developer — max 1 thing in progress at a time"]
- [e.g. "No budget for paid APIs until first revenue"]
- [e.g. "Can only work evenings — 2–3 hours/day"]

## Key decisions already made
[Decisions that are final and should not be re-opened. Saves the PM from suggesting things that were already ruled out.]
- [e.g. "We use Supabase for auth — not building our own"]
- [e.g. "Mobile-first — desktop is secondary"]


<!-- ═══════════════════════════════════════════
     AUTOMATIC TASK TRACKING
     Do not remove this section.
     It enables Claude Code to close tasks
     automatically as it completes work.
     ═══════════════════════════════════════════ -->

## Task tracking
After completing any unit of work, check `project-tasks.md` for a matching task and close it.

Matching rules (apply in order):
1. **Exact ID** — if the work references `#NNN`, match that task directly.
2. **Keyword scan** — extract key nouns/verbs from what you built (e.g. "login", "auth", "fix") and match against task names and descriptions. Partial matches count.
3. **In-progress bias** — if a `[~]` task is plausibly related, prefer it over a `[ ]` task.
4. **Multiple matches** — list them and ask: "Which task should I close? (or all?)"
5. **No match** — prompt: "I don't see a matching task — should I add one?"

To close: change `[ ]` or `[~]` to `[x]` and move the line to `## ✅ Done`.
```

The `## Task tracking` section enables automatic task closure — Claude Code checks `project-tasks.md` as it completes work without you needing to do it manually. Do not remove it.

---

## License

MIT
