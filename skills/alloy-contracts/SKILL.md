---
name: alloy-contracts
description: Deploy and interact with smart contracts using alloy — deploy from artifact, bytecode, or Solidity source; interact via ABI or contract instance; decode errors and handle unknown return types; simulate arbitrage.
---

## What I do

Provide patterns for deploying and interacting with Ethereum smart contracts using alloy's `sol!` macro and contract APIs.

## When to use me

Use this skill when you need to deploy a contract, call contract methods, decode revert data, interact with contracts dynamically, or simulate DeFi arbitrage.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/contracts/examples/`

### Deploy from Contract Source (`deploy_from_contract.rs`)
- Embed Solidity source with pre-compiled bytecode in `sol!`
- Deploy with `Counter::deploy(&provider)`
```rust
sol! {
    #[sol(rpc, bytecode = "6080604052...")]
    contract Counter {
        function increment() external;
        function number() external view returns (uint256);
    }
}
let contract = Counter::deploy(provider).await?;
```

### Deploy from Artifact (`deploy_from_artifact.rs`)
- Load ABI from a JSON artifact file via `sol!`
```rust
sol! {
    #[sol(rpc)]
    contract Counter { /* from Counter.json */ }
}
```

### Deploy from Bytecode (`deploy_from_bytecode.rs`)
- Deploy from raw hex bytecode using `TransactionRequest`
```rust
let tx = TransactionRequest::default().with_deploy_code(bytecode);
let receipt = provider.send_transaction(tx).await?.get_receipt().await?;
let addr = receipt.contract_address();
```

### Deploy and Link Library (`deploy_and_link_library.rs`)
- Deploy a library, then replace placeholder in contract bytecode

### Interact with ABI (`interact_with_abi.rs`)
- Generate type-safe bindings from ABI JSON via `sol!`
- Call view functions with `.call().await`

### Interact with Contract Instance (`interact_with_contract_instance.rs`)
- Low-level `ContractInstance` + `Interface` API
- Call functions by string name with `DynSolValue` args
```rust
let instance = ContractInstance::new(address, abi.into(), provider);
let result = instance.function("balanceOf", &[DynSolValue::Address(addr)])?.call().await?;
```

### Revert Decoding (`revert_decoding.rs`)
- Catch and decode custom Solidity errors from reverts
- Use `err.as_decoded_error::<T>()` or `err.as_decoded_interface_error::<T>()`

### JSON-RPC Error Decoding (`jsonrpc_error_decoding.rs`)
- Parse raw JSON-RPC error payloads into typed custom errors
- `ErrorPayload::as_decoded_error::<T>()`

### Unknown Return Types (`unknown_return_types.rs`)
- Handle complex/unknown return types with `DynSolValue` at runtime
- No compile-time type bindings needed

### Arbitrage Profit Calculator (`arb_profit_calc.rs`)
- Offline WETH/DAI arbitrage profit calculation
- Computes optimal input amount from hardcoded reserves

### Uniswap V2 Simulation (`simulation_uni_v2.rs`)
- Full on-chain arbitrage simulation via forked Anvil
- Mock storage slots, build swap calldata, execute via FlashBotsMultiCall
