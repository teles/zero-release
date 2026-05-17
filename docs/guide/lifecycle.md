---
title: Lifecycle
parent: Guide
nav_order: 5
---

# Lifecycle

The CLI uses semantic-release-like lifecycle names, but zero-release is not semantic-release compatible.

## Hooks

```text
verify
analyze
verify-release
generate-notes
prepare
publish
notify
success
fail
```

Core analysis and publishing are handled by the CLI. Plugins are executable hook adapters and receive context through `ZERO_RELEASE_*` environment variables.

## Release flow

```mermaid
flowchart TD
  accTitle: zero-release lifecycle flow
  accDescr: zero-release verifies the environment, analyzes commits, generates notes, prepares files, publishes tags and artifacts, then sends notifications.

  start([Start]) --> verify[verify]
  verify --> analyze[analyze commits]
  analyze --> needed{Release needed?}
  needed -- No --> noRelease[Print no-release result]
  needed -- Yes --> version[Calculate version and tag]
  version --> notes[generate-notes]
  notes --> prepare[prepare file changes]
  prepare --> dryRun{Dry-run?}
  dryRun -- Yes --> preview[Preview only]
  dryRun -- No --> tag[Create annotated tag]
  tag --> push[Push tag and optional release commit]
  push --> publish[publish plugins]
  publish --> notify[notify plugins]
  notify --> success[success]
```

If no release-worthy commits are found, zero-release reports that no release was produced and skips release actions.

If `--dry-run` is enabled, zero-release previews the release and skips mutations and network plugins.

## Plugin order

Plugin execution order is deterministic and based on lifecycle responsibility, not on the order passed to `--plugins`.

```mermaid
flowchart LR
  accTitle: Plugin execution order
  accDescr: Plugins are grouped by lifecycle, with prepare plugins before publish plugins and notification plugins at the end.

  verify[verify] --> notes[generate-notes]
  notes --> prepare[prepare]
  prepare --> publish[publish]
  publish --> notify[notify]

  prepare --> changelog[changelog]
  prepare --> packageJson[package-json]
  prepare --> gitCommit[git-commit]

  publish --> npm[npm]
  publish --> githubRelease[github-release]

  notify --> webhook[webhook]
  notify --> slack[slack]
  notify --> gchat[gchat]
```

Prepare plugins run in this order when enabled:

```text
changelog
package-json
git-commit
```

Publish plugins run in this order when enabled:

```text
npm
github-release
```

Notify plugins run in this order when enabled:

```text
webhook
slack
gchat
```

## Generated notes

The core always produces a release notes file before `prepare` and `publish`.

Enabling `release-notes` makes notes generation explicit and leaves room for alternate notes plugins, but `changelog`, annotated Git tags, and `github-release` can consume generated notes even when `release-notes` is not listed.
