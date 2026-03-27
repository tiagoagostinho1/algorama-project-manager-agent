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

**2.** Create `.claude/CONTEXT.md` (can be empty).

**3.** That's it. `project-tasks.md` is created automatically at your project root on first use.

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
    └── CONTEXT.md             ← session history (last 10 sessions)
```

`CLAUDE.md` is read automatically by Claude Code at the start of every session. `CONTEXT.md` is appended by the agent at the end of each session and trimmed to the last 10 entries.

---

## CLAUDE.md template

Copy this to `.claude/CLAUDE.md` in your project and fill it in:

```markdown
# Project context

## Project
- **Name**: My App
- **Goal**: [what you're building and why — one sentence]
- **Status**: [where things stand right now]

## Product
- **Who is this for**: [be specific — not "users", but "freelance designers who invoice clients"]
- **Core value**: [the one thing this product does better than anything else]
- **Success looks like**: [how you know the product is working — a metric, a behavior, a feeling]
- **What we are NOT building**: [explicit scope boundaries — helps the PO say no faster]

## Team
- [Name] — [role]
- [Name] — [role]

## Stack
- [frontend framework]
- [backend / database]
- [other key tools]

## Current priorities
1. [top priority]
2. [second priority]
3. [third priority]

## Important context
- [anything the agent should always know — constraints, decisions, deadlines]

## Task tracking
When you complete a feature, fix, or any unit of work, check `project-tasks.md`
for a matching task and mark it done: change `[ ]` or `[~]` to `[x]` and move
the line to the `## ✅ Done` section. Match by task name or description — if
unsure, leave it and let the user confirm.
```

This `## Task tracking` block is what enables automatic task closure — Claude Code will check `project-tasks.md` and close matching tasks as it completes work, without you needing to do it manually.

---

## License

MIT
