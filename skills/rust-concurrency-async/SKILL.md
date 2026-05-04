---
name: rust-concurrency-async
description: Rust concurrency and async programming - threads, message passing (mpsc channels), shared-state concurrency (Mutex, Arc), Send/Sync traits, async/await, futures, streams, and tokio runtime. Load this when building concurrent or asynchronous Rust applications.
---

# Rust Concurrency & Async Programming

> Based on *The Rust Programming Language* (Rust 1.90.0, Edition 2024)

## Fearless Concurrency

### `thread::spawn` — Creating Threads

```rust
use std::thread;
use std::time::Duration;

let handle = thread::spawn(|| {
    for i in 1..10 {
        println!("hi number {i} from the spawned thread!");
        thread::sleep(Duration::from_millis(1));
    }
});

for i in 1..5 {
    println!("hi number {i} from the main thread!");
    thread::sleep(Duration::from_millis(1));
}

handle.join().unwrap();  // wait for thread to finish
```

**Key points:**
- When main thread completes, all spawned threads are shut down
- `JoinHandle<T>.join()` blocks until thread finishes
- `join()` placement affects whether threads run concurrently

### `move` Closures with Threads

Must use `move` to transfer ownership into spawned threads:

```rust
let v = vec![1, 2, 3];
let handle = thread::spawn(move || {
    println!("Here's a vector: {v:?}");
});
handle.join().unwrap();
```

### Message Passing — mpsc Channels

**Multiple Producer, Single Consumer** channels:

```rust
use std::sync::mpsc;

let (tx, rx) = mpsc::channel();

// Transmitter in spawned thread
thread::spawn(move || {
    let val = String::from("hi");
    tx.send(val).unwrap();  // val ownership transferred!
});

// Receiver in main thread
let received = rx.recv().unwrap();  // blocks until message arrives
println!("Got: {received}");
```

- `send()` takes ownership of the value — prevents use-after-send bugs
- `recv()` blocks until a message arrives
- `try_recv()` returns immediately (non-blocking)
- `rx` can be used as an iterator: `for received in rx { ... }`

**Multiple producers** by cloning the transmitter:
```rust
let tx1 = tx.clone();
// Each thread gets its own tx or tx1
```

### `Mutex<T>` — Mutual Exclusion

```rust
use std::sync::Mutex;

let m = Mutex::new(5);
{
    let mut num = m.lock().unwrap();  // blocks until lock acquired
    *num = 6;
}  // lock automatically released (Drop trait on MutexGuard)

println!("m = {m:?}");
```

`lock()` returns a `MutexGuard` that implements `Deref` and `Drop` (auto-releases lock).

### `Arc<T>` — Atomic Reference Counting

Thread-safe version of `Rc<T>`. Use with `Mutex<T>` for shared mutable state:

```rust
use std::sync::{Arc, Mutex};
use std::thread;

let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let counter = Arc::clone(&counter);
    let handle = thread::spawn(move || {
        let mut num = counter.lock().unwrap();
        *num += 1;
    });
    handles.push(handle);
}

for handle in handles {
    handle.join().unwrap();
}

println!("Result: {}", *counter.lock().unwrap());  // Result: 10
```

**Why Arc and not Rc?** `Rc<T>` doesn't implement `Send` — it doesn't use atomics for reference counting, causing potential data races. `Arc<T>` uses atomic operations (with a small performance cost).

**Comparison:** `RefCell<T>/Rc<T>` (single-threaded) ≈ `Mutex<T>/Arc<T>` (multi-threaded)

### `Send` and `Sync` Traits

**Marker traits** (no methods):

| Trait | Meaning | Example |
|-------|---------|---------|
| `Send` | Ownership can be **transferred** between threads safely | Almost all types implement it; `Rc<T>` does NOT |
| `Sync` | Can be **referenced** from multiple threads safely (`&T` is `Send`) | `Mutex<T>` is `Sync`; `RefCell<T>` is NOT |

**Key rules:**
- Types composed entirely of `Send` types are automatically `Send`
- Types composed entirely of `Sync` types are automatically `Sync`
- `Rc<T>` is NOT `Send` or `Sync`
- `Arc<T>` IS `Send` and `Sync`
- Manually implementing `Send`/`Sync` requires `unsafe` code

## Async Programming

### Parallelism vs Concurrency

- **Concurrency**: Switching between tasks before any completes (single worker multitasking)
- **Parallelism**: Multiple workers each handling a task simultaneously
- **CPU-bound** operations are limited by processing speed
- **I/O-bound** operations wait on input/output
- Async Rust runs **concurrently** by default, may use parallelism under the hood

### Futures and `async`/`await`

Futures are values that may not be ready now but will become ready later. They implement the `Future` trait. Futures in Rust are **lazy** — they do nothing until you `.await` them.

```rust
async fn page_title(url: &str) -> Option<String> {
    let response = trpl::get(url).await;
    let response_text = response.text().await;
    Html::parse(&response_text)
        .select_first("title")
        .map(|title| title.inner_html())
}
```

Key points:
- `await` is a **postfix** keyword: `expression.await` (not `await expression`)
- `async fn` is sugar for returning `impl Future<Output = T>`
- `main` cannot be `async` — you need a **runtime** (e.g., `tokio::runtime`, `trpl::block_on`)

```rust
fn main() {
    trpl::block_on(async {
        let url = &args[1];
        match page_title(url).await {
            Some(title) => println!("The title for {url} was {title}"),
            None => println!("{url} had no title"),
        }
    })
}
```

### Spawning Tasks

```rust
trpl::block_on(async {
    let handle = trpl::spawn_task(async {
        for i in 1..10 {
            println!("hi number {i} from the first task!");
            trpl::sleep(Duration::from_millis(500)).await;
        }
    });

    for i in 1..5 {
        println!("hi number {i} from the second task!");
        trpl::sleep(Duration::from_millis(500)).await;
    }

    handle.await.unwrap();
});
```

### Joining Futures

```rust
let fut1 = async { /* ... */ };
let fut2 = async { /* ... */ };
trpl::join(fut1, fut2).await;  // runs concurrently, waits for both
```

Key insight: **Code within one async block executes linearly.** You need separate async blocks + `join` for concurrency.

### Async Message Passing

```rust
let (tx, mut rx) = trpl::channel();

// Sender (in one async block):
tx.send(val).unwrap();

// Receiver (in another async block):
while let Some(value) = rx.recv().await {
    println!("received '{value}'");
}
```

### Yielding Control & Select

```rust
trpl::yield_now().await;  // hands control back to runtime

// select — races futures, returns as soon as one completes
match trpl::select(future_to_try, trpl::sleep(max_time)).await {
    Either::Left(output) => Ok(output),
    Either::Right(_) => Err(max_time),
}
```

### Streams

Streams are **asynchronous iterators** — they yield a sequence of values over time.

```rust
use trpl::StreamExt;

let values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let iter = values.iter().map(|n| n * 2);
let mut stream = trpl::stream_from_iter(iter);

while let Some(value) = stream.next().await {
    println!("The value was: {value}");
}
```

**Stream trait** definition:
```rust
trait Stream {
    type Item;
    fn poll_next(
        self: Pin<&mut Self>,
        cx: &mut Context<'_>,
    ) -> Poll<Option<Self::Item>>;
}
```

### Future Trait Deep Dive

```rust
pub trait Future {
    type Output;
    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}

pub enum Poll<T> {
    Ready(T),
    Pending,
}
```

### Pin and Unpin

- `Pin` wraps pointer-like types and prevents the pointed-to value from moving in memory
- Needed because async block futures can be **self-referential** (internal references)
- `Unpin` is a marker trait — most types auto-implement it (they're safe to move)
- `!Unpin` types require pinning when used with `Pin<...>`
- Use `pin!` macro or `Box::pin` to pin futures

```rust
let futures: Vec<Pin<&mut dyn Future<Output = ()>>> = vec![
    pin!(async { /* ... */ }),
    pin!(async { /* ... */ }),
];
trpl::join_all(futures).await;
```
