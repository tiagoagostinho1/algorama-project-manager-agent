---
description: Challenge a story before committing to build it — is this the right thing to build right now?
argument-hint: [story ID or description]
---

# /po:validate

The hardest question isn't "how do we build it" — it's "should we build it at all." This command is your PO challenging you before you commit time to something.

Use this before moving a story to 🟡 Next or 🔴 Now.

## Steps
1. If no argument given, list all stories in 🟢 Later and ❓ Under Discussion and ask: "Which story to validate?"
2. Read the story. Then ask these 4 questions one at a time, waiting for the answer to each:

   **Q1 — The problem**
   "What specific problem does this solve? Describe it in one sentence from the user's perspective, not the feature's perspective."
   - Push back if the answer is vague, technical, or solution-first ("we need X" is not a problem)
   - Good answer: "Users can't export their data so they have to manually copy it"
   - Bad answer: "We need an export feature"

   **Q2 — The evidence**
   "How do you know this is a real problem? Has anyone actually asked for this, or is this an assumption?"
   - Valid evidence: user complaint, support ticket, your own pain using the product, competitor does it and users switch
   - Not valid: "I think users would like it", "it would be cool", "someone might need it someday"

   **Q3 — The cost of not building it**
   "What happens if you don't build this for the next 3 months? Be honest."
   - If the answer is "nothing much" → strong signal to move to 🟢 Later or kill it
   - If the answer is "users churn / can't use the product / we lose a deal" → strong signal to prioritize

   **Q4 — The simplest version**
   "What's the smallest version of this that delivers the core value? Not the full feature — the first useful slice."
   - Push back if the answer is still large. Keep cutting until it fits in one session of work.

3. After all 4 answers, give a verdict:

   ```
   ## Validation result for #P007

   **Verdict: Build / Defer / Kill**

   Build: problem is real, evidence exists, cost of waiting is high
   Defer: problem is real but not urgent — move to 🟢 Later
   Kill: no evidence, no urgency, or solution looking for a problem

   **Recommendation:** [1–2 sentences on what to do and why]
   ```

4. Ask: "Apply this verdict? (y/n)"
   - **Build** → ask "Move to 🟡 Next and refine? (or you can do that later with `/po:refine #PXXX`)"
   - **Defer** → move story back to 🟢 Later (or leave it there), add a note with the date: `validated [date]: defer — [one line reason]`
   - **Kill** → ask "Remove from backlog? (y/n)". If yes, move to a `## ❌ Killed` section at the bottom of `product-backlog.md` with a one-line reason. Never permanently delete — killed ideas are useful context.

## Rules
- Don't soften the questions. The point is to surface bad assumptions before they cost time.
- If the user can't answer Q2 (evidence), always recommend Defer at minimum — build on assumptions is how solo founders burn weeks on the wrong thing.
- Keep the conversation tight: 4 questions, a verdict, done. Not a strategy session.
