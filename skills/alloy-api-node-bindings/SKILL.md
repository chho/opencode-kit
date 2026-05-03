---
name: alloy-api-node-bindings
description: Alloy 1.8.3 node_bindings module API — local Ethereum node management for Anvil, Geth, and Reth with builder pattern and instance lifecycle.
---

## What I do

Provide the complete public API reference for `alloy::node_bindings` (v1.8.3). Covers launching and managing local Ethereum test nodes (Anvil, Geth, Reth).

## When to use me

Use this skill when you need to launch local test nodes, fork mainnet, configure Anvil/Geth/Reth instances, or manage node lifecycle for testing.

## Feature Flag

`node-bindings`

## Sub-modules

| Module | Description |
|--------|-------------|
| `nodes` | Bindings for various nodes |
| `utils` | Utility functions |

## Structs

| Struct | Description |
|--------|-------------|
| `Anvil` | Builder for launching `anvil` |
| `AnvilInstance` | Running anvil instance (closes on drop) |
| `Geth` | Builder for launching `geth` |
| `GethInstance` | Running geth instance (closes on drop) |
| `Reth` | Builder for launching `reth` |
| `RethInstance` | Running reth instance (closes on drop) |

## Enums

| Enum | Description |
|------|-------------|
| `NodeError` | Errors for node instances |

## Constants

| Constant | Description |
|----------|-------------|
| `WEI_IN_ETHER` | 1 Ether = 1e18 Wei |
| `EIP1559_FEE_ESTIMATION_DEFAULT_PRIORITY_FEE` | Default max priority fee |
| `EIP1559_FEE_ESTIMATION_PAST_BLOCKS` | Blocks for fee history |
| `EIP1559_FEE_ESTIMATION_PRIORITY_FEE_TRIGGER` | Base fee threshold |
| `EIP1559_FEE_ESTIMATION_REWARD_PERCENTILE` | Gas premium percentile |
| `EIP1559_FEE_ESTIMATION_THRESHOLD_MAX_CHANGE` | Max change threshold (%) |
| `NODE_STARTUP_TIMEOUT` | Node ready timeout |
| `NODE_DIAL_LOOP_TIMEOUT` | Peer addition timeout |
