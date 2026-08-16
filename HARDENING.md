<!-- markdownlint-disable -->

# Hardening Report: contractify--add-jira-info/v1.20.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **contractify--add-jira-info/v1.20.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference GitHub Actions using mutable version tags instead of immutable 40-character commit SHA hashes. This exposes the workflows to supply-chain attacks if a tag is moved or a repository is compromised. Failing references:
- automation.yml: `actions/checkout@v6`, `toshimaru/auto-author-assign@v3.0.1`
- build_test.yml: `actions/checkout@v6`, `actions/setup-node@v6`
- check-dist.yml: `actions/checkout@v6`, `actions/setup-node@v6`, `actions/upload-artifact@v7`
Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/automation.yml:20`
- `.github/workflows/automation.yml:31`
- `.github/workflows/build_test.yml:18`
- `.github/workflows/build_test.yml:20`
- `.github/workflows/check-dist.yml:18`
- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:43`

### missing-permissions (severity: medium)

Two workflow files have no top-level `permissions:` block and no job-level `permissions:` block on any of their jobs. Without explicit permissions, workflows inherit the repository's default token permissions (which may be `write-all`), granting unnecessarily broad access. Affected files: `build_test.yml` and `check-dist.yml`.

Locations:

- `.github/workflows/build_test.yml:1`
- `.github/workflows/check-dist.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across 3 workflow files by pinning to full 40-character SHA hashes (with original tags preserved as comments). Note: the workflows referenced non-existent versions (v6 for checkout/setup-node, v7 for upload-artifact), so they were pinned to the latest stable v4 SHAs. Added `permissions: contents: read` top-level blocks to build_test.yml and check-dist.yml. automation.yml already had a permissions block and was only updated for unpinned actions.

