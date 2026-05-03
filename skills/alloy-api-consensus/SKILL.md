---
name: alloy-api-consensus
description: Alloy 1.8.3 consensus module API — Ethereum consensus types including headers, blocks, transactions, EIP-2718 envelopes, receipts, blob sidecars, and transaction type variants (Legacy, EIP-1559, 2930, 4844, 7702).
---

## What I do

Provide the complete public API reference for `alloy::consensus` (v1.8.3). Covers Ethereum EL consensus types: block headers, block bodies, transaction envelopes, receipts, blob sidecars, and signing.

## When to use me

Use this skill when you need the exact struct/enum/trait names, fields, or type relationships for consensus-layer types — building blocks, constructing transaction envelopes, handling blob sidecars, or sealing/ signing transactions.

## Feature Flag

`consensus`

## Sub-modules

| Module | Description |
|--------|-------------|
| `conditional` | Helpers for conditional transactions |
| `constants` | Ethereum protocol-related constants |
| `crypto` | Cryptographic algorithms |
| `error` | Helper errors |
| `extended` | Extended transaction types |
| `proofs` | Merkle proofs and hashes |
| `serde_bincode_compat` | Bincode-compatible serde (feature: `serde` + `serde-bincode-compat`) |
| `transaction` | Transaction types |

## Structs

| Struct | Description |
|--------|-------------|
| `BlobTransactionSidecar` | Set of blobs with commitments and proofs |
| `BlobTransactionSidecarEip7594` | Blobs with commitments and cell proofs (PeerDAS) |
| `Block` | Full Ethereum block |
| `BlockBody` | Response to `GetBlockBodies` |
| `EthereumReceipt<T>` | Typed Ethereum transaction receipt |
| `Header` | Ethereum block header |
| `HeaderInfo` | Essential info extracted from a header |
| `HeaderRoots` | Roots contained in a block header |
| `Receipt<L>` | Receipt with result of transaction execution |
| `ReceiptWithBloom<L>` | Receipt with calculated bloom filter |
| `Receipts` | 2D vector of receipts |
| `Sealed<T>` | Consensus hashable item with memoized hash |
| `SidecarBuilder<C>` | Build `BlobTransactionSidecar` from data |
| `Signed<T, S>` | Transaction with signature and hash seal |
| `SimpleCoder` | Default coder for `SidecarBuilder` (uses last 31 bytes per blob) |
| `TrieAccount` | Account in the account trie |
| `TxEip1559` | Priority fee transaction (EIP-1559) |
| `TxEip2930` | Transaction with access list (EIP-2930) |
| `TxEip4844` | Blob transaction (EIP-4844) |
| `TxEip4844WithSidecar` | Blob transaction with sidecar |
| `TxEip7702` | Set-code transaction (EIP-7702) |
| `TxLegacy` | Legacy transaction |

## Enums

| Enum | Description |
|------|-------------|
| `BlobTransactionSidecarVariant` | Blob sidecar variant (4844 or 7594) |
| `BlobTransactionValidationError` | Blob sidecar validation error (feature: `kzg`) |
| `Eip658Value` | Transaction execution result (success/failure) |
| `EnvKzgSettings` | KZG settings |
| `EthereumTxEnvelope` | Ethereum EIP-2718 transaction envelope |
| `EthereumTypedTransaction` | Typed transaction enum for `EthereumTxEnvelope` |
| `Extended<T, E>` | Combines two transaction types |
| `ReceiptEnvelope` | Receipt envelope (EIP-2718) |
| `TxEip4844Variant` | EIP-4844 variant (with or without sidecar) |
| `TxType` | Transaction type byte |

## Key Traits

| Trait | Description |
|-------|-------------|
| `BlockHeader` | Extract block data from a header |
| `Transaction` | Minimal EVM transaction (dynamic fee vs legacy fee) |
| `TransactionEnvelope` | Typed transaction envelope |
| `SignableTransaction<S>` | A signable transaction |
| `Sealable` | Sealable objects |
| `TxReceipt` | Transaction execution receipt |
| `Typed2718` | Determine EIP-2718 type byte |
| `SidecarCoder` | Strategy for coding data into sidecars |
| `EthBlock` | Trait for Ethereum-like blocks |
| `Eip2718EncodableReceipt` / `Eip2718DecodableReceipt` | EIP-2718 receipt codec |
| `RlpEncodableReceipt` / `RlpDecodableReceipt` | RLP receipt codec |

## Type Aliases

| Alias | Description |
|-------|-------------|
| `TxEnvelope` | Ethereum EIP-2718 transaction envelope (`EthereumTxEnvelope`) |
| `TypedTransaction` | Basic typed transaction (4844 or 4844WithSidecar) |
| `Blob` | Blob as hex string |
| `Bytes48` | Commitment/proof as hex string |

## Constants

| Constant | Description |
|----------|-------------|
| `EMPTY_OMMER_ROOT_HASH` | Ommer root of empty list |
| `EMPTY_ROOT_HASH` | Root hash of empty trie |
