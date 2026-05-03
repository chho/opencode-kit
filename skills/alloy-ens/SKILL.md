---
name: alloy-ens
description: Resolve ENS names to Ethereum addresses and perform reverse lookups (address to ENS name) using alloy's ENS provider extension.
---

## What I do

Provide patterns for ENS (Ethereum Name Service) resolution and reverse lookups using alloy.

## When to use me

Use this skill when you need to resolve an ENS name like "vitalik.eth" to an address, or look up the ENS name for a given Ethereum address.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/ens/examples/`

### Name Resolution (`name_resolution.rs`)
- Forward lookup: ENS name → Ethereum address
```rust
use alloy::providers::ProviderEnsExt;
let address = provider.resolve_name("vitalik.eth").await?;
```

### Address Lookup (`address_lookup.rs`)
- Reverse lookup: Ethereum address → ENS name
```rust
use alloy::providers::ProviderEnsExt;
let name = provider.lookup_address(addr).await?;
```

### Key APIs
- `ProviderEnsExt` trait — adds `resolve_name()` and `lookup_address()` to any provider
- `ProviderBuilder::new().connect_http(url)` — standard HTTP provider
- `address!()` macro — compile-time address literal
