---
name: rust-collections-error-handling
description: Rust standard collections (Vec, String, HashMap) and error handling (panic!, Result, unwrap, expect, ? operator). Load this when working with collections, string operations, or handling errors in Rust.
---

# Rust Collections & Error Handling

> Based on *The Rust Programming Language* (Rust 1.90.0, Edition 2024)

## Vectors (`Vec<T>`)

### Creating

```rust
let v: Vec<i32> = Vec::new();   // empty, explicit type
let v = vec![1, 2, 3];          // macro with initial values (infers Vec<i32>)
```

### Updating

```rust
let mut v = Vec::new();
v.push(5);
v.push(6);
```

### Reading Elements

```rust
let v = vec![1, 2, 3, 4, 5];

let third: &i32 = &v[2];                    // panics on out-of-bounds
let third: Option<&i32> = v.get(2);         // returns None on out-of-bounds
```

**Key rule:** Cannot hold a mutable reference and push at the same time (borrow checker prevents dangling references if vector reallocates).

### Iterating

```rust
// Immutable
let v = vec![100, 32, 57];
for i in &v {
    println!("{i}");
}

// Mutable
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;      // dereference to modify
}
```

### Storing Multiple Types with Enums

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}
let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

Vectors are freed when they go out of scope; all elements are dropped with them.

## Strings

### Types

- **`str`** — core language string slice (borrowed form: `&str`)
- **`String`** — growable, owned, UTF-8 encoded (standard library, wrapper around `Vec<u8>`)

### Creating

```rust
let mut s = String::new();
let s = "initial contents".to_string();
let s = String::from("initial contents");
let s = String::from("こんにちは");   // UTF-8 supported
```

### Updating

```rust
// push_str — appends a &str
let mut s = String::from("foo");
s.push_str("bar");       // s = "foobar"

// push — appends a single char
let mut s = String::from("lo");
s.push('l');             // s = "lol"
```

### Concatenation

```rust
// + operator (takes ownership of s1, borrows s2)
let s3 = s1 + &s2;

// format! macro (borrows all, doesn't take ownership)
let s = format!("{s1}-{s2}-{s3}");
```

### Indexing — NOT supported

```rust
let h = s1[0];    // ERROR: Rust strings don't support indexing
```

**Why?** A `String` is `Vec<u8>`. UTF-8 characters vary in byte length. Indexing by byte would return invalid data at character boundaries.

### String Slicing (with ranges)

```rust
let hello = "Здравствуйте";
let s = &hello[0..4];   // first 4 bytes = "Зд" (2 chars × 2 bytes)
// &hello[0..1] would PANIC — splits a character
```

### Iterating

```rust
// By Unicode scalar values (char)
for c in "Зд".chars() { println!("{c}"); }   // З, д

// By raw bytes
for b in "Зд".bytes() { println!("{b}"); }    // 208, 151, 208, 180

// Grapheme clusters — not in stdlib; use external crates (e.g., unicode-segmentation)
```

### Three Views of Strings

1. **Bytes** — raw `u8` values
2. **Scalar values** — `char` type (may include non-letter diacritics)
3. **Grapheme clusters** — closest to human "letters" (requires external crate)

## HashMap<K, V>

### Creating & Inserting

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

### Accessing

```rust
let score = scores.get(&team_name).copied().unwrap_or(0);
// get returns Option<&V>, copied -> Option<V>, unwrap_or -> V
```

### Iterating

```rust
for (key, value) in &scores {
    println!("{key}: {value}");
}
```

### Ownership

- `Copy` types (like `i32`) are copied into the map
- Owned types (like `String`) are **moved** — original variables become invalid

### Updating

**Overwriting:**
```rust
scores.insert(String::from("Blue"), 25);   // replaces old value
```

**Only insert if key absent (entry API):**
```rust
scores.entry(String::from("Yellow")).or_insert(50);
```

**Update based on old value (word count example):**
```rust
let text = "hello world wonderful world";
let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}
// {"world": 2, "hello": 1, "wonderful": 1}
```

Default hashing: **SipHash** — resistant to DoS attacks. Can switch hasher by implementing `BuildHasher`.

## Error Handling

### `panic!` — Unrecoverable Errors

```rust
panic!("crash and burn");       // explicit
let v = vec![1, 2, 3];
v[99];                          // implicit — index out of bounds panics
```

**Unwinding vs Aborting:**
- **Default:** unwinds the stack (cleans up)
- **Abort mode** (smaller binary): `Cargo.toml` → `[profile.release] panic = 'abort'`

**Backtraces:**
```bash
RUST_BACKTRACE=1 cargo run
```

### `Result<T, E>` — Recoverable Errors

```rust
enum Result<T, E> {
    Ok(T),    // success value
    Err(E),   // error value
}
```

**Handling with `match`:**
```rust
let greeting_file = match File::open("hello.txt") {
    Ok(file) => file,
    Err(error) => panic!("Problem opening the file: {error:?}"),
};
```

**Matching on different error kinds:**
```rust
use std::io::ErrorKind;

let greeting_file = match File::open("hello.txt") {
    Ok(file) => file,
    Err(error) => match error.kind() {
        ErrorKind::NotFound => match File::create("hello.txt") {
            Ok(fc) => fc,
            Err(e) => panic!("Problem creating the file: {e:?}"),
        },
        _ => panic!("Problem opening the file: {error:?}"),
    },
};
```

### `unwrap()` and `expect()`

```rust
// unwrap: returns Ok value, panics on Err with default message
let greeting_file = File::open("hello.txt").unwrap();

// expect: same as unwrap but with custom message (preferred in production)
let greeting_file = File::open("hello.txt")
    .expect("hello.txt should be included in this project");
```

### The `?` Operator — Error Propagation

```rust
// Manual propagation (verbose)
fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}

// Chaining with ?
fn read_username_from_file() -> Result<String, io::Error> {
    let mut username = String::new();
    File::open("hello.txt")?.read_to_string(&mut username)?;
    Ok(username)
}
```

**How `?` works:**
- If `Ok(v)` → evaluates to `v`, continues
- If `Err(e)` → returns `Err(e)` from the function early
- Also calls `From::from()` to convert error types automatically

**Where `?` can be used:**
- Only in functions returning `Result` or `Option` (or any type implementing `FromResidual`)
- Can use `?` on `Result` in `Result`-returning functions
- Can use `?` on `Option` in `Option`-returning functions
- **Cannot mix** — no auto-conversion between `Result` and `Option`

**`?` on `Option<T>`:**
```rust
fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines().next()?.chars().last()
}
```

**`main` returning `Result`:**
```rust
fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;
    Ok(())
}
```

### When to `panic!` vs Return `Result`

**When to `panic!`:**
- Bad state is unexpected, not a normal occurrence
- Code after that point relies on not being in a bad state
- No good way to encode the invariant in the type system
- Examples, prototypes, and tests (using `unwrap`/`expect`)
- You know more than the compiler (e.g., hardcoded valid IP)

**When to return `Result`:**
- Failure is expected (parsing, I/O, network requests)
- Calling code should decide how to handle the error

**Custom types for validation:**
```rust
pub struct Guess {
    value: i32,
}
impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {value}.");
        }
        Guess { value }
    }
    pub fn value(&self) -> i32 { self.value }
}
```
Using the type system guarantees validity at compile time — functions accepting `Guess` never need runtime checks.
