---
title: Concepts
parent: Guide
nav_order: 2
---

# Concepts

This page defines the release terms zero-release uses throughout the docs.

## Tags and releases

A Git tag is a Git reference that points to a commit. zero-release creates annotated tags by default:

```text
v1.2.3 -> commit abc123
```

A GitHub Release is platform metadata attached to a Git tag. It can have a title, body, assets, and prerelease state. It is created only when the `github-release` plugin is enabled.

```mermaid
flowchart LR
  accTitle: Relationship between commits tags and GitHub Releases
  accDescr: A commit is marked by a Git tag, and GitHub can attach a release object to that tag.

  commit[Git commit] --> tag[Git tag]
  tag --> githubRelease[GitHub Release]
  notes[Release notes] --> githubRelease
  notes --> annotatedTag[Annotated tag message]
```

## Release notes and changelog

Release notes describe one release. zero-release always creates a release notes file before `prepare` and `publish`.

A changelog is the persistent history of release notes, usually stored in `CHANGELOG.md`.

| Term | Scope | In zero-release |
|---|---|---|
| Release notes | One version | Generated for the current release |
| Changelog | Release history | Updated by the `changelog` plugin |
| Git tag | Exact code point | Created and pushed by the core |
| GitHub Release | GitHub publication | Created by the `github-release` plugin |

## Core and plugins

The core does local release automation: commit analysis, version calculation, notes, tags, and hook orchestration.

Plugins are explicit adapters for file changes, publishing, and notifications.

```mermaid
flowchart TD
  accTitle: Core and plugin boundary
  accDescr: The zero-release core performs local analysis and tag work, while plugins handle file updates and network publishing.

  core[zero-release core] --> local[Local Git and notes work]
  core --> filePlugins[File plugins]
  core --> networkPlugins[Network plugins]

  filePlugins --> changelog[CHANGELOG.md]
  filePlugins --> packageJson[package.json]
  filePlugins --> releaseCommit[release commit]

  networkPlugins --> githubRelease[GitHub Release]
  networkPlugins --> npm[npm package]
  networkPlugins --> notifications[Slack webhooks Google Chat]
```

## Dry-run

Dry-run mode answers "what would happen?" without changing files, creating tags, pushing commits, publishing packages, creating GitHub Releases, or sending notifications.

Pull request workflows default to dry-run in GitHub Actions.

