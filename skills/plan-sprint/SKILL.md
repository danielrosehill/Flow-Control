---
name: plan-sprint
description: Open, define, extend or close a sprint in the repo's planning/ folder. Promote next-sprint items into a new sprint, write a full sprint file (goal, evidence, done-when, measurable targets with baselines where they apply, one task list per area or platform) that a fresh session can run cold, or add another task list to an existing sprint. Use for "start the next sprint", "define a sprint for X", "make this a sprint across Android and desktop", "set targets", "add a task list for Windows to the sprint", "close this sprint".
---

# Plan a sprint

A sprint is **one goal, a statement of when it's done, and the work to get there**. Small sprints
live entirely in the **Current sprint** section of `planning/backlog.md`. A sprint that spans
areas or platforms, has measurable targets, or will be run by a fresh session gets its own file.

Read `planning/README.md`, `planning/backlog.md` and `planning/sprints/README.md` first; their
conventions win. No `planning/`: run `setup` first. Public repo: follow the hygiene rule.

## Open the next sprint (lightweight)

1. If a sprint is open with unfinished items, ask once: carry them over, or move them to Next
   sprint/Backlog? (Default suggestion: carry over P0/P1, return the rest.)
2. Move the **Next sprint** rows into **Current sprint** (ids unchanged), set the sprint name and
   open date, empty **Next sprint**.
3. Propose a one-line goal from the items and an order (P0, bugs, then the rest). The user can
   change it.
4. Commit only `planning/backlog.md`. Reply: the goal, the items in order, what was carried over.

## Define a sprint file

For a sprint that needs more than a list. Write it for an agent opening the repo cold.

1. **Evidence (why).** The problem in the reporter's words with the date, the diagnosis, what was
   ruled out. Link logs, research notes, result files. **Never invent a number**: an unmeasured
   baseline is written "not measured" and becomes a first task ("measure X").
2. **Done when.** Always. Plain statements of what must be true.
3. **Targets**, when success is measurable: one row each with *what* (a threshold), *measured by*
   (script, query, test, manual check, with a path) and *baseline* (number and date, or "not
   measured"). Add non-regression targets for what the change could harm, and a parity target if
   the sprint spans platforms. Targets are goals; never report them as results.
4. **Task lists**, one per area or platform (`### Shared`, `### Android`, `### Desktop`,
   `### Chapter 3`...), shared groundwork first. Each item: id from the repo's sequence (pull
   existing items from the backlog with their ids rather than re-creating them), what and where,
   a **Check** (target or test), status. Cross-platform items come in pairs that say "parity with
   <id>". Blockers first.
5. **Out of this sprint, related**: what belongs elsewhere, so the executing session doesn't drift.

File `planning/sprints/YYYY-MM-DD-<slug>.md`:

```markdown
---
sprint: <slug>
title: <title>
created: YYYY-MM-DD
areas: [<area>, ...]
status: planned
ship: <how it ships: the ship gate, or "with <version>">
---

# Sprint: <title>
## Goal
## Why (evidence)
## Done when
## Targets          (table: # | Target | Measured by | Baseline; omit if nothing is measurable)
## Task lists       (### per area; table: Id | Item | Check | Status)
## Out of this sprint, related
```

Then link it: a row in `planning/sprints/README.md` (create the folder and index if missing), and
the link in **Current sprint** (or **Next sprint** if it's planned for later) of `backlog.md`,
removing the moved rows from the backlog so each item lives in one place. Record a decision in
`decisions.md` only if defining the sprint settled a choice.

Reply with the file path, goal, targets one per line, id range, anything "not measured", and the
line to start it in a new session:

> Run the sprint in `planning/sprints/<file>`: start with the first task list, and update item
> statuses and target results in the file as you go.

## Add a task list to a sprint

1. Find the sprint: the one named, else the one `in progress`, else the newest `planned`; ask if
   several fit.
2. Split the new items as `capture` does. Already present → add detail, don't duplicate. Same area
   as an existing list → append there; otherwise a new `### <area>` section after the lists it
   depends on.
3. Every item's **Check** names a target or test. Add targets (with baselines or "not measured")
   if the new list needs them; never loosen existing targets without the user saying so.
4. Update front matter (`areas`), the index, and **Done when** if closing now needs more.

## Close a sprint

When the sprint ships (`ship-gate`) or the user says it's finished: set `status: done` (or
`shipped <version>`), record target results with how they were measured, move unfinished items
back to **Next sprint** or **Backlog** with their ids, and open the next sprint if the user wants.

## Commit

Other sessions may be committing in the same repo: stage only your files and commit in one command
(`git add <files> && git commit -m "…" -- <files>`). Push if that is the repo's practice.
