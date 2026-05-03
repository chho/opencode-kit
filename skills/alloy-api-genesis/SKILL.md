---
name: alloy-api-genesis
description: Alloy 1.8.3 genesis module API — Ethereum genesis file definitions including chain config, genesis block, and consensus-specific configurations (Ethash, Clique, Parlia).
---

## What I do

Provide the complete public API reference for `alloy::genesis` (v1.8.3). Covers genesis block specification, chain configuration, and per-consensus configs.

## When to use me

Use this skill when you need to define or parse genesis files, configure chain parameters, or set up initial blockchain state.

## Feature Flag

`genesis`

## Structs

| Struct | Description |
|--------|-------------|
| `Genesis` | The genesis block specification |
| `GenesisAccount` | Account in the genesis state |
| `ChainConfig` | Core blockchain settings per block |
| `EthashConfig` | Consensus config for PoW networks |
| `CliqueConfig` | Consensus config for Clique |
| `ParliaConfig` | Consensus config for Parlia (BSC) |
