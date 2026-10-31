---
name: ship-gate
description: Decide when work ships (a release, a publish, a delivery) instead of shipping every change. Set, read and close a per-repo ship gate in planning/backlog.md (on the user's word, when listed items are done, on a date, or each change for release-to-test), keep the waiting-to-ship list, and ship the sprint as one release when the gate opens. Use when the user says when the next release should happen ("hold releases", "don't release yet", "I'll tell you when the sprint is done", "release when X and Y are in", "release each fix so I can test it on my phone"), when they say the sprint is done ("ship it", "release the sprint"), and BEFORE any step that releases, publishes or delivers (version bump, tag, store upload, publish, send to client).
---

# Ship gate

**Committing and pushing are never gated.** Finished work is committed and pushed as usual. Only
the *ship step* waits: whatever makes a new version reach its users. Find out what that is in this
repo before anything else (CI workflow on tag or version bump, a release script, store upload, a
publish command, sending a document); write it in one line in the gate. A repo's own docs on *how*
to ship always apply; this skill decides only *when*.

## Where the gate lives

A `## Ship gate` section at the top of `planning/backlog.md` (run `setup` if there's no
`planning/`). One gate per independently shipped thing; if a repo ships an Android app and a
desktop app separately, one row per app.

```markdown
## Ship gate

Set YYYY-MM-DD: "<the user's words>".

| Field | Value |
| --- | --- |
| Ship step | <what counts as shipping here, e.g. "push a v* tag (CI builds and publishes)"> |
| Mode | on-word · items · date · each-change |
| Condition | <"the user says the sprint is done" / T-38, T-40 done / 2026-11-14 / every testable change> |
| Covers | <which app, package or deliverable> |
| Hotfix rule | Ship early only if the current version is broken for its users (won't install, crashes on start, loses data), and say why in the same message. |
```

Items done since the last ship are listed under **Waiting to ship** in `backlog.md`.

### Modes

- **on-word** (default when the user says "I'll tell you" or "hold"): nothing ships until the user
  says the sprint is done.
- **items:** ship once every listed item is done and verified, then ship without asking again: the
  condition was the approval.
- **date:** ship on or after the date with whatever is done; say what's in it.
- **each-change** (release-to-test): every testable change ships, because the shipped build is how
  the user tests (e.g. on their own device). Bump only the smallest version part per test build.

If the user's words fit none of these, write a custom row answering: what triggers shipping, what
happens to finished work in between, whether it's tested before or by shipping, which version part
moves, and when a hotfix may go early.

## Setting or changing the gate

1. Read the existing gate; don't keep two.
2. Write the block with the user's words quoted and the date. List anything already done but not
   shipped under **Waiting to ship**.
3. Commit only `planning/backlog.md` in one command and push.
4. If another skill, script or standing instruction ships automatically, the gate overrides it
   while it stands: say so in one line.
5. Confirm in two lines: the mode, the ship step, and when the next ship will happen.

## Before any ship step

1. Read the gate. **No gate:** commit and push, don't ship, and ask once: "Ship now, or hold for
   the sprint?" Write the answer as a gate.
2. **Not met:** stop before the ship step, add the work to **Waiting to ship**, commit, push, and
   say in one line that it's held and what the condition is.
3. **Met** (or the user says the sprint is done): run the repo's ship procedure once for everything
   in **Waiting to ship**, write that list into the changelog or release notes the repo keeps, and
   verify it actually shipped: check the CI run, the published release or the store listing, not
   just that the tag was pushed.
4. Then close the sprint (`plan-sprint`, "Close a sprint"): empty **Waiting to ship**, and reset the
   gate to **on-word** for the next sprint unless the user said otherwise.

A bug in the shipped version: fix, commit, push, add to **Waiting to ship**. Ship early only under
the hotfix rule.
