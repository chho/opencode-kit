---
name: alloy-api-serde
description: Alloy 1.8.3 serde module API — serde helpers for Ethereum types including quantity format, EIP-55 checksum, TTD serialization, hex strings, and OtherFields.
---

## What I do

Provide the complete public API reference for `alloy::serde` (v1.8.3). Covers serde utilities for Ethereum-specific serialization patterns.

## When to use me

Use this skill when you need custom serde functions for Ethereum types — quantity encoding, hex strings, address checksums, or handling extra JSON fields.

## Feature Flag

`serde`

## Sub-modules

| Module | Description |
|--------|-------------|
| `checksum` | EIP-55 checksummed address serde |
| `displayfromstr` | FromStr/Display-based serde |
| `quantity` | Ethereum JSON-RPC quantity format |
| `storage` | Storage-related helpers |
| `ttd` | Geth-compatible TTD format |

## Structs

| Struct | Description |
|--------|-------------|
| `OtherFields` | Generic type for capturing extra fields during deserialization |
| `WithOtherFields<T>` | Extension to capture additional fields |

## Enums

| Enum | Description |
|------|-------------|
| `JsonStorageKey` | Storage key as hex string (up to 32 bytes) for `eth_getStorageAt` |

## Functions

| Function | Description |
|----------|-------------|
| `serialize` | Serialize optional TTD as JSON number |
| `deserialize` | Deserialize optional TTD from number or string |
| `deserialize_json_ttd_opt` | Parse TTD as `Option<u64>` or `Option<f64>` |
| `null_as_default` | Deserialize possibly-null value as default |
| `reject_if_some` | Reject field that must be missing |
| `serialize_hex_string_no_prefix` | Serialize bytes as hex without "0x" |
| `serialize_b256_hex_string_no_prefix` | Serialize `B256` as hex without "0x" |
