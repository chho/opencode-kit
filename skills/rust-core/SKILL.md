---
name: rust-core
description: Rust language fundamentals - getting started, variables, data types, functions, control flow, structs, enums, and pattern matching. Load this for any general Rust development task or when learning Rust basics.
---

# Rust Core Fundamentals

> Based on *The Rust Programming Language* (Rust 1.90.0, Edition 2024)

## Getting Started

### Installation

```bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
rustc --version
rustup update
rustup doc
```

### Cargo — Build System & Package Manager

```bash
cargo new project_name     # create project
cargo build                # compile (creates executable in target/debug/)
cargo run                  # compile + run
cargo check                # check compilation without building binary (faster)
cargo build --release      # optimized build for production
```

Project structure:
```
project_name/
├── Cargo.toml
├── src/
│   └── main.rs
└── Cargo.lock
```

`Cargo.toml`:
```toml
[package]
name = "project_name"
version = "0.1.0"
edition = "2024"

[dependencies]
```

### Hello World

```rust
fn main() {
    println!("Hello, world!");
}
```

- `fn main()` — entry point of every Rust executable
- `println!` — macro (the `!` distinguishes it from a function)
- Lines end with semicolons `;`
- Compile with `rustc main.rs`, run with `./main`

## Variables and Mutability

```rust
let x = 5;              // immutable by default (cannot reassign)
let mut x = 5;          // mutable
x = 6;                  // OK

const MAX_POINTS: u32 = 100_000;  // constant: always immutable, must annotate type, global scope

let spaces = "   ";      // shadowing — reuse variable name
let spaces = spaces.len(); // can change type (unlike mut)
```

**`mut` vs Shadowing:** Shadowing lets you change the type while reusing the name. `mut` cannot change type.

## Data Types

Rust is **statically typed** — the compiler must know all types at compile time.

### Scalar Types

**Integers:**

| Length | Signed | Unsigned |
|--------|--------|----------|
| 8-bit  | `i8`   | `u8`     |
| 16-bit | `i16`  | `u16`    |
| 32-bit | `i32`  | `u32`    |
| 64-bit | `i64`  | `u64`    |
| 128-bit| `i128` | `u128`   |
| arch   | `isize`| `usize`  |

- Default: `i32`
- Literals: `98_222`, `0xff`, `0o77`, `0b1111_0000`, `b'A'` (byte)
- Integer overflow: panics in debug, wraps in release
- Safe overflow methods: `wrapping_*`, `checked_*`, `overflowing_*`, `saturating_*`

**Floating-point:** `f32`, `f64` (default). IEEE-754.

**Boolean:** `bool` — `true` or `false`, 1 byte.

**Character:** `char` — 4 bytes, Unicode scalar value, single quotes.
```rust
let c = 'z';
let z: char = 'ℤ';
let cat = '😻';
```

### Compound Types

**Tuples** — fixed length, mixed types:
```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (x, y, z) = tup;       // destructuring
let five_hundred = tup.0;   // index access
let unit = ();               // unit type — empty tuple
```

**Arrays** — fixed length, same type, stack-allocated:
```rust
let a = [1, 2, 3, 4, 5];
let a: [i32; 5] = [1, 2, 3, 4, 5];
let a = [3; 5];              // [3, 3, 3, 3, 3]
let first = a[0];
```

Invalid array access causes a **panic** at runtime (bounds checking).

## Functions

```rust
fn another_function(x: i32) {
    println!("The value of x is: {x}");
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

- Parameter types must be declared explicitly
- Snake case convention for function/variable names
- Definition order doesn't matter

**Statements vs Expressions:**
- **Statements** perform an action, don't return a value (`let x = 5;`)
- **Expressions** evaluate to a value (`5 + 6`, function calls, blocks)
- Adding `;` turns an expression into a statement (returns `()`)

**Return values:**
```rust
fn five() -> i32 {
    5                        // no semicolon = expression = return value
}

fn plus_one(x: i32) -> i32 {
    x + 1                    // implicit return (no semicolon!)
}
```

## Control Flow

### `if` Expressions

```rust
let number = 3;
if number < 5 {
    println!("condition was true");
} else {
    println!("condition was false");
}

// else if chain
if number % 4 == 0 {
    println!("divisible by 4");
} else if number % 3 == 0 {
    println!("divisible by 3");
} else {
    println!("not divisible by 4, 3, or 2");
}

// if is an expression — can assign to variable
let condition = true;
let number = if condition { 5 } else { 6 };
// Both arms must have the same type!
```

Condition **must be `bool`** (no implicit conversion from integers).

### Loops

**`loop`** — infinite loop:
```rust
loop {
    println!("again!");
    break;        // exit loop
    continue;     // skip to next iteration
}

// Return value from loop
let result = loop {
    counter += 1;
    if counter == 10 {
        break counter * 2;     // returns 20
    }
};

// Loop labels (for nested loops)
'counting_up: loop {
    loop {
        break;                    // breaks inner loop
        break 'counting_up;      // breaks outer loop
    }
}
```

**`while`** — conditional loop:
```rust
let mut number = 3;
while number != 0 {
    println!("{number}!");
    number -= 1;
}
```

**`for`** — iterate over collections (most commonly used):
```rust
let a = [10, 20, 30, 40, 50];
for element in a {
    println!("the value is: {element}");
}

for number in (1..4).rev() {
    println!("{number}!");
}
```

`for` is safer and more efficient than `while` with manual indexing. Range: `1..4` produces `1, 2, 3`.

## Structs

### Definition and Instantiation

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

let mut user1 = User {
    active: true,
    username: String::from("someusername123"),
    email: String::from("someone@example.com"),
    sign_in_count: 1,
};

user1.email = String::from("another@example.com"); // requires mut on entire instance
```

**Field Init Shorthand:**
```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

**Struct Update Syntax:**
```rust
let user2 = User {
    email: String::from("another@example.com"),
    ..user1  // rest of fields from user1
};
// Note: if username (a String) is moved, user1 is no longer valid
```

**Tuple Structs:**
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
// Color and Point are different types!
// Access: black.0, origin.1
// Destructure: let Point(x, y, z) = origin;
```

**Unit-Like Structs** — no fields, useful for implementing traits:
```rust
struct AlwaysEqual;
```

### Debug Trait

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

println!("rect1 is {rect1:?}");       // Debug format
println!("rect1 is {rect1:#?}");      // Pretty Debug format
dbg!(&rect1);                          // prints to stderr with file:line info
```

### Methods

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {          // &self = immutable borrow
        self.width * self.height
    }

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }

    fn square(size: u32) -> Self {   // associated function (no self)
        Self {
            width: size,
            height: size,
        }
    }
}

let sq = Rectangle::square(3); // associated function call with ::
```

Key points:
- `&self` = borrows instance immutably, `&mut self` = borrows mutably, `self` = takes ownership
- **Automatic referencing/dereferencing**: Rust automatically adds `&`, `&mut`, or `*` to match method signature
- Method names can shadow field names (getters pattern)
- Multiple `impl` blocks are allowed

## Enums and Pattern Matching

### Defining Enums

```rust
enum IpAddrKind {
    V4,
    V6,
}

let four = IpAddrKind::V4;
```

**Enums with data:**
```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

enum Message {
    Quit,                       // no data
    Move { x: i32, y: i32 },   // named fields
    Write(String),              // single value
    ChangeColor(i32, i32, i32), // tuple
}
```

**Methods on Enums:**
```rust
impl Message {
    fn call(&self) {
        // method body
    }
}
```

### The Option Enum

Rust has **no null**. Uses `Option<T>` to encode absence:
```rust
enum Option<T> {
    None,
    Some(T),
}
```

- `Option<T>` and `Some`/`None` are in the **prelude** (no import needed)
- `Some(5)` has type `Option<i32>`
- **You cannot use `Option<T>` as if it were `T`** — the compiler enforces handling both cases

### The `match` Control Flow

```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {  // binding values
            println!("State quarter from {state:?}!");
            25
        }
    }
}

// Matching Option<T>
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}
```

**Matches are exhaustive** — you must cover every possible case. Use `_` as catch-all:
```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => (),               // do nothing
}
```

### `if let` and `let...else`

```rust
// if let — syntax sugar for matching one pattern
if let Some(max) = config_max {
    println!("The maximum is {max}");
}

// With else
if let Coin::Quarter(state) = coin {
    println!("State quarter from {state:?}!");
} else {
    count += 1;
}

// let...else — happy path pattern
fn describe_state_quarter(coin: Coin) -> Option<String> {
    let Coin::Quarter(state) = coin else {
        return None;
    };
    Some(format!("{state:?} is pretty old!"))
}
```

**Trade-off:** `if let` is more concise but loses exhaustive checking.

## Modules, Packages, and Crates

### Key Concepts

| Concept | Definition |
|---------|-----------|
| **Package** | Bundle of one or more crates, described by `Cargo.toml` |
| **Crate** | Smallest amount of code the Rust compiler considers at a time |
| **Module** | Organizes code within a crate, controls privacy |

- `src/main.rs` → binary crate root
- `src/lib.rs` → library crate root
- A package can contain as many binary crates as you like (in `src/bin/`) but at most one library crate

### Module Rules

1. Start from crate root (`src/lib.rs` or `src/main.rs`)
2. `mod garden;` tells compiler to look for code in `src/garden.rs` or `src/garden/mod.rs`
3. Code in modules is **private by default** to parent modules
4. `pub` makes modules and items public

### Paths

```rust
crate::front_of_house::hosting::add_to_waitlist();  // absolute
front_of_house::hosting::add_to_waitlist();          // relative
super::deliver_order();                               // parent module (like ../)
```

### `use` Keyword

```rust
use crate::front_of_house::hosting;       // bring parent module (idiomatic for functions)
use std::collections::HashMap;            // bring full path (idiomatic for types)
use std::fmt::Result;
use std::io::Result as IoResult;          // rename with as
pub use crate::front_of_house::hosting;   // re-export
use std::{cmp::Ordering, io};             // nested paths
use std::io::{self, Write};               // self refers to std::io
use std::collections::*;                  // glob (use sparingly)
```

**Privacy rules:**
- All items are private by default
- `pub` on a struct doesn't make fields public — each field must be individually marked `pub`
- `pub` on an enum makes all variants public automatically

### Separating Modules into Files

```
src/
├── lib.rs                     # mod front_of_house;
├── front_of_house.rs          # pub mod hosting;
└── front_of_house/
    └── hosting.rs             # pub fn add_to_waitlist() {}
```

`mod` declares a module once in the module tree — it's not an "include". Other files refer to modules via paths.
