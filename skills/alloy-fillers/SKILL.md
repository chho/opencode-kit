---
name: alloy-fillers
description: Use alloy's transaction fillers (middleware) for automatic gas estimation, nonce management, wallet signing, recommended defaults, and custom gas priority.
---

## What I do

Provide patterns for alloy's transaction filler system — middleware that automatically populates transaction fields like gas, nonce, chain ID, and signing.

## When to use me

Use this skill when you need to configure gas estimation, nonce management, wallet signing, or implement custom fillers for alloy providers.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/fillers/examples/`

### Recommended Fillers (`recommended_fillers.rs`)
- Default setup: `ChainIdFiller` + `GasFiller` + `NonceFiller`
- Submit minimal `TransactionRequest` (only `to` and `value`)
```rust
let provider = ProviderBuilder::new().connect_anvil_with_wallet(url).await?;
let tx = TransactionRequest::default().with_to(addr).with_value(U256::from(100));
let receipt = provider.send_transaction(tx).await?.get_receipt().await?;
```

### Gas Estimation Filler (`gas_filler.rs`)
- Automatically fills `gas_limit`, `max_fee_per_gas`, `max_priority_fee_per_gas`
```rust
let provider = ProviderBuilder::new()
    .disable_recommended_fillers()
    .with_gas_estimation()
    .wallet(signer)
    .connect_http(url).await?;
```

### Nonce Management Filler (`nonce_filler.rs`)
- Tracks and auto-increments nonce across sequential transactions
```rust
let provider = ProviderBuilder::new()
    .disable_recommended_fillers()
    .with_cached_nonce_management()
    .wallet(signer)
    .connect_http(url).await?;
```

### Wallet Filler (`wallet_filler.rs`)
- Auto-signs transactions with a local private key
```rust
let provider = ProviderBuilder::new()
    .wallet(signer)
    .connect_http(url).await?;
```

### Custom Gas Priority Filler (`urgent_filler.rs`)
- Implements custom `TxFiller` trait for real-time gas price from external API
- Key trait methods: `status()`, `fill_sync()`, `fill()`, `prepare()`
- Returns `FillerControlFlow` to control pipeline
