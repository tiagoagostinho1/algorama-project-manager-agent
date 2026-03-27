---
description: Re-order the backlog based on value, effort, and current focus.
---

# /po:prioritize

Read `product-backlog.md` and `.claude/CLAUDE.md`. Challenge the current order and suggest a better one.

## Steps
1. List all items in 🔴 Now and 🟡 Next
2. For each, silently score on two axes:
   - **Value** (high/med/low): does this move the product forward for real users?
   - **Effort** (high/med/low): how much work is this roughly?
3. Flag any issues:
   - 🔴 Now has more than 3 items → too many things "in focus"
   - A high-effort/low-value item is in Now or Next → question it
   - Quick wins (low effort / high value) buried in Later → surface them
   - Items that have been in Now for 2+ sessions without shipping → flag as stuck
4. Present a recommended order:

```
## Suggested priority order

### 🔴 Now (max 3)
1. #P002 story — Value: high | Effort: low ← quick win, do this first
2. #P005 story — Value: high | Effort: high ← core feature, already in progress

### 🟡 Next (pick up after Now)
1. #P007 story — Value: high | Effort: low
2. #P003 story — Value: med | Effort: low

### ⚠️ Reconsider
- #P009 story — low value, high effort. Why is this in Next?
- #P011 story — been in Now for 3 sessions. Is this actually stuck?
```

5. Ask: "Apply this order? (y/n) — or tell me what to change."
6. If yes, rewrite the Now and Next sections of `product-backlog.md` in the new order.
7. Reply: "✅ Backlog re-ordered."

## Rules
- Never silently reorder. Always show the proposed change and get confirmation.
- Don't touch Later or Shipped during prioritization.
- If the user says "no", ask what's wrong and adjust.
