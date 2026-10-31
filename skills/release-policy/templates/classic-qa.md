# Template: classic QA (build and test first)

For repos where a release reaches people other than the user, is slow or costly to redo, or must
not break: public packages, client deliverables, store apps, production services. Build and test
locally, release only what passed.

Copy the block below into `planning/policy.md`, replacing `<...>` from the repo.

```markdown
## Release policy

Set YYYY-MM-DD from template classic: "<the user's words>".

| Field | Value |
| --- | --- |
| Template | classic |
| Release when | on-word: work collects in **Waiting to ship** until the user says the sprint is done (or the gate names items or a date), then ships as one release. |
| Don't release | Anything that hasn't passed the QA order below; docs-only, planning-only or CI-only changes on their own. |
| Ship step | <e.g. "push a v* tag; CI builds and publishes the release"> |
| Version scheme | <e.g. "vX.Y.Z (semver) in package.json and the tag" / the user's own scheme> |
| Version bump | By the largest change in the release: fixes only → smallest part; new behaviour → next part up; breaking change → major. |
| Release record | <CHANGELOG.md> entry per release: version, date (YYYY-MM-DD), changes grouped as Added / Changed / Fixed, and the QA run it passed. Mirror it in the GitHub release notes if the repo uses them. |
| QA order | 1. Agent: clean build from the release commit. 2. Agent: the full test suite (<test command>) and linters pass. 3. Install or run the local build the way users will (<local install / run command>) and smoke-test the changed features plus the main path. 4. The user tests the local build and says it's good (see Sign-off). 5. Bump the version, write the release record, ship step, verify it published (the CI run, the release page or registry, not just the pushed tag). 6. Update the user's own installed copy to the released version, if they use it. |
| Sign-off | The user, on the local build in step 4. <Or "the agent's checks in steps 1–3", if the user says automated checks are enough here.> |
| Local copy | <how a local build is installed for testing, and how the released version is installed afterwards>; check with <e.g. "<cmd> --version">. |
| Broken release | Roll back if users are affected and the previous version is safe (re-publish or point users to it); then fix through the full QA order. Mark the broken version in the release record. |
| Hotfix rule | A fix for a broken release may ship outside the gate, through QA steps 1–3 and 5 (the user's sign-off may be skipped if they're unavailable and users are affected; say so in the release record). |

### History

- YYYY-MM-DD: set from template classic.
```

## When it fits badly

- The user is the only user and tests by using the installed copy: the local-test step duplicates
  what real use gives them. Suggest `post-facto`.
- No test suite exists: steps 1–2 shrink to "it builds"; say so in the policy rather than leaving
  a test command that doesn't exist.
