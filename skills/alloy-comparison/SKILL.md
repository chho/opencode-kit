---
name: alloy-comparison
description: Compare block headers and pending transactions across multiple RPC providers using alloy subscriptions. Measures provider latency and propagation speed.
---

## What I do

Provide patterns for comparing multiple Ethereum RPC providers by subscribing to block headers or pending transactions and measuring which provider delivers data first.

## When to use me

Use this skill when you need to measure RPC provider latency, compare provider performance, or subscribe to the same data stream from multiple providers simultaneously.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/comparison/examples/`

### Compare Block Headers (`compare_new_heads.rs`)
- Connect to multiple providers via WebSocket/IPC
- Subscribe to new block headers on each
- Measure which provider delivers each block first and the ms delay for others
```rust
let provider = ProviderBuilder::new()
    .network::<AnyNetwork>()
    .connect_ws(WsConnect::new(ws_url)).await?;
let sub = provider.subscribe_blocks().await?.into_stream();
```

### Compare Pending Transactions (`compare_pending_txs.rs`)
- Subscribe to pending transaction hashes from multiple providers
- Measure propagation latency across providers
- Only works with WebSocket/IPC (not HTTP)
```rust
let sub = provider.subscribe_pending_transactions().await?.into_stream();
```

### Key APIs
- `ProviderBuilder::new().network::<AnyNetwork>().connect_ws()` / `connect_ipc()`
- `provider.subscribe_blocks()` — returns block header stream
- `provider.subscribe_pending_transactions()` — returns pending tx hash stream
- `.into_stream()` — convert subscription to async stream
