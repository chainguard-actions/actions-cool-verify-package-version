<!-- markdownlint-disable -->

# Hardening Report: actions-cool--verify-package-version/v1.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions-cool--verify-package-version/v1.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference actions using mutable branch or tag refs instead of pinned full-length commit SHAs, making the action vulnerable to supply-chain attacks.

- `.github/workflows/test.yml`: `uses: actions/checkout@main` (branch ref)
- `.github/workflows/verify-package-version.yml`: `uses: actions/checkout@v2` (tag ref)
- `.github/workflows/verify-package-version.yml`: `uses: actions-cool/verify-package-version@main` (branch ref)

All three should be pinned to a full 40-character commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

Locations:

- `.github/workflows/test.yml:9`
- `.github/workflows/verify-package-version.yml:11`
- `.github/workflows/verify-package-version.yml:12`

### missing-permissions (severity: medium)

Neither `.github/workflows/test.yml` nor `.github/workflows/verify-package-version.yml` defines a top-level `permissions:` key, and no job in either file defines its own `permissions:` block. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/verify-package-version.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three unpinned action references by pinning to full 40-character commit SHAs (with original refs preserved as comments). Added top-level `permissions: {}` blocks to both workflow files (.github/workflows/test.yml and .github/workflows/verify-package-version.yml) to enforce least-privilege access for the GITHUB_TOKEN.

