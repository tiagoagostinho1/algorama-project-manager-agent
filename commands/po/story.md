---
description: Add a new story or feature idea to the backlog from plain text.
argument-hint: [idea or feature description]
---

# /po:story

Add a product story or feature idea to `product-backlog.md` from natural language.

## Examples
- `/po:story users should be able to export their data as CSV`
- `/po:story the onboarding flow is confusing, need to fix it`
- `/po:story add dark mode — low priority`
- `/po:story blocked: we need a decision on whether to support SSO`

## Steps
1. Parse the input into a story:
   - Rewrite as: `As a [user type], I want [goal] so that [outcome]`
   - If the user type or outcome isn't clear from context, use your best judgement — don't ask unless it's genuinely ambiguous
   - **Section rules:**
     - Contains "blocked", "decision", "unclear", "unsure", "?" → `❓ Under Discussion`
     - Contains "now", "urgent", "today", "critical" → `🔴 Now`
     - Contains "later", "someday", "low priority", "nice to have" → `🟢 Later`
     - Everything else → `🟢 Later` (default — unrefined ideas go to Later)
   - **ID:** Scan `product-backlog.md` for the highest existing `#PNNN`. Next ID = max + 1, zero-padded to 3 digits. If none exist, start at `#P001`.
2. Confirm before adding:
   ```
   Adding to [section]:
   - [ ] #P005 As a [user], I want [goal] so that [outcome]
   Confirm? (y/n)
   ```
2b. **MCP sync check** (only after user confirms):
   - Scan available tools for names starting with any of these prefixes: `mcp__clickup`, `mcp__trello`, `mcp__jira`, `mcp__asana`, `mcp__linear`, `mcp__notion`, `mcp__todoist`, `mcp__monday`, `mcp__basecamp`, `mcp__github`, `mcp__gitlab`
   - If one or more are found: ask "Also create in [Tool Name]? (y/n)"
   - If none found: skip this step silently
   - Session memory: remember the user's answer for the rest of the session. Pre-select it as default on subsequent stories — e.g. `[Y/n]` if they said yes, `[y/N]` if they said no.
3. **Always write to `product-backlog.md` first**, unconditionally, before any MCP call.
3b. **MCP write** (only if user said yes in step 2b):
   - Call the detected MCP tool. Map fields: story text → title/description, section → status/column.
   - On failure: do NOT roll back `product-backlog.md`. Report the error clearly.
4. Reply and offer to keep moving:
   - If story landed in 🟢 Later:
     ```
     ✅ Story added to Later.
     Refine it now? (y/n) — adds acceptance criteria and moves to Next
     ```
     - If yes: run `/po:refine #PXXX` inline
     - If no: done
   - If story landed in 🔴 Now:
     ```
     ✅ Story added to Now.
     Break it down into tasks? (y/n)
     ```
     - If yes: run `/po:breakdown #PXXX` inline
     - If no: done
   - If story landed in ❓ Under Discussion: `✅ Story added to Under Discussion.` — no follow-up prompt, it needs a decision first
   - MCP variants: prepend the sync outcome before the follow-up prompt
     - `✅ Story added to product-backlog.md and created in [Tool].`
     - `✅ Story added to product-backlog.md. Could not sync to [Tool]: [error]. Your story is safe.`

## Rules
- Raw ideas always go to 🟢 Later unless explicitly urgent or blocked. Later items don't need acceptance criteria yet.
- If the idea is vague but worth capturing, capture it as-is. The refine prompt gives the user an immediate path to sharpen it.
- If the input is clearly two separate features, split into two stories with sequential IDs and confirm both at once. Offer to refine the first one after both are saved.
