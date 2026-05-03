---
name: alloy-advanced
description: Advanced alloy patterns — AnyNetwork for L2 chains, JSON ABI decoding, dynamic and static ABI encoding, EIP-712 typed data, foundry-fork-db caching, Uniswap V2 arbitrage math and simulation.
---

## What I do

Provide advanced alloy patterns: L2 network handling, ABI introspection, encoding (dynamic and static), EIP-712 signing, fork DB caching, and DeFi arbitrage calculations.

## When to use me

Use this skill when you need to work with non-Ethereum L2 chains, inspect or encode ABI data, compute EIP-712 hashes, cache fork data, or implement DEX arbitrage math.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/advanced/examples/`

### AnyNetwork (`any_network.rs`)
- Interact with non-Ethereum L2 chains (e.g. Arbitrum Sepolia)
- Extract network-specific receipt fields from `receipt.other`
```rust
let provider = ProviderBuilder::new()
    .network::<AnyNetwork>()
    .wallet(signer)
    .connect_http(url).await?;
let receipt = provider.send_transaction(tx).await?.get_receipt().await?;
let l1_gas: U128 = receipt.other.deserialize_into("gasUsedForL1")?;
```

### Decoding JSON ABI (`decoding_json_abi.rs`)
- Load and inspect contract ABI from JSON file
```rust
let abi: JsonAbi = serde_json::from_slice(&json_bytes)?;
for func in abi.functions() { /* iterate */ }
let transfer = abi.function("transfer")?;
```

### Dynamic ABI Encoding (`encoding_dyn_abi.rs`)
- Runtime type construction and EIP-712 encoding with `DynSolType` / `DynSolValue`
```rust
let domain_type = DynSolType::Tuple(vec![
    DynSolType::String, DynSolType::String, DynSolType::Uint(256), DynSolType::Address,
]);
let domain_value = DynSolValue::Tuple(vec![...]);
let encoded = domain_value.abi_encode();
let hash = keccak256(encoded);
let sig = signer.sign_hash(hash.into()).await?;
```

### Static Encoding with sol! (`encoding_sol_static.rs`)
- Compile-time calldata encoding via `sol!` function declarations
```rust
sol! {
    function swapExactTokensForTokens(
        uint256 amountIn, uint256 amountOutMin, address[] calldata path,
        address to, uint256 deadline
    ) external returns (uint256[] memory amounts);
}
let call = swapExactTokensForTokensCall { amount_in: U256::from(100), ... };
let calldata = call.abi_encode();
```

### Foundry Fork DB (`foundry_fork_db.rs`)
- Cache RPC data with `foundry_fork_db::BlockchainDb` + `SharedBackend`
- Execute EVM transactions via `revm` and commit state back
```rust
let db = BlockchainDb::new(BlockchainDbMeta::new(...), Some(cache_path));
let backend = SharedBackend::spawn(provider, db);
// Benchmark cached vs uncached fetches
// Execute via revm EVM
```

### Uniswap V2 Profit (alloy) (`uniswap_u256_alloy_profit.rs`)
- Offline arbitrage profit calc using alloy `U256`
- Compute optimal input across Uniswap/SushiSwap pairs

### Uniswap V2 Profit (ethers) (`uniswap_u256_ethers_profit.rs`)
- Same calculation using ethers-rs `U256` for comparison

### Uniswap V2 Simulation (`uniswap_u256_alloy_simulation.rs`)
- Full on-chain arbitrage simulation on forked Anvil
- Mock storage, build swap calldata, execute via FlashBotsMultiCall
- Verify profit by checking WETH balance before/after
