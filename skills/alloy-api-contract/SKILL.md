---
name: alloy-api-contract
description: Alloy 1.8.3 contract module API — interact with on-chain contracts via CallBuilder, event querying, contract instances, storage slot finding, and ABI encode/decode.
---

## What I do

Provide the complete public API reference for `alloy::contract` (v1.8.3). Covers building contract calls, querying events, managing contract instances, and finding storage slots.

## When to use me

Use this skill when you need the exact type names and relationships for calling smart contracts, decoding return data, querying event logs, or finding storage slots.

## Feature Flag

`contract`

## Structs

| Struct | Description |
|--------|-------------|
| `CallBuilder<D, P>` | Builder for `eth_sendTransaction` or `eth_call` |
| `ChunkedEvent<D>` | Chunked event query builder |
| `ContractInstance<P, N>` | Handle to a contract at a specific address |
| `EthCall<D, P>` | `eth_call` request with ABI decoder |
| `Event<D, P>` | Helper for managing event filter before querying/streaming |
| `EventPoller<D, P>` | Event poller |
| `EventSubscription<D, P>` | Event subscription |
| `Interface` | Smart contract interface |
| `StorageSlotFinder<P>` | Utility for finding storage slots (useful for ERC20) |

## Enums

| Enum | Description |
|------|-------------|
| `Error` | Contract interaction errors |
| `TryParseTransportErrorResult` | Result of parsing transport error into specific interface |

## Traits

| Trait | Description |
|-------|-------------|
| `CallDecoder` | Decode output of a contract function call |
| `TransportErrorExt` | Extension for parsing `TransportError` |

## Type Aliases

| Alias | Description |
|-------|-------------|
| `DynCallBuilder<P>` | `CallBuilder` using `Function` as call decoder |
| `RawCallBuilder<P>` | `CallBuilder` without a call decoder |
| `SolCallBuilder<P, C>` | `CallBuilder` using `SolCall` type as decoder |
| `Result` | Dynamic contract result type |
