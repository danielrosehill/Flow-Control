---
name: setup
description: Set up a lightweight project-management scaffold in an existing repository (planning/ with inbox, backlog with current and next sprint, ideas, decisions), adapted to the repo, code or not, and wire capture into CLAUDE.md/AGENTS.md so new asks are triaged automatically. Use for "set up project management here", "add a backlog / to-do tracking to this repo", "set up flow control", or when capture finds no planning folder.
---

# Set up the planning scaffold

Goal: the smallest structure that lets the user throw ideas at the agent at any time and find them
later, sorted. Defaults and templates: [references/layout.md](references/layout.md).

## 1. Look before creating

- **Existing tracking:** `planning/`, `docs/planning/`, `TODO.md`, `BACKLOG.md`, `ROADMAP.md`,
  `CHANGELOG.md` "Unreleased", `planning/bugs/`-style folders, a decision log, or a statement in
  `README`/`CLAUDE.md`/`AGENTS.md` that work is tracked in GitHub Issues or another tool.
  If any exists, **don't build a parallel system.** Add only what is missing (usually the priority
  and sprint sections, or the CLAUDE.md section), link to the existing files from
  `planning/README.md`, and record in it where each kind of item goes. Moving or rewriting existing
  files needs the user's yes.
- **Kind of project:** code or not (sets the default categories), and its areas or platforms if it
  has several (`android/`, `web/`, `docs/`, chapters, campaigns). Read them from the layout; don't
  ask.
- **Public or private:** `gh repo view --json visibility` or the remote host. Public repos get the
  hygiene rule in `planning/README.md`.

## 2. Ask at most one question

Only if the answer can't be read from the repo, ask once, in one message, e.g.:

> Setting up `planning/` here. It's a public repo: commit the planning files (visible to anyone),
> or keep them local with `.gitignore`?

When `capture` calls this skill mid-task (**quick mode**), ask nothing: use the defaults, commit
(or gitignore if the repo is public and has no visible planning), and mention it in one line.

## 3. Create

Write the files from [references/layout.md](references/layout.md): `planning/README.md` (the
conventions, adapted: categories for this kind of project, the repo's existing ids if any, the
hygiene rule if public), `inbox.md`, `backlog.md`, `decisions.md`, `policy.md`. Use absolute dates. Each file
starts with one line saying when and why it was created.

Then add the **Flow control** section from the layout file to the repo's agent instructions file
(`CLAUDE.md`, or `AGENTS.md` if that's what the repo uses; create a minimal `CLAUDE.md` if neither
exists). That section is what makes capture an intercept rather than a command.

If the user has said how releases and testing should work here, run `release-policy` with their
words; otherwise leave the policy's release section as the stub.

If the conversation so far already holds asks or ideas, run `capture` on them now.

## 4. Commit and report

Other work may be in progress in the tree: stage only the files this skill created or changed,
and commit them in one command (`git add <files> && git commit -m "Set up planning scaffold" -- <files>`).
Push if the repo has a remote and the user's practice is to push.

Reply with the tree created (or the parts added to existing tracking), the categories in use, one
line saying a release policy can be applied with `release-policy` (post-facto or classic) if none
was set, and one line: "From here, just mention things as they come up; I'll file them and keep
going."
