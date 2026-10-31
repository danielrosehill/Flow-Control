---
name: next
description: Get the user back into flow - where the current sprint stands, what's next, and what is waiting to ship - and keep the planning files true by marking finished items done and triaging anything parked in the inbox. Use for "what's next", "where are we", "what's left in the sprint", "I'm back", "pick something", at the start of a session in a repo with planning/, and after finishing an item.
---

# Next

Read `planning/README.md`, `planning/backlog.md` and the current sprint file if one is linked.
No `planning/`: say so in one line and offer `setup`.

## 1. Make the files true

- **Done but not marked:** check recent commits (`git log --since=<sprint open date> --oneline`)
  against open items. Mark an item `done` only when the commit or the code shows it, and note the
  commit; if it's only probably done, list it as "looks done, confirm?".
- **Inbox:** triage anything in `planning/inbox.md` with `capture`'s rules, and empty the triaged
  lines.
- **Ship gate:** if a done item should be shipped, move it to **Waiting to ship**. If the gate's
  condition is now met (items or date mode), say so.

Commit only the planning files you changed, in one command.

## 2. Report

Short, in this order:

```
Sprint: <name> (opened YYYY-MM-DD), goal: <goal>. 4 of 9 done.
Next: T-38 Export drops the last row (bug, P1), then T-41, T-44.
Waiting to ship: T-31, T-35 (gate: on your word).
Inbox: 2 items triaged (T-45 P2, T-46 idea).
```

Pick **one** next item and say why in a few words (P0 first, then blockers, then bugs, then the
user's stated order). If the current sprint is empty, offer to open the next one (`plan-sprint`).

## 3. Go

If the user asked "what's next" while ready to work, start on the chosen item unless they pick
another. If they asked "where are we", stop after the report.
