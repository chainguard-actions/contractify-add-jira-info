<!-- markdownlint-disable -->

# Hardening Report: contractify--add-jira-info/v1.19.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **contractify--add-jira-info/v1.19.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files reference GitHub Actions using mutable version tags instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the action is compromised. Failing references: actions/checkout@v6, actions/setup-node@v6, toshimaru/auto-author-assign@v3.0.1, actions/upload-artifact@v7.

Locations:

- `.github/workflows/automation.yml:16`
- `.github/workflows/automation.yml:27`
- `.github/workflows/build_test.yml:16`
- `.github/workflows/build_test.yml:18`
- `.github/workflows/check-dist.yml:15`
- `.github/workflows/check-dist.yml:18`
- `.github/workflows/check-dist.yml:37`

### missing-permissions (severity: medium)

build_test.yml and check-dist.yml have no top-level `permissions:` block and no job-level `permissions:` block on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions. Each workflow should declare minimal required permissions explicitly.

Locations:

- `.github/workflows/build_test.yml:1`
- `.github/workflows/check-dist.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across 3 workflow files by pinning to full 40-character commit SHAs (with original tags preserved as comments): actions/checkout@v6→d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6→249970729cb0ef3589644e2896645e5dc5ba9c38, toshimaru/auto-author-assign@v3.0.1→4d585cc37690897bd9015942ed6e766aa7cdb97f, actions/upload-artifact@v7→043fb46d1a93c77aae656e7c1c64a875d1fc6a0a. Added top-level `permissions: contents: read` to build_test.yml and check-dist.yml. automation.yml already had appropriate permissions (contents: read, pull-requests: write) which were preserved.

