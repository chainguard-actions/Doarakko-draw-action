<!-- markdownlint-disable -->

# Hardening Report: Doarakko--draw-action/v1.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Doarakko--draw-action/v1.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or version strings instead of immutable 40-character commit SHAs, making them vulnerable to supply-chain attacks if the referenced tag is moved.

.github/workflows/check-dist.yml:
  - uses: actions/checkout@v4
  - uses: actions/setup-node@v4.0.3
  - uses: actions/upload-artifact@v4

.github/workflows/eslint.yml:
  - uses: actions/checkout@v4
  - uses: actions/setup-node@v4.0.3

.github/workflows/main.yml:
  - uses: Doarakko/draw-action@main  (mutable branch reference)

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:44`
- `.github/workflows/eslint.yml:6`
- `.github/workflows/eslint.yml:7`
- `.github/workflows/main.yml:11`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` block, and no job within them defines job-level permissions either. Without explicit permissions, workflows run with the default (potentially broad) token permissions. Each workflow should declare minimal required permissions.

- .github/workflows/check-dist.yml: no top-level or job-level permissions
- .github/workflows/eslint.yml: no top-level or job-level permissions
- .github/workflows/main.yml: no top-level or job-level permissions

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/eslint.yml:1`
- `.github/workflows/main.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

**unpinned-uses** (6 references pinned):
- actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4
- actions/setup-node@v4.0.3 → @1e60f620b9541d16bece96c5465dc8ee9832be0b # v4.0.3
- actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02 # v4
- Doarakko/draw-action@main → @00cf6214edf61fb2c04e5e7558e1ded9c854da7d # main

**missing-permissions** (3 files updated):
- check-dist.yml: added `permissions: {}` (no token access needed for dist checking)
- eslint.yml: added `permissions: {}` (no token access needed for linting)
- main.yml: added `permissions: { issues: write }` (needed for the draw-action to post comments on issues)

