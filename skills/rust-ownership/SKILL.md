---
name: rust-ownership
description: Rust ownership system - ownership rules, borrowing, references, mutable/immutable references, slice types, and lifetime annotations. Load this when dealing with borrow checker errors, ownership transfer, references, or lifetimes.
---

# Rust Ownership, Borrowing, and Lifetimes

> Based on *The Rust Programming Language* (Rust 1.90.0, Edition 2024)

## Ownership Rules

1. Each value in Rust has an **owner**.
2. There can only be **one owner** at a time.
3. When the owner goes out of scope, the value will be **dropped**.

Ownership exists primarily to manage **heap data**.

### Stack vs Heap

- **Stack**: LIFO, fast push/pop. Stores values with known, fixed size.
- **Heap**: Less organized. Allocator finds space, returns a pointer. Slower access (must follow pointer).

### Move Semantics

Assigning a `String` **moves** it (no deep copy):
```rust
let s1 = String::from("hello");
let s2 = s1;
// s1 is NO LONGER VALID — ownership moved to s2
// println!("{s1}"); // ERROR: borrow of moved value
```

This prevents **double free** errors.

### Clone — Deep Copy

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
// Both s1 and s2 are valid
```

### Copy Trait — Stack-Only Data

Types like `i32`, `bool`, `f64`, `char`, tuples (of Copy types) are copied instead of moved:
```rust
let x = 5;
let y = x;
// Both x and y are valid — integers implement Copy
```

### Ownership and Functions

Passing a value into a function moves or copies it:
```rust
fn takes_ownership(s: String) { /* s dropped at end */ }
fn makes_copy(x: i32) { /* x copied, original still valid */ }

fn main() {
    let s = String::from("hello");
    takes_ownership(s);
    // s is no longer valid here

    let x = 5;
    makes_copy(x);
    // x is still valid here
}
```

### Return Values and Scope

Returning a value transfers ownership:
```rust
fn gives_ownership() -> String {
    String::from("yours")  // ownership moves to the caller
}
```

Using tuples to return multiple values with ownership transfer is cumbersome — use **references** instead.

## References and Borrowing

### Immutable References

Borrow without taking ownership:
```rust
fn calculate_length(s: &String) -> usize {
    s.len()
} // s goes out of scope, but since it doesn't have ownership, nothing is dropped

fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);
    // s1 is still valid here
    println!("The length of '{s1}' is {len}.");
}
```

The `&` creates a reference. References are immutable by default — you **cannot modify** something borrowed immutably.

### Mutable References

```rust
fn main() {
    let mut s = String::from("hello");
    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

### Borrowing Rules

1. At any given time, you can have **either** one mutable reference **or** any number of immutable references.
2. References must always be valid (no dangling references).

```rust
// ERROR: two mutable references at the same time
let r1 = &mut s;
let r2 = &mut s; // ERROR

// ERROR: cannot have mutable ref while immutable ref exists
let r1 = &s;
let r2 = &s;
let r3 = &mut s; // ERROR
```

**Non-overlapping scopes are OK:**
```rust
let r1 = &s;
let r2 = &s;
println!("{r1} and {r2}");
// r1 and r2 are no longer used after this point (NLL — Non-Lexical Lifetimes)

let r3 = &mut s; // OK — no overlap with r1/r2
println!("{r3}");
```

### Dangling References

Rust prevents them at compile time:
```rust
fn dangle() -> &String { // ERROR: missing lifetime specifier
    let s = String::from("hello");
    &s // returning reference to local variable — would be dangling!
} // s is dropped here

// Correct: return owned String instead
fn no_dangle() -> String {
    String::from("hello")
}
```

## The Slice Type

Slices reference a contiguous sequence of elements **without taking ownership**.

### String Slices (`&str`)

```rust
let s = String::from("hello world");
let hello = &s[0..5];   // "hello"
let world = &s[6..11];  // "world"

// Range sugar:
let slice = &s[..2];    // from beginning
let slice = &s[3..];    // to end
let slice = &s[..];     // entire string
```

**First word function with slices:**
```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    &s[..]
}
```

Best practice: use `&str` instead of `&String` as parameter — works with both `String` and string literals.

**String literals are slices**: `let s = "Hello, world!";` — type is `&str`, pointing into the binary.

### Array Slices

```rust
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3]; // type: &[i32], values [2, 3]
```

## Lifetimes

Lifetimes ensure references are valid as long as needed. Every reference has a lifetime, mostly inferred.

### Lifetime Annotation Syntax

Starts with apostrophe, usually `'a`:
```rust
&i32          // reference
&'a i32       // reference with explicit lifetime
&'a mut i32   // mutable reference with explicit lifetime
```

### Generic Lifetimes in Functions

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

The returned reference's lifetime is the **smaller** of the input lifetimes.

When the return value only depends on one parameter:
```rust
fn longest<'a>(x: &'a str, y: &str) -> &'a str {
    x  // only x's lifetime matters
}
```

### Lifetimes in Structs

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().unwrap();
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```

### Lifetime Elision Rules

The compiler uses 3 rules to infer lifetimes:

1. **Each reference parameter gets its own lifetime**: `fn foo<'a>(x: &'a i32)`
2. **If exactly one input lifetime, it's assigned to all outputs**: `fn foo<'a>(x: &'a i32) -> &'a i32`
3. **If multiple input lifetimes but one is `&self` or `&mut self`, `self`'s lifetime is assigned to all outputs**

### The Static Lifetime `'static`

```rust
let s: &'static str = "I have a static lifetime.";
```

All string literals have `'static`. Don't use `'static` to silence errors without understanding the real issue.

### Combining Generics, Trait Bounds, and Lifetimes

```rust
use std::fmt::Display;

fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {ann}");
    if x.len() > y.len() { x } else { y }
}
```
