---
name: alloy-node-bindings
description: Spin up local Ethereum test nodes (Anvil, Geth, Reth) with alloy node bindings — local instances, forked mainnet, storage mocking, and contract deployment.
---

## What I do

Provide patterns for spawning and interacting with local Ethereum test nodes (Anvil, Geth, Reth) using alloy's node bindings.

## When to use me

Use this skill when you need to spin up a local test node, fork mainnet for testing, mock storage/balances on Anvil, or deploy contracts to a local network.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/node-bindings/examples/`

### Anvil Local Instance (`anvil_local_instance.rs`)
- Manually create and configure Anvil with custom block time and chain ID
```rust
let anvil = Anvil::new().block_time(1).chain_id(1337).try_spawn()?;
let provider = ProviderBuilder::new().connect_http(anvil.endpoint_url());
```

### Anvil Local Provider (`anvil_local_provider.rs`)
- One-step: spawn Anvil and get connected provider
```rust
let provider = ProviderBuilder::new()
    .connect_anvil_with_config(Anvil::new().block_time(1)).await?;
```

### Anvil Fork Instance (`anvil_fork_instance.rs`)
- Fork mainnet state from a remote RPC for testing against real contracts
```rust
let anvil = Anvil::new().fork(rpc_url).try_spawn()?;
```

### Anvil Fork Provider (`anvil_fork_provider.rs`)
- One-step fork: spawn forked Anvil with connected provider

### Deploy Contract on Anvil (`anvil_deploy_contract.rs`)
- Deploy a `Counter` contract using `sol!` with embedded bytecode
- Call `setNumber(42)`, `increment()`, read `number()`

### Mock Storage on Anvil (`anvil_set_storage_at.rs`)
- Artificially set contract storage slots for testing
- Example: mock WETH balance for a target account
```rust
use alloy::node_bindings::AnvilApi;
provider.anvil_set_storage_at(address, slot, value).await?;
```

### Geth Local Instance (`geth_local_instance.rs`)
- Spawn local Geth node with custom chain ID and ports
```rust
let geth = Geth::new().chain_id(1337).port(8545).spawn()?;
```

### Reth Local Instance (`reth_local_instance.rs`)
- Spawn local Reth node in dev mode with discovery disabled
```rust
let reth = Reth::new().dev().disable_discovery().instance(1).spawn()?;
```
