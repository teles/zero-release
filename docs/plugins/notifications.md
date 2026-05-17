---
title: Notifications
parent: Plugins
nav_order: 3
---

# Notification Plugins

Notification plugins run after publish plugins. They are explicit network plugins and are skipped in dry-run mode.

## Order

Notify plugins run in this order when enabled:

```text
webhook
slack
gchat
```

## Slack

The `slack` plugin sends a Slack webhook notification.

Typical usage:

```bash
SLACK_WEBHOOK_URL="https://hooks.slack.com/services/..." \
  zero-release --plugins release-notes,changelog,github-release,slack
```

The webhook is required only for real releases that reach `notify`.

## Generic webhook

The `webhook` plugin sends a generic webhook notification.

Typical usage:

```bash
ZERO_RELEASE_WEBHOOK_URL="https://example.com/release-hook" \
  zero-release --plugins release-notes,changelog,github-release,webhook
```

## Google Chat

The `gchat` plugin sends a Google Chat webhook notification.

Typical usage:

```bash
GCHAT_WEBHOOK_URL="https://chat.googleapis.com/..." \
  zero-release --plugins release-notes,changelog,github-release,gchat
```

## Safety

Network plugins are skipped in dry-run mode and are not required for no-release runs. This keeps pull request previews safe and avoids requiring secrets before a release is actually produced.
