---
name: release-policy
description: Set a repo's standing release policy (when releases go out and when they don't, how versions are numbered and dated, and how QA is done) by applying a ready-made template (post-facto QA, or classic build-and-test-first) or a custom one, and record it in the repo's flow policy (planning/policy.md) so every later session follows it. Use for "set up a release policy", "apply the post-facto template", "we test locally before releasing here", "release, install it for me and I'll test", "how do we version this repo", "change the release policy", and when ship-gate finds no policy.
---

# Release policy

The release policy is the **standing** rule for shipping in one repo: it holds across sprints and
sessions. The ship gate (`ship-gate`) is the **current** state: whether this sprint's work may ship
yet. The policy sets the gate's default and the QA order; the user's words for a given sprint
still override it for that sprint.

## Where it lives: the flow policy

`planning/policy.md` is the repo's flow policy: persistent agent instructions scoped to this
project only, read at the start of any planning or ship work (and by the `## Flow control` section
in `CLAUDE.md`/`AGENTS.md`). The release policy is its `## Release policy` section. No `planning/`
yet: run `setup` in quick mode first. `planning/` exists but no `policy.md`: create it from the
skeleton in `setup`'s [layout](../setup/references/layout.md#policymd).

A repo whose `CLAUDE.md`, `AGENTS.md` or docs already state a release or versioning policy keeps
it: copy the substance into `planning/policy.md`, replace the original with a one-line pointer to
it only on the user's yes, and otherwise note in the policy section where the original lives.
Never keep two policies that can disagree.

## Templates

| Template | QA happens | Default gate | File |
| --- | --- | --- | --- |
| `post-facto` | After release, on the user's own installed copy | each-change | [templates/post-facto-qa.md](templates/post-facto-qa.md) |
| `classic` | Before release, on a local build | on-word | [templates/classic-qa.md](templates/classic-qa.md) |
| `custom` | Wherever the user says | Whatever the user says | Fill the field table below by hand |

Pick from the user's words ("release it, update my copy, I'll test" → post-facto; "test it
locally first", "nothing goes out untested" → classic). If they gave no signal, ask once:

> Release policy for this repo: **post-facto** (release each change, update your installed copy,
> you test the real thing, fix forward) or **classic** (build and test locally, release when it
> passes)?

## Applying a template

1. **Read the repo** for the fields a template leaves open, don't ask for them: the ship step
   (CI workflow on tag, release script, publish or store command), the version scheme and where
   the number lives (`git tag -l`, `package.json`, `pyproject.toml`, `plugin.json`, `build.gradle`,
   a `CHANGELOG.md` heading style), the release record (`CHANGELOG.md`, GitHub Releases, both), the
   test command, and how the user's local copy is installed or updated (a `make install`, `uv tool
   install`, `claude plugin update`, `adb install`, a `.deb`, a package manager). If the user has a
   global versioning rule (their own `CLAUDE.md`, a release-versioning habit), the repo's scheme
   follows it. Write `TBD` for anything you can't find; never invent a command.
2. **Fill the template** into `planning/policy.md` under `## Release policy`, replacing any earlier
   one. Start it with `Set YYYY-MM-DD from template <name>: "<the user's words>".` Keep the
   template's steps; change only what this repo needs, and say what you changed in the reply.
3. **Line up the gate.** If `planning/backlog.md` has no gate, write one in the policy's default
   mode (`ship-gate`, "Setting or changing the gate"). If a gate stands and disagrees with the
   policy (e.g. on-word under post-facto), leave it for this sprint and say so in one line; it
   resets to the policy default when the sprint closes.
4. **Commit** only `planning/policy.md` (and `backlog.md` if the gate changed) in one command:
   `git add <files> && git commit -m "Release policy: <template>" -- <files>`. Push.
5. **Confirm** in three lines at most: template, when releases go out, the QA order, and any
   `TBD` field the user needs to fill.

## Fields

Every policy, template or custom, answers these. A template fills the rows; `custom` asks for
whichever the repo doesn't answer, in one message.

| Field | Answers |
| --- | --- |
| Template | `post-facto`, `classic` or `custom` |
| Release when | Default gate mode (on-word · items · date · each-change) and what that means here |
| Don't release | What never triggers a release on its own (docs-only, planning files, refactors with no user-visible change, half-done work) |
| Ship step | The command or event that makes a new version reach users |
| Version scheme | Format and where the number is written (files, tag) |
| Version bump | Which part moves for which kind of release |
| Release record | Where each release is written down, with its version and date (YYYY-MM-DD) |
| QA order | The ordered steps from "change done" to "release accepted" |
| Sign-off | Who says it works: the user, the agent's checks, or both |
| Local copy | How the user's own installed copy is updated, and how to check its version |
| Broken release | Fix forward, or roll back, and when |
| Hotfix rule | When a fix may ship outside the gate |

## Changing the policy

"From now on we test before releasing", "switch this repo to post-facto", "stop bumping the minor":
rewrite the affected rows (or re-apply another template), add a dated line under the section's
**History** with the user's words, commit, push, one-line confirmation. A one-off exception ("ship
this one without testing") is not a policy change: follow it once and leave the policy alone.

## Where other skills use it

- `ship-gate` reads the policy before any ship step and runs its **QA order** around the ship
  step; on closing a sprint it resets the gate to **Release when**, not to on-word.
- `capture` files standing instructions about how to work in this repo ("always…", "from now on
  in this repo…") into `planning/policy.md`; release-related ones go through this skill.
