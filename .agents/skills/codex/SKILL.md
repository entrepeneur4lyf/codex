```markdown
# codex Development Patterns

> Auto-generated skill from repository analysis

## Overview

This skill provides a comprehensive guide to the development patterns, coding conventions, and key workflows used in the `codex` Rust codebase. It covers file organization, code style, commit practices, and detailed step-by-step instructions for common tasks such as extracting crates, toggling feature flags, removing tools, updating APIs, and managing plugins. Use this as a reference for contributing effectively and maintaining consistency across the project.

## Coding Conventions

- **File Naming:**  
  Use `snake_case` for all file and directory names.
  ```
  // Good
  mod my_feature.rs
  src/tool_handler.rs

  // Bad
  mod MyFeature.rs
  src/ToolHandler.rs
  ```

- **Import Style:**  
  Prefer relative imports within the crate.
  ```rust
  // Good
  use super::utils;
  use crate::handlers::my_handler;

  // Bad
  use codex::handlers::my_handler;
  ```

- **Export Style:**  
  Use named exports for modules and functions.
  ```rust
  // Good
  pub mod my_module;
  pub fn my_function() {}

  // Bad
  pub use my_module::*;
  ```

- **Commit Patterns:**  
  - Use prefixes such as `feat`, `fix`, `chore`, and `[plugins]` in commit messages.
  - Keep commit messages concise (average ~53 characters).
  ```
  feat: add new plugin manager API
  fix: correct file handler error propagation
  chore: update dependencies
  [plugins] update menu mentionability
  ```

## Workflows

### Extract New Crate
**Trigger:** When a shared module or functionality needs to be separated for reuse or clarity  
**Command:** `/extract-crate`

1. Create a new crate directory, e.g., `codex-rs/<new-crate>/`.
2. Add `BUILD.bazel` and `Cargo.toml` for the new crate.
3. Move relevant source files from the existing crate to the new crate.
4. Update the original crate's `Cargo.toml` to depend on the new crate.
5. Update code in the original crate and other consumers to import from the new crate.
6. Update workspace-level `Cargo.toml` and `Cargo.lock`.
7. Add or move tests for the extracted functionality.
8. Run CI to verify the extraction.

**Files Involved:**
- `codex-rs/Cargo.lock`
- `codex-rs/Cargo.toml`
- `codex-rs/<new-crate>/Cargo.toml`
- `codex-rs/<new-crate>/BUILD.bazel`
- `codex-rs/<new-crate>/src/*.rs`
- `codex-rs/core/Cargo.toml`
- `codex-rs/core/src/*.rs`

---

### Feature Flag Toggle
**Trigger:** When enabling, disabling, or flipping a feature flag for a new or existing feature  
**Command:** `/flip-feature-flag`

1. Update `codex-rs/features/src/lib.rs` to change the flag state.
2. Update or add tests in `codex-rs/features/src/tests.rs`.
3. Optionally update related files, e.g., `core/src/tools/spec_tests.rs`, `core/src/plugins/discoverable_tests.rs`.
4. Commit with a message referencing the flag change.

**Files Involved:**
- `codex-rs/features/src/lib.rs`
- `codex-rs/features/src/tests.rs`
- `codex-rs/core/src/tools/spec_tests.rs`
- `codex-rs/core/src/plugins/discoverable_tests.rs`

**Example:**
```rust
// features/src/lib.rs
pub const ENABLE_NEW_TOOL: bool = true;
```

---

### Remove Tool or Handler
**Trigger:** When a tool or handler is deprecated or no longer needed  
**Command:** `/remove-tool`

1. Delete the handler/tool source file(s), e.g., `grep_files.rs`, `read_file.rs`, `artifacts.rs`.
2. Remove references from `mod.rs` and `spec.rs`.
3. Remove or update related tests.
4. Remove or update related templates or documentation.
5. Commit with a message referencing the removed tool.

**Files Involved:**
- `codex-rs/core/src/tools/handlers/mod.rs`
- `codex-rs/core/src/tools/handlers/<tool>.rs`
- `codex-rs/core/src/tools/spec.rs`
- `codex-rs/core/src/tools/spec_tests.rs`
- `codex-rs/core/tests/suite/<tool>.rs`
- `codex-rs/core/tests/common/test_codex.rs`

---

### Add or Update API Endpoint or Protocol
**Trigger:** When a new API/protocol method is needed or an existing one changes  
**Command:** `/add-api-endpoint`

1. Update or add JSON schema files in `app-server-protocol/schema/json/`.
2. Update or add TypeScript types in `app-server-protocol/schema/typescript/`.
3. Update Rust protocol implementation in `app-server-protocol/src/protocol/`.
4. Update server-side implementation, e.g., `app-server/src/`.
5. Update or add tests for the endpoint/protocol.
6. Update documentation if needed.

**Files Involved:**
- `codex-rs/app-server-protocol/schema/json/*.json`
- `codex-rs/app-server-protocol/schema/typescript/*.ts`
- `codex-rs/app-server-protocol/src/protocol/*.rs`
- `codex-rs/app-server/src/*.rs`
- `codex-rs/app-server/tests/suite/**/*.rs`

---

### Add or Update Plugin or Plugin Menu
**Trigger:** When adding, updating, or polishing plugin menus, plugin details, or plugin mention logic  
**Command:** `/update-plugin-menu`

1. Update plugin manager or plugin-related source files (`core/src/plugins/manager.rs`, `manager_tests.rs`, `manifest.rs`).
2. Update TUI or TUI app server plugin menu files (`tui/src/chatwidget/plugins.rs`, `tui_app_server/src/chatwidget/plugins.rs`).
3. Update or add tests and snapshots for plugin UI.
4. Update feature flags if necessary.
5. Commit with a message referencing plugin or menu changes.

**Files Involved:**
- `codex-rs/core/src/plugins/manager.rs`
- `codex-rs/core/src/plugins/manager_tests.rs`
- `codex-rs/core/src/plugins/manifest.rs`
- `codex-rs/tui/src/chatwidget/plugins.rs`
- `codex-rs/tui_app_server/src/chatwidget/plugins.rs`
- `codex-rs/tui/src/chatwidget/snapshots/*.snap`
- `codex-rs/tui_app_server/src/chatwidget/snapshots/*.snap`

---

## Testing Patterns

- **Framework:** Jest (for TypeScript/JavaScript portions)
- **File Pattern:** Test files are named using the pattern `*.test.ts`.
- **Rust Tests:** Place Rust tests in appropriate `tests/` directories or alongside modules using `#[cfg(test)]`.

**Example (TypeScript):**
```typescript
// app-server-protocol/schema/typescript/my_endpoint.test.ts
import { myEndpoint } from './my_endpoint';

test('should handle valid input', () => {
  expect(myEndpoint({ foo: 'bar' })).toBeTruthy();
});
```

**Example (Rust):**
```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_my_function() {
        assert_eq!(my_function(), 42);
    }
}
```

## Commands

| Command              | Purpose                                                        |
|----------------------|----------------------------------------------------------------|
| /extract-crate       | Extract a module or functionality into a new standalone crate  |
| /flip-feature-flag   | Enable or disable a feature flag                              |
| /remove-tool         | Remove a deprecated or unneeded tool or handler               |
| /add-api-endpoint    | Add or update an API endpoint or protocol method              |
| /update-plugin-menu  | Add or update plugin features or plugin menu UI               |
```
