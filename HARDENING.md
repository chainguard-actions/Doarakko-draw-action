<!-- markdownlint-disable -->

# Hardening Report: Doarakko--draw-action/v1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Doarakko--draw-action/v1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file uses `Doarakko/draw-action@main`, which pins to a mutable branch name (`main`) rather than an immutable 40-character commit SHA. This means any future commit pushed to that branch will be automatically used, enabling a supply-chain attack if the referenced repository is compromised or the branch is force-pushed.

Locations:

- `.github/workflows/main.yml:11`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key, and the `draw` job also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions (which may be `write-all`). Minimal permissions (e.g., `issues: write` for posting comments) should be declared explicitly.

Locations:

- `.github/workflows/main.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed hardened/action/.github/workflows/main.yml: (1) Pinned `Doarakko/draw-action@main` to the full commit SHA `00cf6214edf61fb2c04e5e7558e1ded9c854da7d` with a `# main` comment for readability. (2) Added a top-level `permissions:` block with `issues: write` — the minimal permission required for the action to post Yu-Gi-Oh! card draws as comments on GitHub issues.

