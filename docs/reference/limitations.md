---
title: Limitations
parent: Reference
nav_order: 6
---

# Limitations

zero-release intentionally keeps a small scope.

## Current limitations

| Limitation | Notes |
|---|---|
| No config files | No `.releaserc`, `.releaserc.json`, `.releaserc.yml`, or `package.json#release` |
| No compatibility layer | semantic-release compatibility may be explored later |
| No custom release rules | Default Conventional Commit rules only |
| No GitLab release plugin yet | GitLab release creation is not implemented yet |
| Minimal `package-json` update | Updates the top-level `"version"` field using a minimal text-based strategy; complex JSON rewriting is out of scope |
| Simple prerelease support | Branch/channel prereleases are intentionally limited |

## Roadmap

- Optional config loader plugins:
  - `.releaserc`
  - `.releaserc.json`
  - `.releaserc.yml`
  - `package.json#release`
- Semantic-release compatibility layer.
- GitLab Release plugin.
- MCP server/wrapper.
- Optional Docker image for generic CI systems.
- Custom release rules.
- Custom changelog templates.
