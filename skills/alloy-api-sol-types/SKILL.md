---
name: alloy-api-sol-types
description: Alloy 1.8.3 sol_types module API — Solidity type system with sol! macro, ABI codec, EIP-712 domain, SolType/SolCall/SolEvent/SolError/SolStruct traits, and revert/panic handling.
---

## What I do

Provide the complete public API reference for `alloy::sol_types` (v1.8.3). Covers the Solidity type system, ABI codec, EIP-712 typed data, and the `sol!` macro for generating type-safe Rust bindings.

## When to use me

Use this skill when you need the exact trait names and methods for ABI encoding/decoding, EIP-712 signing, contract error handling, or `sol!` macro usage patterns.

## Feature Flag

`sol-types`

## Sub-modules

| Module | Description |
|--------|-------------|
| `abi` | Ethereum ABI codec implementation |
| `sol_data` | Solidity type definitions |
| `utils` | Internal utilities |

## Macros

| Macro | Description |
|-------|-------------|
| `sol! { ... }` | Generate types implementing alloy-sol-types traits for type-safe ABI/EIP-712 serialization |
| `eip712_domain! { ... }` | Instantiate an EIP-712 domain |

## Structs

| Struct | Description |
|--------|-------------|
| `Eip712Domain` | EIP-712 domain attributes |
| `Revert` | Solidity revert (`revert(reason)` / `require(cond, reason)`) |
| `Panic` | Solidity panic (`assert`/`require`) |
| `Selectors` | Iterator over function/error selectors |

## Enums

| Enum | Description |
|------|-------------|
| `ContractError<C, D>` | Generic contract error |
| `Error` | ABI encoding/decoding errors |
| `PanicKind` | Solidity panic code |
| `RevertReason` | Revert reason |

## Key Traits

| Trait | Description |
|-------|-------------|
| `SolType` | Maps Solidity type to Rust type |
| `SolValue` | A Solidity value |
| `SolCall` | Solidity function call (ABI encode/decode) |
| `SolEvent` | Solidity event |
| `SolEventInterface` | Collection of `SolEvent`s |
| `SolError` | Solidity custom error |
| `SolInterface` | Collection of ABI-encodable call-like types |
| `SolStruct` | Solidity struct (EIP-712 signing) |
| `SolEnum` | Solidity enum (wrapper around `u8`) |
| `SolConstructor` | Solidity constructor |
| `EventTopic` | Solidity event topic |
| `TopicList` | List of event topics |
| `JsonAbiExt` | ABI representation extension |

## Functions

| Function | Description |
|----------|-------------|
| `decode_revert_reason(data)` | Decode revert reason from output data |

## Type Aliases

| Alias | Description |
|-------|-------------|
| `Result<T>` | ABI result type |
| `Word` | ABI word type (`B256`) |
| `GenericContractError` | Generic contract error alias |
| `GenericRevertReason` | Generic revert reason alias |
