---
title: Release Rules
parent: Guide
nav_order: 5
---

# Release Rules

zero-release uses Conventional Commit style messages to decide whether a release is needed and which SemVer bump to apply.

## Defaults

| Commit pattern | Release type |
|---|---|
| `feat:` / `feat(scope):` | `minor` |
| `fix:` / `fix(scope):` | `patch` |
| `perf:` / `perf(scope):` | `patch` |
| `type!:` / `type(scope)!:` | `major` |
| `BREAKING CHANGE` / `BREAKING-CHANGE` | `major` |
| `chore:` / `docs:` / `test:` | no release |
| `major:` | no release by default |

## Breaking changes

Breaking changes are detected from:

```text
BREAKING CHANGE
BREAKING-CHANGE
feat!: message
feat(scope)!: message
fix!: message
fix(scope)!: message
perf!: message
perf(scope)!: message
refactor(core)!: message
```

`major:` does not create a major release by default.

## Release notes sections

Generated release notes group commits into sections:

| Commit type | Section |
|---|---|
| Breaking change | Breaking Changes |
| `feat` | Features |
| `fix` | Bug Fixes |
| `perf` | Performance |
| Other release-range commits | Other Changes |

When a repository URL is detected, commit links point to the provider commit page.
