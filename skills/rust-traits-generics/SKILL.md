---
name: rust-traits-generics
description: Rust generics, traits, trait bounds, lifetimes, and polymorphism - generic functions/structs/enums, trait definitions and implementations, impl Trait, where clauses, associated types, operator overloading, and lifetime annotations. Load this when designing abstractions, implementing traits, or working with generic code in Rust.
---

# Rust Generics, Traits, and Lifetimes

> Based on *The Rust Programming Language* (Rust 1.90.0, Edition 2024)

## Generic Data Types

### Generic Functions

```rust
fn largest<T: PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];
    for item in list {
        if item > largest {
            largest = item;
        }
    }
    largest
}
```

### Generic Structs

```rust
struct Point<T> {
    x: T,
    y: T,
}

struct Point<T, U> {
    x: T,
    y: U,
}
```

### Generic Enums

```rust
enum Option<T> {
    Some(T),
    None,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

### Generic Methods

Declare `T` after `impl` so it applies to all `Point<T>`. Can also implement methods only for specific concrete types:

```rust
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}
```

Methods can have their own generic parameters independent of the struct's:
```rust
impl<X1, Y1> Point<X1, Y1> {
    fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
        Point { x: self.x, y: other.y }
    }
}
```

**Monomorphization:** Rust compiles generic code into specific code for each concrete type at compile time. There is **zero runtime cost** for using generics.

## Traits

### Defining a Trait

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

### Implementing a Trait on a Type

```rust
pub struct NewsArticle {
    pub headline: String,
    pub author: String,
    pub location: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}
```

**Orphan Rule:** You can implement a trait on a type only if either the trait or the type (or both) are local to your crate.

### Default Implementations

```rust
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}
```

Default implementations can call other trait methods. You **cannot** call the default implementation from an overriding implementation.

### Traits as Parameters

```rust
// impl Trait syntax (allows different types for each param)
pub fn notify(item: &impl Summary) { ... }

// Trait bound syntax (forces same type for multiple params)
pub fn notify<T: Summary>(item: &T) { ... }

// Multiple trait bounds with +
pub fn notify(item: &(impl Summary + Display)) { ... }
pub fn notify<T: Summary + Display>(item: &T) { ... }
```

### `where` Clauses

Cleaner syntax for complex bounds:
```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{ ... }
```

### Returning `impl Trait`

```rust
fn returns_summarizable() -> impl Summary {
    SocialPost { ... }
}
```

**Limitation:** Can only return a **single** type (not conditionally different types). Use trait objects (`Box<dyn Trait>`) for dynamic dispatch.

### Conditional Method Implementation

```rust
impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) { ... }
}
```

### Blanket Implementations

Implement a trait for any type satisfying a bound:
```rust
impl<T: Display> ToString for T { ... }
```

## Lifetimes

Lifetimes ensure references are valid as long as needed. Every reference has a lifetime, mostly inferred.

### Lifetime Annotation Syntax

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

### Lifetimes in Structs

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}
```

### Lifetime Elision Rules

1. **Each reference parameter gets its own lifetime**
2. **If exactly one input lifetime, it's assigned to all outputs**
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

## Advanced Traits

### Associated Types

One implementation per type:
```rust
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}

impl Iterator for Counter {
    type Item = u32;
    fn next(&mut self) -> Option<u32> { /* ... */ }
}
```

### Default Generic Parameters & Operator Overloading

```rust
use std::ops::Add;

#[derive(Debug, Copy, Clone)]
struct Point { x: i32, y: i32 }

impl Add for Point {
    type Output = Point;
    fn add(self, other: Point) -> Point {
        Point { x: self.x + other.x, y: self.y + other.y }
    }
}

// Custom Rhs type:
impl Add<Meters> for Millimeters {
    type Output = Millimeters;
    fn add(self, other: Meters) -> Millimeters {
        Millimeters(self.0 + (other.0 * 1000))
    }
}
```

### Fully Qualified Syntax (Disambiguating Methods)

```rust
Pilot::fly(&person);
Wizard::fly(&person);
<Dog as Animal>::baby_name();
```

### Supertraits

```rust
trait OutlinePrint: fmt::Display {
    fn outline_print(&self) {
        let output = self.to_string();
        // ...
    }
}
```

### Newtype Pattern

Implementing external traits on external types:
```rust
struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}
```
