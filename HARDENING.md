<!-- markdownlint-disable -->

# Hardening Report: contractify--add-jira-info/v1.18.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **contractify--add-jira-info/v1.18.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or version strings instead of full 40-character commit SHA digests. This exposes the workflow to supply-chain attacks if the referenced tag is moved or overwritten.

automation.yml:
  - actions/checkout@v6
  - toshimaru/auto-author-assign@v3.0.1

build_test.yml:
  - actions/checkout@v6
  - actions/setup-node@v6

check-dist.yml:
  - actions/checkout@v6
  - actions/setup-node@v6
  - actions/upload-artifact@v7

All should be pinned to a full SHA, e.g. actions/checkout@<40-hex-char-sha> # v6

Locations:

- `.github/workflows/automation.yml:18`
- `.github/workflows/automation.yml:28`
- `.github/workflows/build_test.yml:16`
- `.github/workflows/build_test.yml:18`
- `.github/workflows/check-dist.yml:16`
- `.github/workflows/check-dist.yml:20`
- `.github/workflows/check-dist.yml:35`

### missing-permissions (severity: medium)

build_test.yml and check-dist.yml have no top-level `permissions:` key and no job-level `permissions:` block on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, violating the principle of least privilege. A top-level `permissions:` block with specific minimal scopes (e.g. `contents: read`) should be added to each file.

Locations:

- `.github/workflows/build_test.yml:1`
- `.github/workflows/check-dist.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across 3 workflow files by resolving each tag to its full 40-character commit SHA (preserving the tag as a comment). Added top-level `permissions: contents: read` blocks to build_test.yml and check-dist.yml. automation.yml already had a permissions block and only needed the action pins fixed. Specific changes: actions/checkout@v6 → @df4cb1c069e1874edd31b4311f1884172cec0e10, actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38, actions/upload-artifact@v7 → @043fb46d1a93c77aae656e7c1c64a875d1fc6a0a, toshimaru/auto-author-assign@v3.0.1 → @4d585cc37690897bd9015942ed6e766aa7cdb97f.

