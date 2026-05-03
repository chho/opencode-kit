---
name: alloy-subscriptions
description: Subscribe to Ethereum events via alloy — new blocks, pending transactions, filtered logs, all contract events, and event multiplexing with tokio::select!
---

## What I do

Provide patterns for subscribing to real-time Ethereum data streams: blocks, pending transactions, and contract event logs.

## When to use me

Use this skill when you need to subscribe to new blocks, monitor pending transactions, listen for contract events, or multiplex multiple event streams.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/subscriptions/examples/`

### Subscribe to Blocks (`subscribe_blocks.rs`)
- WebSocket subscription yielding full block headers
- Poll-based watcher yielding block hashes
```rust
let sub = provider.subscribe_blocks().await?.into_stream();
let poller = provider.watch_blocks().await?.into_stream();
```

### Subscribe Pending Transactions (`subscribe_pending_transactions.rs`)
- Stream of pending tx hashes from the mempool (WebSocket only)
```rust
let sub = provider.subscribe_pending_transactions().await?.into_stream();
// Fetch full tx details
let tx = provider.get_transaction_by_hash(hash).await?;
```

### Subscribe to Specific Logs (`subscribe_logs.rs`)
- WebSocket log subscription filtered by event signature and address
```rust
let filter = Filter::new()
    .address(addr)
    .event("Transfer(address,address,uint256)")
    .from_block(BlockNumberOrTag::Latest);
let sub = provider.subscribe_logs(&filter).await?.into_stream();
```

### Subscribe All Logs (`subscribe_all_logs.rs`)
- Subscribe to all events from a contract, pattern-match on `topic0`
- Decode typed events using `sol!`-generated `SIGNATURE_HASH`
```rust
let log = provider.subscribe_logs(&filter).await?.into_stream().next().await;
match log.topic0() {
    x if x == &IWETH9::Approval::SIGNATURE_HASH => { /* decode */ }
    x if x == &IWETH9::Transfer::SIGNATURE_HASH => { /* decode */ }
    _ => {}
}
```

### Poll Logs (`poll_logs.rs`)
- Type-safe event polling via `<Event>_filter().watch()`
- Works without WebSocket (HTTP polling)
```rust
let poller = contract.Increment_filter().watch().await?;
let stream = poller.into_stream();
```

### Event Multiplexer (`event_multiplexer.rs`)
- Deploy contract, multiplex multiple event streams with `tokio::select!`
```rust
let add_stream = contract.Add_filter().watch().await?.into_stream();
let sub_stream = contract.Sub_filter().watch().await?.into_stream();
tokio::select! {
    Some(log) = add_stream.next() => { /* handle Add */ }
    Some(log) = sub_stream.next() => { /* handle Sub */ }
}
```
