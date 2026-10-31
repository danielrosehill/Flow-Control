# Flow Control

Agent skills that let you stay in flow while the project keeps its structure.

You're in the middle of building something and think of three other things: a bug you just
noticed, a feature for later, a "what if". Mention them. **Capture** intercepts them, splits and
triages each one (category, priority, current sprint / next sprint / backlog / ideas), files it in
the repo, shows a one-table card, and goes straight back to the task in hand. Nothing is lost and
nothing derails you.

Built for software projects first, but the scaffold and categories work for any project kept in a
repository: writing, research, campaigns, documentation.

## Skills

| Skill | What it does |
| --- | --- |
| `capture` | The intercept. Files asks, ideas, bugs and decisions you mention mid-thread or narrate in a batch, triaged by category and priority into the right place, then resumes the current task. Also re-triages ("make T-12 a P1"). |
| `setup` | Adds a lightweight `planning/` scaffold to an existing repo (inbox, backlog with current and next sprint, ideas, decisions) and a short section in `CLAUDE.md`/`AGENTS.md` so capture fires without being named. Reuses tracking that's already there. |
| `plan-sprint` | Opens the next sprint from queued items, writes a full sprint file (goal, evidence, done-when, targets with baselines, task lists per area or platform) for a fresh session to run, extends a sprint with another task list, and closes sprints. |
| `ship-gate` | Decides when work ships: on your word, when listed items are done, on a date, or each change (release-to-test). Holds releases and keeps a waiting-to-ship list until the gate opens, then ships the sprint once. |
| `next` | "What's next / where are we": brings the files up to date (done items, inbox), reports sprint progress and what's waiting to ship, and picks one next item. |

## Priorities decide placement

| Priority | Meaning | Goes to |
| --- | --- | --- |
| P0 | Blocks or breaks the current work | Current sprint, top, raised with you before switching |
| P1 | Part of what this sprint is about | Current sprint |
| P2 | Soon, not now | Next sprint |
| P3 | Wanted, no time frame | Backlog |
| — | Maybe / what if | Ideas |

## The scaffold

```
planning/
  README.md      conventions for this repo
  inbox.md       untriaged captures ("just park it")
  backlog.md     ship gate, current sprint, next sprint, backlog, ideas, waiting to ship
  decisions.md   decisions, newest first
  sprints/       full sprint files, when a sprint needs one
```

Every item has one id (`T-<n>`) for life, whichever section or category it moves to. Repos that
already track work (a `TODO.md`, GitHub Issues, their own ids) keep their system; the skills file
into it.

## Example

> **You:** …and make the save button disabled until the form is valid. Oh, also, export is
> dropping the last row, and at some point we should support dark mode.

> **Agent:** *(disables the save button, then)*
> ```
> Captured:
> | Id   | Item                       | Cat     | Pri | →              |
> | T-41 | Export drops the last row  | bug     | P1  | current sprint |
> | T-42 | Dark mode                  | feature | P3  | backlog        |
> Back to the form validation.
> ```

## Install

```
/plugin marketplace add danielrosehill/Claude-Code-Plugins
/plugin install flow-control@danielrosehill
```

Then run `setup` in a repo, or just start mentioning things: capture sets up the scaffold on first
use.

## Licence

MIT
