# Template: post-facto QA

For repos where the shipped build is how the user tests: personal tools, plugins, apps on the
user's own devices, anything where releasing is cheap and a bad release is fixed by the next one.
Release first, put it on the user's machine, test the real thing, fix forward.

Copy the block below into `planning/policy.md`, replacing `<...>` from the repo.

```markdown
## Release policy

Set YYYY-MM-DD from template post-facto: "<the user's words>".

| Field | Value |
| --- | --- |
| Template | post-facto |
| Release when | each-change: every change the user can see or test ships as its own release, as soon as it builds. Several changes finished in one session may go out as one release. |
| Don't release | Docs-only, planning-only or CI-only changes; refactors with no user-visible effect; work that doesn't build. These are committed and pushed, and ride along with the next real release. |
| Ship step | <e.g. "push a v* tag; CI builds and publishes the release"> |
| Version scheme | <e.g. "vX.Y.Z in plugin.json and the tag" / the user's own scheme> |
| Version bump | Smallest part per release. Next part up only for a breaking change or when the user says so. |
| Release record | <CHANGELOG.md> entry per release: version, date (YYYY-MM-DD), what changed, QA result (`pending`, `ok`, or `broken → fixed in <version>`). |
| QA order | 1. Agent: it builds, and fast automated checks pass (if the repo has them). 2. Ship step, and verify it published (the CI run, the release page or registry, not just the pushed tag). 3. Update the user's local copy (see Local copy) and check the installed version is the new one. 4. Agent smoke check on the installed copy where possible (starts, `--version`, the changed command runs). 5. Tell the user what to test, in one or two lines; the user tests in real use. 6. Record the QA result in the release record when the user reports back. |
| Sign-off | The user, by using it. The agent's checks in steps 1 and 4 only catch a release that can't run. |
| Local copy | <e.g. "claude plugin update <name>" / "uv tool upgrade <name>" / "adb install -r <apk>">; check with <e.g. "<cmd> --version">. |
| Broken release | Fix forward: fix, release the next version through the same QA order, update the local copy again. Roll back (reinstall the previous version) only if the user is blocked and the fix isn't quick. Mark the broken version in the release record. |
| Hotfix rule | Not needed: every fix ships as soon as it builds. |

### History

- YYYY-MM-DD: set from template post-facto.
```

## When it fits badly

- Other people use the released version (a public package, a client, an app store audience): a
  broken release reaches them before the user sees it. Suggest `classic`, or post-facto on a test
  channel (a beta track, a pre-release tag) with `classic` for the public release.
- Releasing is slow or costly (store review, paid builds): batch per session or per sprint instead
  of each change: change **Release when** to `on-word` and keep the QA order.
