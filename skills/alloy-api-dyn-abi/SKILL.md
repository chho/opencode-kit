---
name: alloy-api-dyn-abi
description: Alloy 1.8.3 dyn_abi module API — dynamic Solidity type system with run-time ABI encoding/decoding, EIP-712 typed data, DynSolType/DynSolValue for when types are unknown at compile time.
---

## What I do

Provide the complete public API reference for `alloy::dyn_abi` (v1.8.3). Covers dynamic Solidity type representation, ABI encoding/decoding at runtime, and EIP-712 typed data construction.

## When to use me

Use this skill when Solidity types are not known at compile time — building generic EIP-712 signing interfaces, decoding arbitrary ABI data, or constructing dynamic type representations.

## Feature Flag

`dyn-abi`

## Sub-modules

| Module | Description |
|--------|-------------|
| `abi` | Ethereum ABI codec implementation |
| `eip712` | Dynamic EIP-712 implementation (feature: `eip712`) |
| `parser` | Solidity type parser |

## Structs

| Struct | Description |
|--------|-------------|
| `DecodedError` | Decoded dynamic ABI error |
| `DecodedEvent` | Decoded dynamic ABI event |
| `Decoder<'a>` | Progressive byte-slice deserializer into tokens |
| `DynSolCall` | Representation of a Solidity call |
| `DynSolError` | Dynamic ABI error |
| `DynSolEvent` | Dynamic ABI event |
| `DynSolReturns` | Representation of Solidity call returns |
| `Eip712Domain` | EIP-712 domain attributes |
| `Eip712Types` | Custom types for `TypedData` |
| `Encoder` | ABI encoder |
| `PropertyDef` | EIP-712 property definition |
| `Resolver` | Dependency graph for safe JSON → DynSolType resolution |
| `TypeDef` | EIP-712 type definition |
| `TypedData` | EIP-712 typed data object |

## Enums

| Enum | Description |
|------|-------------|
| `DynSolType` | Dynamic Solidity type (uint, string, tuple, array, etc.) |
| `DynSolValue` | Dynamic Solidity value |
| `DynToken` | Dynamic ABI token |
| `Error` | EIP-712 parsing error |

## Traits

| Trait | Description |
|-------|-------------|
| `ErrorExt` | Error encoding/decoding for `Error` type |
| `EventExt` | Event encoding/decoding for `Event` type |
| `FunctionExt` | ABI encoding/decoding for `Function` type |
| `JsonAbiExt` | ABI encoding/decoding for JSON ABI types |
| `SolType` | Solidity type trait |
| `Specifier` | Resolve to `DynSol*` types |

## Type Aliases

| Alias | Description |
|-------|-------------|
| `Result` | Dynamic ABI result type |
| `Word` | ABI word type (`B256`) |
