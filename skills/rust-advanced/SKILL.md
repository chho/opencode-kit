---
name: rust-advanced
description: Rust advanced features - smart pointers (Box, Rc, RefCell, Weak), closures, iterators, Fn traits, unsafe Rust, advanced traits, macros (declarative and procedural), and advanced types. Load this when working with complex Rust patterns, FFI, macros, or unsafe code.
---

# Rust Advanced Features

> Based on *The Rust Programming Language* (Rust 1.90.0, Edition 2024)

## Closures

Closures are anonymous functions that **capture values from their environment**.

### Syntax

```rust
// Fully annotated
let expensive_closure = |num: u32| -> u32 {
    println!("calculating slowly...");
    num
};

// All equivalent forms:
fn  add_one_v1   (x: u32) -> u32 { x + 1 }
let add_one_v2 = |x: u32| -> u32 { x + 1 };
let add_one_v3 = |x|             { x + 1 };
let add_one_v4 = |x|               x + 1  ;
```

### Capturing the Environment

1. **Immutable borrow** — closure only reads: `|| println!("{:?}", list)`
2. **Mutable borrow** — closure modifies: `|| list.push(7)`
3. **Take ownership** — with `move` keyword: `move || println!("{:?}", list)`

```rust
// move closure for threads
thread::spawn(move || println!("From thread: {list:?}"))
```

Type inference: Closure types are inferred but locked on first use. You cannot call a closure with different types.

### Fn Traits

All closures implement at least `FnOnce`. Traits are additive:

| Trait | Behavior | Callable |
|-------|----------|----------|
| `FnOnce` | Moves captured values out of closure body | Once |
| `FnMut` | Mutates captured values but doesn't move them | Multiple times |
| `Fn` | Neither moves nor mutates captured values | Multiple times, safely concurrent |

```rust
// unwrap_or_else uses FnOnce — called at most once
pub fn unwrap_or_else<F>(self, f: F) -> T
where
    F: FnOnce() -> T

// sort_by_key uses FnMut — called for each item
fn sort_by_key<K, F>(&mut self, f: F)
where
    F: FnMut(&T) -> K
```

## Iterators

Iterators are **lazy** — they have no effect until consumed.

### The Iterator Trait

```rust
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}
```

### Three Kinds of Iteration

- `iter()` — borrows each element (`&T`)
- `iter_mut()` — mutably borrows (`&mut T`)
- `into_iter()` — takes ownership (`T`)

### Consuming Adapters

Use up the iterator:
```rust
let total: i32 = v1.iter().sum();
```

### Iterator Adapters

Produce new iterators, lazy:
```rust
let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();
```

`collect()` consumes the iterator and gathers results into a collection.

### `filter()` with Closures

```rust
fn shoes_in_size(shoes: Vec<Shoe>, shoe_size: u32) -> Vec<Shoe> {
    shoes.into_iter().filter(|s| s.size == shoe_size).collect()
}
```

## Smart Pointers

### `Box<T>` — Heap Allocation

```rust
let b = Box::new(5);
println!("b = {b}");
```

Use cases:
- Types with unknown size at compile time (recursive types)
- Large data transfers without copying
- Trait objects

**Enabling recursive types:**
```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}

let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
```

### `Deref` Trait — Dereference Coercion

```rust
use std::ops::Deref;

impl<T> Deref for MyBox<T> {
    type Target = T;
    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```

**Deref coercion** converts `&T` → `&U` automatically when `T: Deref<Target=U>`.

Three cases:
1. `&T` → `&U` when `T: Deref<Target=U>`
2. `&mut T` → `&mut U` when `T: DerefMut<Target=U>`
3. `&mut T` → `&U` when `T: Deref<Target=U>` (mutable → immutable OK; reverse is NOT)

### `Drop` Trait — Cleanup

```rust
impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping with data `{}`!", self.data);
    }
}

// Force early drop:
drop(c);  // std::mem::drop, NOT c.drop()
```

Variables are dropped in **reverse order** of creation. You **cannot** call `.drop()` manually.

### `Rc<T>` — Reference Counting (Single-Threaded)

Enables **multiple ownership**. `Rc::clone` only increments the reference count:

```rust
use std::rc::Rc;

enum List {
    Cons(i32, Rc<List>),
    Nil,
}

let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
let b = Cons(3, Rc::clone(&a));  // ref count = 2
let c = Cons(4, Rc::clone(&a));  // ref count = 3
```

Use `Rc::strong_count(&a)` to inspect the count. `Rc<T>` only allows **immutable** access. **Single-threaded only**.

### `RefCell<T>` — Interior Mutability (Single-Threaded)

Enforces borrowing rules at **runtime** instead of compile time:

```rust
use std::cell::RefCell;

struct MockMessenger {
    sent_messages: RefCell<Vec<String>>,
}

impl Messenger for MockMessenger {
    fn send(&self, message: &str) {
        self.sent_messages.borrow_mut().push(String::from(message));
    }
}
```

- `borrow()` — returns `Ref<T>` (immutable), like `&T`
- `borrow_mut()` — returns `RefMut<T>` (mutable), like `&mut T`
- Violating borrowing rules at runtime causes a **panic**

**Comparison table:**

| Smart Pointer | Owners | Borrow checking | Mutability |
|---|---|---|---|
| `Box<T>` | Single | Compile time | Immutable or mutable |
| `Rc<T>` | Multiple | Compile time | Immutable only |
| `RefCell<T>` | Single | Runtime | Immutable or mutable |

**Combining `Rc<RefCell<T>>`** for multiple owners + mutability:
```rust
let value = Rc::new(RefCell::new(5));
let a = Rc::new(Cons(Rc::clone(&value), Rc::new(Nil)));
*value.borrow_mut() += 10;
```

### `Weak<T>` — Preventing Reference Cycles

`Rc::downgrade` creates a `Weak<T>` that doesn't affect `strong_count`:

```rust
use std::rc::{Rc, Weak};
use std::cell::RefCell;

struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,        // weak reference (non-owning)
    children: RefCell<Vec<Rc<Node>>>,   // strong references (owning)
}

let branch = Rc::new(Node {
    value: 5,
    parent: RefCell::new(Weak::new()),
    children: RefCell::new(vec![]),
});
*leaf.parent.borrow_mut() = Rc::downgrade(&branch);
```

Use `weak.upgrade()` → `Option<Rc<T>>` to access the value (returns `None` if dropped).

## Unsafe Rust

Five **unsafe superpowers** (inside `unsafe {}` blocks):

1. **Dereference raw pointers** (`*const T`, `*mut T`)
2. **Call unsafe functions/methods**
3. **Access/modify mutable static variables**
4. **Implement unsafe traits**
5. **Access fields of unions**

```rust
let mut num = 5;
let r1 = &raw const num;    // *const i32
let r2 = &raw mut num;      // *mut i32

unsafe {
    println!("r1 is: {}", *r1);
    println!("r2 is: {}", *r2);
}
```

### Unsafe Functions

```rust
unsafe fn dangerous() {}

unsafe {
    dangerous();
}
```

### Safe Abstraction over Unsafe Code

```rust
use std::slice;

fn split_at_mut(values: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = values.len();
    let ptr = values.as_mut_ptr();
    assert!(mid <= len);
    unsafe {
        (
            slice::from_raw_parts_mut(ptr, mid),
            slice::from_raw_parts_mut(ptr.add(mid), len - mid),
        )
    }
}
```

### FFI with `extern`

```rust
unsafe extern "C" {
    fn abs(input: i32) -> i32;
}

fn main() {
    unsafe {
        println!("Absolute value of -3 according to C: {}", abs(-3));
    }
}
```

Exporting Rust functions to C:
```rust
#[unsafe(no_mangle)]
pub extern "C" fn call_from_c() {
    println!("Just called a Rust function from C!");
}
```

### Mutable Static Variables

```rust
static mut COUNTER: u32 = 0;

unsafe fn add_to_count(inc: u32) {
    unsafe { COUNTER += inc; }
}
```

### Miri — Undefined Behavior Detection

```bash
cargo +nightly miri run
```

## Advanced Types

### Type Aliases

```rust
type Kilometers = i32;
type Thunk = Box<dyn Fn() + Send + 'static>;
type Result<T> = std::result::Result<T, std::io::Error>;
```

### The Never Type `!`

Coerces to any type:
```rust
fn bar() -> ! {
    panic!();
}
// continue, break, panic!, loop all have type !
```

### Dynamically Sized Types (DSTs)

- `str` (not `&str`) is a DST — can't store directly
- Must use behind a pointer: `&str`, `Box<str>`, `Rc<str>`
- `dyn Trait` is also a DST
- `Sized` trait is auto-implemented for compile-time-known sizes
- `?Sized` opt-out: `fn generic<T: ?Sized>(t: &T) { }`

### Function Pointers (`fn` type)

```rust
fn add_one(x: i32) -> i32 { x + 1 }

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

let answer = do_twice(add_one, 5);
```

`fn` implements `Fn`, `FnMut`, `FnOnce` — can always pass functions where closures are expected.

### Returning Closures

```rust
// Single return type — impl Trait works:
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}

// Multiple closure types — need Box<dyn Fn>:
fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}
```

## Macros

### Declarative Macros (`macro_rules!`)

```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

Pattern syntax:
- `$x:expr` — captures any Rust expression as `$x`
- `$( ... ),*` — matches zero or more comma-separated
- `$()*` in body — repeats for each match

### Procedural Macros

Must be in their own crate with `proc-macro = true` in `Cargo.toml`.

**1. Custom Derive Macros:**
```rust
#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    let ast = syn::parse(input).unwrap();
    impl_hello_macro(&ast)
}

fn impl_hello_macro(ast: &syn::DeriveInput) -> TokenStream {
    let name = &ast.ident;
    let generated = quote! {
        impl HelloMacro for #name {
            fn hello_macro() {
                println!("Hello, Macro! My name is {}!", stringify!(#name));
            }
        }
    };
    generated.into()
}
```

Uses `syn` (parsing), `quote` (code generation), `proc_macro` (TokenStream API).

**2. Attribute-like Macros:**
```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
    // attr = attribute contents, item = annotated item
}
```

**3. Function-like Macros:**
```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
    // parse/validate SQL, return generated code
}
```
