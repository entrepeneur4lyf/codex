---
name: extract-new-crate
description: Workflow command scaffold for extract-new-crate in codex.
allowed_tools: ["Bash", "Read", "Write", "Grep", "Glob"]
---

# /extract-new-crate

Use this workflow when working on **extract-new-crate** in `codex`.

## Goal

Extracts a module or functionality from an existing crate into a new standalone crate, updating all callsites to use the new crate.

## Common Files

- `codex-rs/Cargo.lock`
- `codex-rs/Cargo.toml`
- `codex-rs/<new-crate>/Cargo.toml`
- `codex-rs/<new-crate>/BUILD.bazel`
- `codex-rs/<new-crate>/src/*.rs`
- `codex-rs/core/Cargo.toml`

## Suggested Sequence

1. Understand the current state and failure mode before editing.
2. Make the smallest coherent change that satisfies the workflow goal.
3. Run the most relevant verification for touched files.
4. Summarize what changed and what still needs review.

## Typical Commit Signals

- Create new crate directory (e.g., codex-rs/<new-crate>/)
- Add BUILD.bazel and Cargo.toml for the new crate
- Move relevant source files from existing crate to new crate
- Update original crate's Cargo.toml to depend on the new crate
- Update code in original crate and other consumers to import from the new crate

## Notes

- Treat this as a scaffold, not a hard-coded script.
- Update the command if the workflow evolves materially.