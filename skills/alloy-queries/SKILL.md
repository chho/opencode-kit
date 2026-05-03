---
name: alloy-queries
description: Query on-chain data with alloy — read contract storage slots, fetch deployed bytecode, and filter event logs.
---

## What I do

Provide patterns for querying on-chain data: reading contract storage, fetching bytecode, and querying event logs.

## When to use me

Use this skill when you need to read raw storage from a deployed contract, inspect its bytecode, or search for event logs.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/queries/examples/`

### Query Contract Storage (`query_contract_storage.rs`)
- Read raw storage slot value from a deployed contract
```rust
let slot_value: U256 = provider.get_storage_at(address, U256::ZERO).await?;
// For specific mapping keys: compute keccak256 of key first
```

### Query Deployed Bytecode (`query_deployed_bytecode.rs`)
- Get runtime bytecode of a contract at a given address
```rust
let bytecode: Bytes = provider.get_code_at(address).await?;
```

### Query Logs (`query_logs.rs`)
- Filter and query event logs by event signature or contract address
```rust
let filter = Filter::new()
    .from_block(BlockNumberOrTag::Latest)
    .event("Transfer(address,address,uint256)")
    .address(address!("0x..."));
let logs: Vec<Log> = provider.get_logs(&filter).await?;
```

### Key APIs
- `provider.get_storage_at(address, slot)` — raw storage read
- `provider.get_code_at(address)` — deployed bytecode
- `provider.get_logs(&filter)` — query event logs
- `Filter::new()` — build log filters with `.address()`, `.event()`, `.from_block()`
