---
name: alloy-layers
description: Apply tower middleware layers to alloy transports — retry with backoff, fallback across providers, request logging, delay injection, and custom Hyper HTTP layers.
---

## What I do

Provide patterns for adding tower middleware layers to alloy RPC transports for resilience, observability, and failover.

## When to use me

Use this skill when you need to add retry logic, fallback providers, request/response logging, or custom HTTP middleware to alloy transports.

## Examples Reference

Source: `~//hao/Dev/solana/alloy_examples/examples/layers/examples/`

### Retry with Backoff (`retry_layer.rs`)
- Built-in `RetryBackoffLayer` for automatic retry on rate-limited requests (HTTP 429)
```rust
let layer = RetryBackoffLayer::new(max_retries, backoff_ms, compute_units_per_second);
let client = RpcClient::builder().layer(layer).http(url);
let provider = ProviderBuilder::new().connect_client(client);
```

### Fallback Layer (`fallback_layer.rs`)
- Cycle through multiple RPC transports, auto-failover on failure
- Periodically re-ranks transports by responsiveness
```rust
let layer = FallbackLayer::default().with_active_transport_count(2);
let client = tower::ServiceBuilder::new().layer(layer).service(transports);
```

### Logging Layer (`logging_layer.rs`)
- Custom `tower::Layer` / `tower::Service` that intercepts `RequestPacket` / `ResponsePacket`
- Print raw JSON-RPC requests and responses for debugging

### Delay Layer (`delay_layer.rs`)
- Custom `tower::Layer` that sleeps before forwarding each request
- Pattern for inserting arbitrary async middleware

### Hyper HTTP Layer (`hyper_http_layer.rs`)
- Insert middleware at the Hyper HTTP level (below JSON-RPC)
- Example: add custom HTTP headers to every request
```rust
let hyper_client = HyperClient::with_service(tower_service, url);
let transport = Http::with_client(hyper_client, url);
```
