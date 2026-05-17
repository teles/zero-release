---
title: FAQ
parent: Guide
nav_order: 9
---

# FAQ

## Does zero-release require Node?

No. The core CLI runs with Bash, Git, and standard Unix tools.

Node is only needed by projects that choose Node-specific workflows, such as running package tests before release or publishing to npm with the `npm` plugin.

## Does the documentation require Node?

No. The documentation source is Markdown plus Jekyll configuration. GitHub Pages builds it with Jekyll.

## What is the difference between a tag and a GitHub Release?

A tag is Git-native. It points to a commit.

A GitHub Release is GitHub metadata attached to a tag. It can include a title, release body, assets, and prerelease state.

zero-release creates the Git tag in the core release flow. It creates a GitHub Release only when the `github-release` plugin is enabled.

## What is the difference between release notes and a changelog?

Release notes describe one release.

A changelog is the persistent history of releases, usually stored in `CHANGELOG.md`.

zero-release always generates notes for the current release. The `changelog` plugin writes those notes into the changelog file.

## Is `release-notes` required when using `github-release`?

No. The core always creates a release notes file before `prepare` and `publish`.

The `release-notes` plugin makes that step explicit, but `github-release`, annotated Git tags, and `changelog` can consume the generated notes even when `release-notes` is not listed.

## Why does `github-release` fail with `--no-tag` or `--no-push`?

The GitHub Release should point to a tag that exists on GitHub.

`--no-tag` prevents tag creation. `--no-push` prevents the tag from reaching GitHub. In both cases, publishing a GitHub Release would be misleading, so the plugin fails clearly.

## Why is `fetch-depth: 0` needed in GitHub Actions?

zero-release needs tags and commit history to find the previous release and analyze the commit range.

The default shallow checkout can hide tags or older commits. Use:

```yaml
- uses: actions/checkout@v4
  with:
    fetch-depth: 0
```

## Which permissions are needed?

For GitHub tags and GitHub Releases:

```yaml
permissions:
  contents: write
```

For npm Trusted Publishing:

```yaml
permissions:
  contents: write
  id-token: write
```

## What happens on pull requests?

Pull request events default to dry-run in GitHub Actions.

This means zero-release can preview the release result without creating commits, tags, pushes, network calls, packages, GitHub Releases, or notifications.

## Can plugins run in any order?

The user-provided `--plugins` order does not control lifecycle order.

zero-release runs plugin hooks in deterministic lifecycle order, so file-changing plugins run before `git-commit`, and publish plugins run after tags are created and pushed.

## How do I know why no release happened?

Run:

```bash
zero-release --dry-run --debug
zero-release analyze --json
zero-release doctor
```

Then check:

- current branch is allowed;
- previous tags are available;
- commits match the release rules;
- pull request dry-run behavior is expected.

