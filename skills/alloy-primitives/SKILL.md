---
name: alloy-primitives
description: Work with alloy primitive types — bytes, addresses, fixed-size byte arrays, and hashing functions (keccak256, EIP-191).
---

## What I do

Provide patterns for constructing and using alloy's primitive types and cryptographic hashing functions.

## When to use me

Use this skill when you need to create Ethereum addresses, byte arrays, compute keccak256 hashes, or sign/verify message hashes.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/primitives/examples/`

### Bytes and Address Types (`bytes_and_address_types.rs`)
- Compile-time literal macros for all primitive types:
```rust
use alloy::primitives::{bytes, address, b256, b512, fixed_bytes, Bytes, Address, FixedBytes};

let addr = address!("0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045");
let hash = b256!("0x...");
let dynamic_bytes: Bytes = bytes!("0x...");
let custom = fixed_bytes!("0x..."); // FixedBytes<N>
```

### Hashing Functions (`hashing_functions.rs`)
- `keccak256(data)` — standard Ethereum hash
- `eip191_hash_message(msg)` — prefixed hash for personal message signing
```rust
use alloy::primitives::{keccak256, eip191_hash_message};
let hash = keccak256(b"hello");
let personal_hash = eip191_hash_message(b"hello");
```
