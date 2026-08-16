<!-- markdownlint-disable -->

# Hardening Report: contractify--add-jira-info/v1.21.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **contractify--add-jira-info/v1.21.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks where a tag could be silently moved to point to malicious code.

- .github/workflows/automation.yml: `actions/checkout@v6`, `toshimaru/auto-author-assign@v3.0.1`
- .github/workflows/build_test.yml: `actions/checkout@v6`, `actions/setup-node@v6`
- .github/workflows/check-dist.yml: `actions/checkout@v6`, `actions/setup-node@v6`, `actions/upload-artifact@v7`

All of these should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v6`.

Locations:

- `.github/workflows/automation.yml:16`
- `.github/workflows/automation.yml:28`
- `.github/workflows/build_test.yml:16`
- `.github/workflows/build_test.yml:18`
- `.github/workflows/check-dist.yml:14`
- `.github/workflows/check-dist.yml:17`
- `.github/workflows/check-dist.yml:37`

### missing-permissions (severity: medium)

Two workflow files have no top-level `permissions:` block and no job-level `permissions:` block on any of their jobs. Without explicit permissions, workflows inherit the repository's default token permissions, which may be overly broad (e.g. write access to contents). A minimal explicit permissions block should be added.

- .github/workflows/build_test.yml: no permissions declared at top level or job level.
- .github/workflows/check-dist.yml: no permissions declared at top level or job level.

Locations:

- `.github/workflows/build_test.yml:1`
- `.github/workflows/check-dist.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all unpinned action references to full commit SHAs: actions/checkout@v6 → d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6 → 249970729cb0ef3589644e2896645e5dc5ba9c38, actions/upload-artifact@v7 → 043fb46d1a93c77aae656e7c1c64a875d1fc6a0a, toshimaru/auto-author-assign@v3.0.1 → 4d585cc37690897bd9015942ed6e766aa7cdb97f. Added top-level `permissions: contents: read` to build_test.yml and check-dist.yml. automation.yml already had appropriate permissions (contents: read, pull-requests: write).

