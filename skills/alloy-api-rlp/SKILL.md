---
name: alloy-api-rlp
description: Alloy 1.8.3 rlp module API — Ethereum RLP encoding/decoding with Encodable/Decodable traits, Header, Rlp decoder, derive macros, and helper functions.
---

## What I do

Provide the complete public API reference for `alloy::rlp` (v1.8.3). Covers RLP (Recursive Length Prefix) encoding/decoding used for Ethereum EL data structures.

## When to use me

Use this skill when you need to RLP-encode or decode Ethereum types, implement `Encodable`/`Decodable`, or use RLP derive macros.

## Feature Flag

`rlp`

## Sub-modules

| Module | Description |
|--------|-------------|
| `bytes` | `Bytes`/`BytesMut` abstractions |

## Structs

| Struct | Description |
|--------|-------------|
| `Header` | RLP item header (list vs string, length) |
| `Rlp<'a>` | Active RLP decoder over a payload slice |
| `Bytes` | Cloneable chunk of memory |
| `BytesMut` | Unique reference to memory slice |

## Enums

| Enum | Description |
|------|-------------|
| `Error` | RLP error type |
| `PayloadView<'a>` | Structured RLP payload representation |

## Traits

| Trait | Description |
|-------|-------------|
| `Encodable` | Type encodable via RLP |
| `Decodable` | Type decodable from RLP |
| `MaxEncodedLen` | Max encoded length as const generic |
| `MaxEncodedLenAssoc` | Max encoded length as associated constant |
| `Buf` | Read bytes from buffer |
| `BufMut` | Sequential write to bytes |

## Functions

| Function | Description |
|----------|-------------|
| `encode(val)` | Encode a value |
| `encode_fixed_size(val)` | Encode fixed-size type (feature: `arrayvec`) |
| `encode_iter(iter)` | Encode iterator items |
| `encode_list(list)` | Encode list of items |
| `decode_exact(buf)` | Decode entire input (no trailing bytes) |
| `length_of_length(len)` | Length of RLP length prefix |
| `list_length(items)` | Calculate list length |

## Derive Macros

| Macro | Description |
|-------|-------------|
| `RlpEncodable` | Derive `Encodable` as rlp-list |
| `RlpDecodable` | Derive `Decodable` from rlp-list |
| `RlpEncodableWrapper` | Derive `Encodable` for newtype (single field) |
| `RlpDecodableWrapper` | Derive `Decodable` for newtype |
| `RlpMaxEncodedLen` | Derive `MaxEncodedLen` for constant-size types |

## Constants

| Constant | Description |
|----------|-------------|
| `EMPTY_LIST_CODE` | RLP prefix for 0-length array |
| `EMPTY_STRING_CODE` | RLP prefix for 0-length string |

## Type Aliases

| Alias | Description |
|-------|-------------|
| `Result<T>` | RLP result type |
