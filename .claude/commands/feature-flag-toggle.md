---
name: feature-flag-toggle
description: Workflow command scaffold for feature-flag-toggle in codex.
allowed_tools: ["Bash", "Read", "Write", "Grep", "Glob"]
---

# /feature-flag-toggle

Use this workflow when working on **feature-flag-toggle** in `codex`.

## Goal

Enables or disables feature flags, often for plugins, apps, or experimental features.

## Common Files

- `codex-rs/features/src/lib.rs`
- `codex-rs/features/src/tests.rs`
- `codex-rs/core/src/tools/spec_tests.rs`
- `codex-rs/core/src/plugins/discoverable_tests.rs`

## Suggested Sequence

1. Understand the current state and failure mode before editing.
2. Make the smallest coherent change that satisfies the workflow goal.
3. Run the most relevant verification for touched files.
4. Summarize what changed and what still needs review.

## Typical Commit Signals

- Update codex-rs/features/src/lib.rs to change flag state
- Update or add tests in codex-rs/features/src/tests.rs
- Optionally update related files (e.g., core/src/tools/spec_tests.rs, core/src/plugins/discoverable_tests.rs)
- Commit with a message referencing the flag change

## Notes

- Treat this as a scaffold, not a hard-coded script.
- Update the command if the workflow evolves materially.