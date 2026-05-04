---
name: rust-project-cargo
description: Rust project management with Cargo - project setup, dependencies, testing (unit and integration), cargo commands, release profiles, publishing crates, workspaces, and Cargo configuration. Load this when setting up Rust projects, writing tests, managing dependencies, or publishing crates.
---

# Rust Project Management with Cargo & Testing

> Based on *The Rust Programming Language* (Rust 1.90.0, Edition 2024)

## Cargo Commands

```bash
cargo new project_name     # create new project
cargo init                 # init project in existing directory
cargo build                # compile (debug mode)
cargo build --release      # compile (release mode, optimizations)
cargo run                  # compile + run
cargo check                # check compilation without building (faster)
cargo test                 # run all tests
cargo test test_name       # run tests matching name
cargo doc --open           # build and open documentation
cargo clean                # remove build artifacts
cargo update               # update dependencies (respects Cargo.lock)
```

## Project Structure

```
project_name/
├── Cargo.toml              # project manifest
├── Cargo.lock              # auto-generated dependency lock
├── src/
│   ├── main.rs             # binary crate root
│   ├── lib.rs              # library crate root (optional)
│   └── bin/                # additional binary crates
│       └── extra_bin.rs
├── tests/                  # integration tests
│   └── integration_test.rs
├── examples/               # examples
│   └── example.rs
└── benches/                # benchmarks
    └── bench.rs
```

## Cargo.toml

```toml
[package]
name = "my_project"
version = "0.1.0"
edition = "2024"
authors = ["Your Name <you@example.com>"]
description = "A brief description"
license = "MIT"

[dependencies]
serde = { version = "1.0", features = ["derive"] }
rand = "0.8.5"
anyhow = "1.0"

[dev-dependencies]
# only used in tests/examples
criterion = "0.5"

[profile.release]
opt-level = 3
panic = 'abort'
lto = true
```

## Customizing Builds with Release Profiles

```toml
[profile.dev]
opt-level = 0        # no optimization (fast compile)

[profile.release]
opt-level = 3        # max optimization
```

## Writing Automated Tests

### Basic Test Structure

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        let result = add(2, 2);
        assert_eq!(result, 4);
    }
}
```

### Assert Macros

```rust
assert!(condition);                         // passes if true
assert_eq!(left, right);                    // equality check (needs PartialEq + Debug)
assert_ne!(left, right);                    // inequality check

// Custom failure messages
assert!(
    result.contains("Carol"),
    "Greeting did not contain name, value was `{result}`"
);
```

### Testing for Panics

```rust
#[test]
#[should_panic]
fn greater_than_100() {
    Guess::new(200);
}

// With expected message
#[test]
#[should_panic(expected = "less than or equal to 100")]
fn greater_than_100() {
    Guess::new(200);
}
```

### Using `Result<T, E>` in Tests

```rust
#[test]
fn it_works() -> Result<(), String> {
    let result = add(2, 2);
    if result == 4 {
        Ok(())
    } else {
        Err(String::from("two plus two does not equal four"))
    }
}
```

Enables use of the `?` operator. Cannot use `#[should_panic]` with `Result<T, E>` tests — use `assert!(value.is_err())` instead.

### Controlling Test Execution

```bash
cargo test                              # run all tests (parallel by default)
cargo test -- --test-threads=1          # run serially
cargo test -- --show-output             # print stdout from passing tests
cargo test test_name                    # run tests matching filter
cargo test -- --ignored                 # run only ignored tests
cargo test -- --include-ignored         # run all tests including ignored
```

### Ignoring Tests

```rust
#[test]
#[ignore]
fn expensive_test() { ... }
```

## Test Organization

### Unit Tests

Placed in `src/` in a `#[cfg(test)]` module. Can test private functions:

```rust
// src/lib.rs
pub fn add(left: u64, right: u64) -> u64 {
    left + right
}

fn private_helper() -> u64 { 42 }

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn it_works() {
        let result = add(2, 2);
        assert_eq!(result, 4);
    }

    #[test]
    fn test_private() {
        assert_eq!(private_helper(), 42);  // can access private items
    }
}
```

### Integration Tests

Placed in a top-level `tests/` directory. Each file is compiled as a separate crate. Only the public API is accessible:

```
adder/
├── src/
│   └── lib.rs
└── tests/
    └── integration_test.rs
```

```rust
// tests/integration_test.rs
use adder::add;

#[test]
fn it_adds_two() {
    assert_eq!(add(2, 2), 4);
}
```

**Run specific integration test file:**
```bash
cargo test --test integration_test
```

**Shared helpers** — use `tests/common/mod.rs` (not `tests/common.rs`) to avoid it appearing as a test section:
```rust
// tests/common/mod.rs
pub fn setup() { /* shared setup code */ }

// tests/integration_test.rs
mod common;

#[test]
fn it_works() {
    common::setup();
    // test code
}
```

## Publishing a Crate

### Documentation Comments

```rust
/// Adds two numbers together.
///
/// # Examples
///
/// ```
/// let result = my_crate::add(2, 3);
/// assert_eq!(result, 5);
/// ```
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

//! # My Crate
//!
//! `my_crate` is a collection of utilities for doing things.
```

Documentation generates HTML from markdown. Common sections: `# Examples`, `# Panics`, `# Errors`, `# Safety`.

### Publishing

```bash
cargo login                # login with API token
cargo publish              # publish to crates.io
cargo yank --vers 1.0.1   # yank a version (prevents new uses)
```

### Cargo Workspaces

Multiple related packages in one repository:

```toml
# Cargo.toml (workspace root)
[workspace]
members = [
    "adder",
    "add_one",
]
```

```
add/
├── Cargo.toml         # workspace root
├── adder/
│   ├── Cargo.toml
│   └── src/
│       └── main.rs
└── add_one/
    ├── Cargo.toml
    └── src/
        └── lib.rs
```

Workspace shares a single `Cargo.lock` and `target/` directory.

**Depend on workspace member:**
```toml
# adder/Cargo.toml
[dependencies]
add_one = { path = "../add_one" }
```

## Installing Binaries

```bash
cargo install ripgrep    # install binary crate from crates.io
cargo install --path .   # install local binary
```

## Extending Cargo with Custom Commands

Any executable named `cargo-something` on `$PATH` becomes a Cargo subcommand:

```bash
cargo expand    # runs cargo-expand if installed
cargo audit     # runs cargo-audit
```

## Useful Development Tools

- **`cargo fmt`** — format code with `rustfmt`
- **`cargo clippy`** — lint code with Clippy
- **`cargo doc`** — generate documentation
- **`cargo fix`** — automatically fix lint warnings
- **`rust-analyzer`** — IDE support (LSP server)
