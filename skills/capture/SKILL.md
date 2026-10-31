---
name: capture
description: Intercept new ideas, requests, bugs and decisions the user mentions in the middle of other work, triage them (split, categorise, prioritise, place in current sprint, next sprint, backlog or ideas), file them in planning/, show a one-table triage card, then carry on with the task in hand. Use whenever the user drops something outside the current task ("oh, also…", "while I think of it", "at some point we should…", "for the next version…", "another bug:", "note that…", "park this"), when they narrate a batch of asks at once (typed or dictated), and when they re-triage ("make T-12 a P1", "move that to next sprint").
---

# Capture: file it, sort it, keep going

The user is in the middle of something. They mention a new thing. The two bad outcomes are
**derailing** (starting the new thing now) and **losing it** (it stays in the chat). Capture does
neither: it files the item where it belongs, tells the user in a few lines where it went, and goes
back to the current task in the same turn.

## 0. Is it a capture?

- **Part of the current task** (a steer, a correction, a detail of what's being built): not a
  capture. Just do it.
- **A different piece of work**, however small: capture. If it is tiny and the user clearly wants
  it now ("fix that typo too"), doing it is fine; say so in one line.
- **Mixed** ("change the colour, and later we need dark mode"): do the first, capture the second.

When unsure, capture: an item filed by mistake costs one line to drop; a lost one costs more.

## 1. Find the planning files

Read `planning/README.md` (conventions), `planning/backlog.md`, and the current sprint's file if
`backlog.md` links one. These override the defaults in `setup`'s `references/layout.md`. If the repo
tracks work elsewhere (README or CLAUDE.md says GitHub Issues, a `TODO.md`, `planning/bugs/`
folders), file there in its format.

**No planning at all:** run `setup` in quick mode (no questions), say so in one line, and continue.

## 2. Split

One item per distinct piece of work.
- One sentence can hold two items ("fix the header and make it sticky": a bug and a feature).
- The user circling back to something already said, or already filed: one item; add the new detail
  to the existing entry instead of creating another. Check the backlog and sprint file for it.
- Keep the user's specifics (numbers, names, defaults). Don't invent steps, causes or acceptance
  criteria; write `TBD`.

## 3. Triage

For each item:
- **Category** from the repo's list (defaults: code `bug`/`feature`/`docs`/`chore`/`idea`; other
  `task`/`content`/`research`/`admin`/`idea`). The user's label is a hint: "feature: the form
  doesn't send" is a bug; say so in the card.
- **Priority**, which decides placement:

  | | Signal | Goes to |
  | --- | --- | --- |
  | P0 | Blocks or breaks the current work; "now", "urgent", data loss, users can't use it | Current sprint, top, and raised (step 5) |
  | P1 | Part of what this sprint is about; "this sprint", "before we ship" | Current sprint |
  | P2 | "Next", "soon", "for the next version"; wanted but outside this sprint's goal | Next sprint |
  | P3 | Wanted, no time frame | Backlog |
  | — | "Maybe", "what if", "at some point", "could be cool" | Ideas |

  The user's words outrank your judgement. With no signal, compare to the current sprint's goal:
  fits it → P1, otherwise P2 for bugs and P3 for the rest.
- **Decisions** ("from now on we use X", "we're not supporting Y"): not an item; append to
  `planning/decisions.md` with the user's words.
- **"Just park it" / "don't sort it now"**: append to `planning/inbox.md` untriaged; `next` triages
  it later.

If a split, category or priority is a guess, file the best guess and mark it with `?` in the card.
Don't stop to ask.

## 4. File

- Ids: the repo's sequence (default `T-<n>`, highest existing + 1 across all of `planning/`).
- Write each item as a row in the right section, in that section's table format. Anything longer
  than a row (the user's full words, steps, links, screenshots) goes in a `### T-<n>` block under
  **Notes**.
- Current sprint defined in its own file: P0/P1 items go into that file's matching task list
  (by area), with a `Check` of `TBD` if the sprint uses targets.
- Commit only the planning files you touched, in one command, separate from the work in progress:
  `git add <files> && git commit -m "Capture: T-41 bug P1, T-42 idea" -- <files>`. Never stage
  the in-progress work with it. Push if that is the repo's practice.

## 5. Report and resume

One compact card, then straight back to work:

```
Captured:
| Id   | Item                          | Cat     | Pri | → |
| T-41 | Export drops the last row     | bug     | P1  | current sprint |
| T-42 | Dark mode                     | feature | P2  | next sprint |
| T-43 | Plugin system?                | idea    | —   | ideas |
Back to the settings refactor.
```

Then continue the current task in the same reply. No questions unless something is P0.

**P0:** file it, then ask one line: "T-41 blocks <current task>. Switch to it now, or finish
<current step> first?" Don't switch without an answer.

## Batch mode

When the user narrates many items at once (a list, a voice note transcript, "here's everything for
the next sprint"), the same steps apply; the card is longer, and add a **Check** line for guessed
splits and recategorised items. If there was no task in hand, end with a proposed order of work
for the current sprint (P0, then bugs, then the rest) and stop; don't start building.

## Re-triage

"Make T-42 a P1", "move T-43 to next sprint", "drop T-40": move the row (the id stays), update
priority or status (`dropped` with the reason), commit, one-line confirmation.

## Notes

- Never leave an item only in the chat.
- Never change the current sprint's goal or targets from capture. If a capture suggests the goal
  is wrong, say so in one line; changing it is `plan-sprint`.
- If the item belongs to another repo, say which and file it there only if that repo has
  `planning/`; otherwise list it in the card as "not filed: belongs in <repo>".
