---
name: update-github-info
description: Refresh the GitHub Info page with practical, sourced updates from GitHub Blog and Changelog.
intent: Keep Mona's GitHub Info page current with concise, useful guidance backed by official GitHub sources.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  copilot-requests: write
engine:
  id: copilot
  copilot-sdk: true
tools:
  edit: true
  web-fetch: {}
network:
  allowed:
    - defaults
    - copilot
    - github.blog
    - github.com
safe-outputs:
  create-pull-request:
    allowed-files:
      - site/content/github-info.md
    base-branch: main
    draft: false
    max: 1
  noop:
---

# Update GitHub Info

Read `notes/mona-notes.md` and the current `site/content/github-info.md` before making changes.

Use web fetch to read both:

- https://github.blog/latest/
- https://github.blog/changelog/

Identify recent, official updates that offer a practical takeaway for developers and fit the site's existing editorial angle. Do not invent details, dates, or announcements. Preserve the existing useful content and structure; update only `site/content/github-info.md`. For each addition, include a concise summary and a direct source link, naming whether it came from the GitHub Blog or Changelog.

Avoid duplicating items already covered on the page. If neither source has a sufficiently useful new item, if the sources cannot be verified, or if the page needs no meaningful change, make no edits and call `noop` with a brief reason.

When there is a meaningful update, open one pull request against `main` for Mona to review. Summarize the changes and link the official sources in the PR description. Submit it only through the configured `create-pull-request` safe output; never write directly to `main` or use another write mechanism. Do not modify any other file.
