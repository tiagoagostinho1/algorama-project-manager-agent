---
description: Refine a backlog item — sharpen the story, add acceptance criteria, and move it to Next.
argument-hint: [story ID or description]
---

# /po:refine

Take a raw story from 🟢 Later or ❓ Under Discussion and make it ready to build.

## Steps
1. If no argument given, show all unrefined items (Later + Under Discussion) and ask: "Which one?"
2. Read the story. Ask clarifying questions if needed (max 3):
   - Who specifically is this for? (be more precise than "user")
   - What's the simplest version that delivers the value?
   - How will we know it's done?
3. Rewrite the story in clean format if needed:
   ```
   As a [specific user type], I want [concrete action] so that [measurable outcome]
   ```
4. Add 2–4 acceptance criteria as an indented list:
   ```
   - [ ] #P003 As a returning user, I want to reset my password via email so that I can regain access
     - Email is sent within 30 seconds
     - Link expires after 1 hour
     - Works on mobile
   ```
5. Confirm: "Move #PXXX to 🟡 Next? (y/n)"
6. Move to `## 🟡 Next (refined and ready)` in `product-backlog.md`
7. Ask: "Break this down into tasks now? (y/n)"
   - If yes: run `/po:breakdown #PXXX` inline — no need for the user to type it separately
   - If no: reply: "✅ #PXXX refined and moved to Next. Run `/po:breakdown #PXXX` when you're ready to start building."
8. After task breakdown (or if user said no to breakdown), ask: "Want a spec file for this story? (y/n)"
   - If yes: run `/pm:spec #PXXX` inline — generates `specs/PXXX-slug.md` with full behaviors, edge cases, and states
   - If no: skip silently

## Rules
- Push back if the story is too vague, too large, or solves a problem nobody has confirmed. Say so directly.
- If it's really two stories, split it. Assign the new one the next available ID.
- Never move to Next without acceptance criteria.
