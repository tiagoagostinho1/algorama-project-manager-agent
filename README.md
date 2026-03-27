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

After installing, create `.claude/CLAUDE.md` in your project with your context:

```markdown
# Project context

- **Project**: My App
- **Team**: Alice (frontend), Bob (backend)
- **Stack**: Next.js + Postgres
- **Current goal**: Ship MVP by end of month
```

Also create `.claude/CONTEXT.md` (can be empty). The agent will create `project-tasks.md` at your project root automatically on first use.

---

## Commands

| Command | What it does |
|---|---|
| `/pm:briefing` | Current state — blocked, in progress, up next |
| `/pm:next` | One thing to do right now |
| `/pm:done` | Close session, update tasks, save context |
| `/pm:task [text]` | Add a task from natural language |
| `/pm:standup` | Weekly report ready to share |
| `/pm:help` | List all commands |

---

## How it works

```
your-project/
├── project-tasks.md           ← your task list (agent reads/writes this; auto-created if missing)
└── .claude/
    ├── CLAUDE.md              ← your project context (fill once, info only)
    └── CONTEXT.md             ← session history (agent updates this, keeps last 10 sessions)
```

`CLAUDE.md` is read automatically by Claude Code at the start of every session — it's purely informational context (who, what, stack, goal). Tasks live in `project-tasks.md` at the project root.

---

## License

MIT
