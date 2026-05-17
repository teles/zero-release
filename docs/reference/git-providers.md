---
title: Git Providers
parent: Reference
nav_order: 3
---

# Git Providers

zero-release uses Git remotes to build compare and commit URLs when possible.

## Supported URL forms

Remote URL helpers support common HTTPS URLs and SSH conversion such as:

```text
git@github.com:user/repo.git
https://github.com/user/repo.git
```

## Provider links

Compare and commit URLs are generated for:

- GitHub
- GitLab
- Bitbucket
- Azure DevOps

When a provider URL cannot be detected, release notes still include short commit hashes without provider links.

## GitHub repository detection

The `github-release` plugin determines the target repository from:

1. `GITHUB_REPOSITORY`, when present.
2. A parseable GitHub repository URL from zero-release provider context.

The expected repository form is:

```text
owner/name
```

For GitHub Enterprise, set `GITHUB_API_URL` to the API base URL.
