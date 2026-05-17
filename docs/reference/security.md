---
title: Security Model
parent: Reference
nav_order: 4
---

# Security Model

zero-release keeps its default release path small and explicit.

| Rule | Why it matters |
|---|---|
| No `eval` | Avoids command injection from untrusted input |
| No project config is sourced | Avoids executing repository-provided configuration |
| No `.releaserc` support | Keeps configuration explicit through flags/env |
| No `jq` | Keeps the core free of non-standard runtime dependencies |
| No improvised JSON/YAML config parser | Avoids fragile config parsing behavior |
| No network calls in the core | Keeps network behavior isolated to explicit plugins |
| Network plugins are skipped in dry-run | Keeps PR and preview workflows safe |
| Pull request events default to dry-run | Prevents publishing from PR workflows |
| Releases only run on allowed branches | Avoids accidental releases from arbitrary branches |
| Tags, versions, branch names, and plugin names are validated | Reduces unsafe input handling |
| Commits and commit messages are treated as untrusted input | Prevents release metadata from becoming executable behavior |

## Network access

Network access is opt-in through plugins:

- `github-release`
- `npm`
- `slack`
- `webhook`
- `gchat`

Those plugins are skipped in dry-run mode.

## Pull requests

Pull request workflows default to dry-run in GitHub Actions. This prevents a PR from creating tags, pushing commits, publishing packages, creating GitHub Releases, or sending notifications.

## Secrets

Secrets are only required for real releases that reach the relevant plugin lifecycle. For example, webhook secrets are not required for no-release runs.
