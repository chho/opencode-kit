---
name: alloy-api-rpc
description: Alloy 1.8.3 rpc module API — Ethereum JSON-RPC client, types, and JSON-RPC 2.0 data types via sub-modules client, json_rpc, and types.
---

## What I do

Provide the complete public API reference for `alloy::rpc` (v1.8.3). Covers the RPC module which re-exports three sub-crates: the RPC client, JSON-RPC types, and Ethereum RPC type definitions.

## When to use me

Use this skill when you need to navigate the RPC module structure or understand the relationship between `rpc::client`, `rpc::json_rpc`, and `rpc::types`.

## Feature Flag

`rpc`

## Sub-modules

| Module | Feature Flag | Description |
|--------|-------------|-------------|
| `client` | `rpc-client` | Low-level RPC client implementation |
| `json_rpc` | `json-rpc` | JSON-RPC 2.0 data types |
| `types` | `rpc-types` | Ethereum JSON-RPC type definitions |

## Module Structure

- `rpc::client` — `alloy-rpc-client`: the low-level client for making JSON-RPC requests, batch requests, and managing connections
- `rpc::json_rpc` — `alloy-json-rpc`: core types like `Request`, `Response`, `Error`, `Params`, `Id`, `Version`
- `rpc::types` — `alloy-rpc-types`: meta-crate re-exporting all Ethereum RPC type namespaces (`eth`, `debug`, `trace`, `txpool`, `admin`, `engine`, `beacon`, `mev`, `anvil`, `tenderly`)

### Key Types in `rpc::json_rpc`

| Type | Description |
|------|-------------|
| `Request<S>` | JSON-RPC request |
| `Response<Payload>` | JSON-RPC response |
| `Error` | JSON-RPC error object |
| `Params` | JSON-RPC params (positional or named) |
| `Id` | Request/response ID |
| `Version` | JSON-RPC version |

### Key Types in `rpc::types` (sub-modules)

| Sub-module | Description |
|------------|-------------|
| `eth` | `eth_*` namespace types (blocks, transactions, logs, filters, receipts) |
| `debug` | `debug_*` namespace types |
| `trace` | `trace_*` namespace types (parity-style) |
| `txpool` | Txpool content/inspect types |
| `admin` | Node info types |
| `engine` | Engine API types (ForkchoiceUpdated, NewPayload, etc.) |
| `beacon` | Beacon chain types |
| `mev` | MEV bundle types |
| `anvil` | Anvil-specific types |
| `tenderly` | Tenderly node types |
