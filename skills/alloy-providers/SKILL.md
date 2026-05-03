---
name: alloy-providers
description: Connect to Ethereum via alloy providers — HTTP, WebSocket, IPC, batch RPC, multicall, mocking, wrapped providers, DynProvider, and consensus type conversion.
---

## What I do

Provide patterns for creating and using alloy providers to connect to Ethereum nodes via various transports and advanced provider patterns.

## When to use me

Use this skill when you need to connect to an Ethereum node, send RPC calls, batch requests, mock providers for testing, wrap providers in custom types, or use type-erased providers.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/providers/examples/`

### HTTP Provider (`http.rs`)
- Simplest provider: connect and fetch block number
```rust
let provider = ProviderBuilder::new().connect_http(url);
let block = provider.get_block_number().await?;
```

### HTTP with Auth (`http_with_auth.rs`)
- Attach `Authorization` header via custom `reqwest::Client`
```rust
let http = Http::with_client(reqwest_client, url);
let client = RpcClient::new(http);
let provider = ProviderBuilder::new().connect_client(client);
```

### WebSocket Provider (`ws.rs`)
- Connect via WS and subscribe to blocks
```rust
let provider = ProviderBuilder::new().connect_ws(WsConnect::new(url)).await?;
let sub = provider.subscribe_blocks().await?;
```

### WS with Auth (`ws_with_auth.rs`)
- Basic or Bearer token auth on WebSocket
```rust
let ws = WsConnect::new(url).with_auth(Authorization::bearer(token));
```

### IPC Provider (`ipc.rs`)
- Connect via IPC socket
```rust
let provider = ProviderBuilder::new().connect_ipc(IpcConnect::new(path)).await?;
```

### Built-in Auto-detect (`builtin.rs`)
- Auto-detect transport from URL scheme: `http://`, `ws://`, IPC path
```rust
let provider = ProviderBuilder::new().connect(url).await?;
```

### Builder Pattern (`builder.rs`)
- Build provider with wallet signer, then send ETH transfer
```rust
let provider = ProviderBuilder::new().wallet(signer).connect_http(url);
```

### Basic Provider (No Fillers) (`basic_provider.rs`)
- Minimal provider with all fillers disabled; manually set every field

### JSON-RPC Batch Request (`batch_rpc.rs`)
- Send multiple RPC calls as a single HTTP batch
```rust
let mut batch = client.new_batch();
let block_fut = batch.add_call::<_, U64>("eth_blockNumber", &[])?;
let gas_fut = batch.add_call::<_, U256>("eth_gasPrice", &[])?;
batch.send().await?;
let block = block_fut.await?;
```

### Multicall Builder (`multicall.rs`)
- Aggregate multiple contract calls into a single Multicall3 invocation
```rust
let results = provider.multicall()
    .add(erc20.balanceOf(addr))
    .add(erc20.totalSupply())
    .aggregate().await?;
```

### Multicall Batching Layer (`multicall_batching.rs`)
- Auto-aggregate concurrent `eth_call` requests into Multicall3 calls
```rust
let provider = ProviderBuilder::new()
    .layer(CallBatchLayer::new().wait(Duration::from_millis(100)))
    .connect_http(url);
```

### DynProvider (`dyn_provider.rs`)
- Type-erased provider via `.erased()`, avoids verbose generics
```rust
let dyn_provider: DynProvider = provider.erased();
```

### Wrapped Provider (`wrapped_provider.rs`)
- Wrap provider in custom structs with `Provider` trait as generic bound
```rust
struct Deployer<P: Provider> { provider: P }
```

### Mocking (`mocking.rs`)
- `MockTransport` + `Asserter` for unit testing without network
```rust
let asserter = Asserter::new();
asserter.push_success(abi_encoded_data);
let provider = ProviderBuilder::new().connect_mocked_client(asserter);
```

### Consensus x RPC Types (`embed_consensus_rpc.rs`)
- Convert RPC types to consensus types via `.into_consensus()`, `.into_recovered()`
