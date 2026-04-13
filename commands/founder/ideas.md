---
description: Capture a raw idea without committing to build it.
argument-hint: [idea text — optional]
---

# /ideas

Capture raw ideas in `ship-log.md` without creating tasks or committing to build.

## Also triggered by
"I have an idea", "add this to the list", "note this down", "maybe we should", "what about adding"

---

## If given text (e.g. `/ideas dark mode`)

1. Append to `## Ideas` in `ship-log.md` as a plain line:
   ```
   - [idea text]
   ```
2. Confirm in one line: `Added: "[idea text]"`

No story format. No ID. No questions. Just append and confirm.

---

## If no text given

Read `## Ideas` from `ship-log.md`. Show the list:

```
## Ideas waiting

- dark mode
- team accounts — wait until 10+ users
- mobile app

Want to promote one to /ship? Type the idea or its number.
```

If the user picks one: trigger `/ship [idea text]` — remove it from Ideas first, then run the ship flow.

If the user says no or nothing: stop.

---

## If Ideas is empty

```
No ideas captured yet.
To add one: /ideas [your idea]
```

---

## Rules
- Never ask clarifying questions when adding an idea — just add it
- Never assign IDs or story format to ideas
- Ideas are cheap — capture everything, commit to nothing
