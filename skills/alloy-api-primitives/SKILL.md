---
name: alloy-api-primitives
description: Alloy 1.8.3 primitives module API — low-level Ethereum types including Address, Bytes, FixedBytes, Uint, Signed, Signature, Bloom, keccak256, compile-time macros, and 30+ type aliases.
---

## What I do

Provide the complete public API reference for `alloy::primitives` (v1.8.3). Covers all fundamental Ethereum types, hashing, signatures, and compile-time construction macros.

## When to use me

Use this skill when you need the exact type names, macro signatures, or function signatures for primitive Ethereum types — addresses, byte arrays, integers, hashes, signatures, bloom filters.

## Sub-modules

| Module | Description |
|--------|-------------|
| `aliases` | Type aliases for common primitives |
| `map` | Map types (feature: `map`) |
| `ruint` | Rust uint with const generics |
| `utils` | Common Ethereum utilities |

## Macros (Compile-time Construction)

| Macro | Description |
|-------|-------------|
| `address!("0x...")` | Compile-time `Address` |
| `b256!("0x...")` | Compile-time `B256` |
| `b512!("0x...")` | Compile-time `B512` |
| `b128!("0x...")` | Compile-time `B128` |
| `b64!("0x...")` | Compile-time `B64` |
| `bytes!("0x...")` | Compile-time `Bytes` |
| `fixed_bytes!("0x...")` | Compile-time `FixedBytes<N>` |
| `bloom!("0x...")` | Compile-time `Bloom` |
| `hex!("0x...")` | Hex literal to byte array |
| `wrap_fixed_bytes!` | Wrap byte array in newtype |
| `try_vec!` | Try creating `Vec` |

## Structs

| Struct | Description |
|--------|-------------|
| `Address` | 20-byte Ethereum address (EIP-55/EIP-1191 checksum) |
| `Bytes` | Wrapper around `bytes::Bytes` with hex support |
| `FixedBytes<N>` | Fixed-length byte array `[u8; N]` |
| `Bloom` | 256-byte Ethereum bloom filter |
| `Uint<BITS, LIMBS>` | Unsigned integer mod 2^BITS |
| `Signed<BITS, LIMBS>` | Signed integer wrapping `Uint` |
| `Signature` | Ethereum ECDSA signature (65 bytes) |
| `Log<T>` | Log: address + data |
| `LogData` | Ethereum event log object |
| `Sealed<T>` | Hashable item with memoized hash |
| `Function` | ABI function pointer (24 bytes) |
| `Keccak256` | Keccak-256 hasher |
| `AddressChecksumBuffer` | Stack buffer for address checksums |

## Enums

| Enum | Description |
|------|-------------|
| `TxKind` | `to` field: address or create |
| `BloomInput` | Input to `Bloom::accrue` |
| `Sign` | Sign of signed integer |
| `AddressError` | Address checksum error |
| `SignatureError` | Signature parsing/verification error |
| `ParseSignedError` | Signed integer parse error |
| `BigIntConversionError` | Integer conversion error |

## Functions

| Function | Description |
|----------|-------------|
| `keccak256(data)` | Keccak-256 hash |
| `keccak256_uncached(data)` | Keccak-256 (no caching) |
| `eip191_hash_message(msg)` | EIP-191 personal message hash |
| `logs_bloom(logs)` | Bloom filter for logs |
| `normalize_v(v)` | Normalize v to parity boolean |
| `to_eip155_v(v, chain_id)` | Apply EIP-155 to v |

## Traits

| Trait | Description |
|-------|-------------|
| `Sealable` | Sealable objects |
| `IntoLogData` | Convert to log data |
| `FixedBytesSliceExt` | Flatten `FixedBytes` slice |
| `FixedBytesVecExt` | Flatten `Vec<FixedBytes>` |

## Type Aliases

| Alias | Bits | Description |
|-------|------|-------------|
| `U8` / `I8` | 8 | 8-bit int |
| `U16` / `I16` | 16 | 16-bit int |
| `U32` / `I32` | 32 | 32-bit int |
| `U64` / `I64` | 64 | 64-bit int |
| `U128` / `I128` | 128 | 128-bit int |
| `U160` / `I160` | 160 | 160-bit int |
| `U256` / `I256` | 256 | 256-bit int |
| `U512` | 512 | 512-bit int |
| `B64` | 64 | 8-byte fixed bytes |
| `B128` | 128 | 16-byte fixed bytes |
| `B256` | 256 | 32-byte fixed bytes |
| `B512` | 512 | 64-byte fixed bytes |
| `BlockHash` | 256 | Block hash |
| `TxHash` | 256 | Transaction hash |
| `ChainId` | 64 | Chain ID |
| `Selector` | 32 | Function selector (4 bytes) |
| `StorageKey` | 256 | Storage key |
| `StorageValue` | 256 | Storage value |

## Constants

| Constant | Description |
|----------|-------------|
| `KECCAK256_EMPTY` | Keccak-256 of empty string |
| `BLOOM_SIZE_BITS` | Bloom size in bits |
| `BLOOM_SIZE_BYTES` | Bloom size in bytes |
| `BLOOM_BITS_PER_ITEM` | Bits set per bloom input |
