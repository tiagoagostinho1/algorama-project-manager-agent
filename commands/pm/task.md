---
description: Add a new task from plain text.
argument-hint: [task description]
---

# /pm:task

Add a task to `project-tasks.md` (at the project root) from natural language.

## Examples
- `/pm:task fix login bug → João`
- `/pm:task client meeting Friday`
- `/pm:task implement auth — high priority`
- `/pm:task deploy staging by April 1`

## Steps
1. Parse: name, assignee, description, section, and due date:
   - **Section rules:**
     - Contains "blocked" → `⚠️ Blocked`
     - Contains "urgent", "today", or "now" → `🔄 In Progress`
     - Everything else → `📋 Backlog`
   - **Due date rules:** Extract any date mention (e.g. "by Friday", "April 1", "due 2026-04-10") and convert to `due:YYYY-MM-DD`. Use today's date as reference for relative dates. If no date is mentioned, omit `due:`.
   - **ID:** Scan `project-tasks.md` for the highest existing `#NNN`. Next ID = max + 1, zero-padded to 3 digits. If no tasks exist yet, start at `#001`.
   - **Description:** If the user provides extra context beyond the task name, capture it as a short description (max 15 words) after `|`. If none given, ask: "Any description? (or press enter to skip)"
2. Confirm before adding:
   ```
   Adding to [section]:
   - [ ] #005 task name → person | description  due:2026-04-01
   Confirm? (y/n)
   ```
2b. **MCP sync check** (only after user confirms):
   - Scan available tools for names starting with any of these prefixes: `mcp__clickup`, `mcp__trello`, `mcp__jira`, `mcp__asana`, `mcp__linear`, `mcp__notion`, `mcp__todoist`, `mcp__monday`, `mcp__basecamp`, `mcp__github`, `mcp__gitlab`
   - If one or more are found: ask "Also create in [Tool Name]? (y/n)"
   - If none found: skip this step silently
   - Session memory: remember the user's answer for the rest of the session. On subsequent tasks pre-select it as the default — e.g. `[Y/n]` if they said yes, `[y/N]` if they said no. Always accept an explicit override.
3. **Always write to `project-tasks.md` first**, unconditionally, before any MCP call. Add to the correct section.
3b. **MCP write** (only if user said yes in step 2b):
   - Call the detected MCP tool to create the task. Map fields:
     - task name → title
     - description (after `|`) → description/notes field
     - due date → due date field (if the tool supports it)
     - assignee (after `→`) → assignee (best-effort; skip if the tool requires a user ID lookup)
   - On MCP failure: do NOT roll back `project-tasks.md`. Report the error clearly (see step 4).
4. Reply:
   - No MCP sync: `✅ Task added.`
   - MCP success: `✅ Task added to project-tasks.md and created in [Tool].`
   - MCP failure: `✅ Task added to project-tasks.md. Could not sync to [Tool]: [error]. Your task is safe — no retry needed.`
