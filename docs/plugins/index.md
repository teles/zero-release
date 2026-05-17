---
title: Plugins
nav_order: 3
has_children: true
---

# Plugins

Plugins are small executable hook adapters. They run only when listed in `--plugins` or the GitHub Action `plugins` input.

Network plugins are never run in `--dry-run`.

## Built-in plugins

| Plugin | Lifecycle hook | Changes files? | Network? | Purpose |
|---|---|---:|---:|---|
| `release-notes` | `generate-notes` | No | No | Generates release notes |
| `changelog` | `prepare` | Yes | No | Updates `CHANGELOG.md` or `--changelog-file` |
| `package-json` | `prepare` | Yes | No | Updates the top-level `version` field in `package.json` |
| `git-commit` | `prepare` | Yes | No | Commits changed release assets when explicitly enabled |
| `npm` | `publish` | No | Yes | Publishes with `npm publish` using npm Trusted Publishing/OIDC |
| `github-release` | `publish` | No | Yes | Creates a GitHub Release with the generated Markdown notes |
| `slack` | `notify` | No | Yes | Sends a Slack webhook notification |
| `webhook` | `notify` | No | Yes | Sends a generic webhook notification |
| `gchat` | `notify` | No | Yes | Sends a Google Chat webhook notification |

## Release notes and changelog

The core always creates a release notes file before `prepare` and `publish`.

`release-notes` makes generation explicit, but the generated notes are available even if the plugin is not listed. This means `changelog`, annotated Git tags, and `github-release` can still consume notes without `release-notes`.

`changelog` is different: it writes those notes into a persistent file, usually `CHANGELOG.md`.

## Asset-changing plugins

`changelog`, `package-json`, and `git-commit` can change the working tree.

`git-commit` should be enabled when you want zero-release to commit changed release assets before the tag is created.

## Network plugins

Network behavior is opt-in:

| Plugin | Required tool or secret |
|---|---|
| `github-release` | `curl` and `GITHUB_TOKEN`, `GH_TOKEN`, or `ZERO_RELEASE_GITHUB_TOKEN` |
| `npm` | npm CLI and a configured npm trusted publisher |
| `slack` | `curl` and a webhook URL |
| `webhook` | `curl` and a webhook URL |
| `gchat` | `curl` and a webhook URL |

Network plugin secrets are not required for dry-run or no-release runs. They are required for real releases that reach the plugin lifecycle.

## Choosing a plugin set

For a typical GitHub repository:

```bash
zero-release --plugins release-notes,changelog,package-json,github-release
```

For an npm package with trusted publishing:

```bash
zero-release --plugins release-notes,changelog,package-json,git-commit,npm,github-release
```

For preview-only CI:

```bash
zero-release --dry-run --plugins release-notes,changelog,package-json,github-release
```
