# Planning layout and conventions

These are the defaults that `setup` writes into a repo. Once written, the repo's own
`planning/README.md` is the authority: every flow-control skill reads it first and follows it over
this file.

## Tree

```
planning/
  README.md      conventions for this repo (copy of the sections below, adapted)
  inbox.md       raw captures not yet triaged
  backlog.md     ship gate, current sprint, next sprint, backlog, ideas
  decisions.md   decisions, newest first
  sprints/
    README.md    index of sprint files
    YYYY-MM-DD-<slug>.md   one per defined sprint (plan-sprint)
```

Create `sprints/` only when the first sprint is defined, unless the user asks for it up front.

## Ids

One sequence per repo for every item, whatever its category: `T-1`, `T-2`, ... Next id = highest
`T-<n>` anywhere under `planning/` (inbox, backlog, sprint files, done sections) + 1. One sequence
means an item keeps its id when it moves between sections or changes category.

Decisions have their own ids: `D-YYYY-MM-DD-n`.

If the repo already numbers items another way (`B-12`, `BUG-0001`, issue numbers), use that.

## Categories

Code projects: `bug` (broken), `feature` (new or changed behaviour), `docs`, `chore` (maintenance,
tooling, refactor), `idea` (not committed to).

Other projects: `task`, `content`, `research`, `admin`, `idea`.

`decision` is not an item category: a decision goes to `decisions.md`.

A repo can replace these lists in its `planning/README.md`.

## Priorities

Priority decides placement, so a single letter tells the user where the item went.

| Priority | Meaning | Goes to |
| --- | --- | --- |
| P0 | Blocks or breaks the current work, or the user said "now" | Current sprint, top. Raised with the user before switching. |
| P1 | Belongs in what is being done now | Current sprint |
| P2 | Wanted soon, not now | Next sprint |
| P3 | Wanted, no time frame | Backlog |
| none | `idea` category: "maybe", "what if", "at some point" | Ideas |

## Status words

`open`, `in progress`, `done`, `shipped <version or date>`, `dropped` (with a reason).

## backlog.md

```markdown
# Backlog

Created YYYY-MM-DD by flow-control setup. Items live in exactly one place: here, or in the
current sprint's file when one is defined.

## Ship gate

(none set; see the ship-gate skill)

## Current sprint

Sprint: <name, or link to planning/sprints/<file> when defined>. Opened YYYY-MM-DD.

| Id | Item | Category | Priority | Status | Added |
| --- | --- | --- | --- | --- | --- |

## Next sprint

| Id | Item | Category | Priority | Added |
| --- | --- | --- | --- | --- |

## Backlog

| Id | Item | Category | Priority | Added |
| --- | --- | --- | --- | --- |

## Ideas

| Id | Idea | Added |
| --- | --- | --- |

## Waiting to ship

Done items not yet in a release or delivery.

## Notes

### T-<n> <title>
Detail for any item that needs more than its table row: the user's words, steps to reproduce,
links, screenshots.
```

When a sprint has its own file (from `plan-sprint`), the **Current sprint** section holds only the
link, and the items live in the sprint file's task lists.

## inbox.md

```markdown
# Inbox

Untriaged captures, newest first. `capture` or `next` triages them into backlog.md.

- YYYY-MM-DD: <the user's words, lightly cleaned>
```

## decisions.md

```markdown
# Decisions

Newest first.

## D-YYYY-MM-DD-1: <decision>
- Source: <who said it, where, in their words if short>
- Consequence: <what changes because of it>
```

## Public repositories

If the repo is public, `planning/README.md` carries this rule, and every skill follows it: no
personal details beyond the maintainer's public role, no host names, account ids, internal URLs,
credentials or costs; other people unnamed unless they ask to be named. If the user wants planning
kept out of a public repo, add `planning/` to `.gitignore` and say so.

## The CLAUDE.md / AGENTS.md section

`setup` adds this to the repo's agent instructions file. It is what makes capture work as an
intercept, without the user having to name the skill.

```markdown
## Flow control

Planning lives in `planning/` (conventions: `planning/README.md`).

- When the user mentions something outside the task in hand (a new idea, request, bug, "also",
  "at some point", "for the next version"), don't start it. Run flow-control `capture`: triage it
  into `planning/`, show the one-table triage card, then carry on with the current task.
- Before any release, publish or delivery step, read the ship gate in `planning/backlog.md`
  (flow-control `ship-gate`).
- "What's next", "where are we", "I'm back": flow-control `next`.
```
