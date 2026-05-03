---
name: alloy-api-eips
description: Alloy 1.8.3 eips module API — EIP implementations for block identifiers, EIP-1559 base fee, EIP-2718 envelopes, EIP-2930 access lists, EIP-4844 blob gas, EIP-7702 auth, and 20+ other EIPs.
---

## What I do

Provide the complete public API reference for `alloy::eips` (v1.8.3). Covers block identifiers, EIP-1559 fee calculation, EIP-2718 envelope traits, access lists, blob gas parameters, and many other EIP types.

## When to use me

Use this skill when you need block ID types (`BlockId`, `BlockNumberOrTag`), fee calculation functions, EIP-2718 encoding traits, access list types, blob gas constants, or any EIP-specific type definitions.

## Feature Flag

`eips`

## Sub-modules

| Module | Description |
|--------|-------------|
| `eip1559` | EIP-1559 constants, helpers, types |
| `eip1898` | EIP-1898 block identifiers |
| `eip2124` | EIP-2124 fork filter types |
| `eip2718` | EIP-2718 traits |
| `eip2930` | EIP-2930 access list types |
| `eip2935` | History storage contract (Prague) |
| `eip4788` | EIP-4788 constants |
| `eip4844` | EIP-4844 blob constants/helpers |
| `eip4895` | EIP-4895 withdrawal type |
| `eip6110` | Deposit request types (Prague) |
| `eip7002` | Withdrawal request types (Prague) |
| `eip7251` | Consolidation types (Prague) |
| `eip7594` | PeerDAS types and constants |
| `eip7623` | Calldata cost constants |
| `eip7685` | General purpose EL requests |
| `eip7691` | EIP-7691 constants/utilities |
| `eip7702` | EIP-7702 set-code types |
| `eip7825` | EIP-7825 constants |
| `eip7840` | EIP-7840 blob schedule |
| `eip7892` | EIP-7892 constants/helpers |
| `eip7910` | EIP-7910 implementation |
| `eip7928` | Block-level access lists |
| `merge` | Beacon chain merge constants |

## Structs

| Struct | Description |
|--------|-------------|
| `BlobScheduleBlobParams` | Blob parameters with scheduled updates |
| `NumHash` | Block number and hash pair |
| `RpcBlockHash` | Block hash with optional `requireCanonical` |

## Enums

| Enum | Description |
|------|-------------|
| `BlobScheduleEntry` | Scheduled blob parameter update |
| `BlockId` | Block identifier — hash, number, or tag (EIP-1898) |
| `BlockNumberOrTag` | Block number or tag ("latest", "earliest", "pending") |
| `HashOrNumber` | Either a hash or a block number |

## Traits

| Trait | Description |
|-------|-------------|
| `Decodable2718` | Decode EIP-2718 envelopes |
| `Encodable2718` | Encode EIP-2718 envelopes |
| `Typed2718` | Determine EIP-2718 type byte |

## Functions

| Function | Description |
|----------|-------------|
| `calc_blob_gasprice(excess_blob_gas)` | Blob gas price from header |
| `calc_excess_blob_gas(parent_blob_gas_used, parent_excess_blob_gas)` | Excess blob gas |
| `calc_next_block_base_fee(gas_used, parent_base_fee, parent_gas_limit)` | EIP-1559 next block base fee |

## Type Aliases

| Alias | Description |
|-------|-------------|
| `BlockHashOrNumber` | Block hash or number |
| `BlockNumHash` | Block number and hash |
| `ForkBlock` | Fork block number and hash |
