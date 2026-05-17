---
title: Troubleshooting
parent: Guide
nav_order: 8
---

# Troubleshooting

Use `zero-release doctor` first when a workflow behaves unexpectedly.

```bash
zero-release doctor
zero-release doctor --json
```

## Diagnosis flow

```mermaid
flowchart TD
  accTitle: Troubleshooting decision flow
  accDescr: The troubleshooting flow checks whether a release was expected, then validates branch, tags, permissions, plugins, and secrets.

  start[Problem] --> releaseExpected{Expected a release?}
  releaseExpected -- No --> dryRun[Use dry-run or analyze to inspect result]
  releaseExpected -- Yes --> branch{Allowed branch?}
  branch -- No --> branches[Check --branches and --prerelease-branches]
  branch -- Yes --> tags{Tags fetched?}
  tags -- No --> checkout[Use actions/checkout fetch-depth 0]
  tags -- Yes --> commits{Release-worthy commits?}
  commits -- No --> rules[Check Conventional Commit release rules]
  commits -- Yes --> permissions{Publishing failed?}
  permissions -- Tag push --> contents[Check contents: write]
  permissions -- GitHub Release --> ghToken[Check GITHUB_TOKEN and github-release plugin]
  permissions -- npm --> npmSetup[Check id-token: write and trusted publisher]
  permissions -- Notifications --> webhooks[Check webhook secrets]
```

## Common symptoms

| Symptom | Check |
|---|---|
| No previous tag is detected | Use `actions/checkout` with `fetch-depth: 0` |
| No release is produced | Confirm commits use `feat:`, `fix:`, `perf:`, or breaking-change syntax |
| Release works locally but not in CI | Check branch allowlists and GitHub event type |
| Tag push fails | Add `permissions: contents: write` |
| GitHub Release fails | Enable `github-release` and provide `GITHUB_TOKEN`, `GH_TOKEN`, or `ZERO_RELEASE_GITHUB_TOKEN` |
| npm publish fails | Configure npm Trusted Publishing and `id-token: write` |
| Changelog changes are not committed | Enable `git-commit` after `changelog` or `package-json` |
| Network plugin runs during PR preview | It should not; confirm `--dry-run` is active or the event is `pull_request` |

## Useful commands

```bash
zero-release --dry-run --debug
zero-release analyze --json
zero-release doctor --json
git tag --list
git log --oneline
```
