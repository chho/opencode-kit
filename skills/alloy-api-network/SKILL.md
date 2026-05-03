---
name: alloy-api-network
description: Alloy 1.8.3 network module API — network abstraction for RPC types, Ethereum/AnyNetwork types, TransactionBuilder, NetworkWallet, and response type traits.
---

## What I do

Provide the complete public API reference for `alloy::network` (v1.8.3). Covers the network abstraction layer that parameterizes providers, signers, and RPC types by network.

## When to use me

Use this skill when you need to work with network-specific types, implement custom network support, use `TransactionBuilder`, or understand `Ethereum` vs `AnyNetwork` type relationships.

## Feature Flag

`network`

## Sub-modules

| Module | Description |
|--------|-------------|
| `any` | Types for unknown network types |
| `eip2718` | EIP-2718 traits |
| `primitives` | Network primitive types |

## Structs

| Struct | Description |
|--------|-------------|
| `Ethereum` | Types for mainnet-like Ethereum network |
| `EthereumWallet` | Wallet for signing Ethereum transactions |
| `AnyNetwork` | Catch-all network types |
| `AnyHeader` | Block header with optional fields |
| `AnyReceiptEnvelope` | Receipt envelope for any network |
| `AnyRpcBlock` | Block wrapper for unknown types |
| `AnyRpcTransaction` | Transaction wrapper for unknown types |
| `AnyTxType` | Transaction type for catch-all network |
| `UnknownTxEnvelope` | Transaction envelope from unknown network |
| `UnknownTypedTransaction` | Typed transaction of unknown network |
| `UnbuiltTransactionError` | Unbuilt transaction with error |

## Enums

| Enum | Description |
|------|-------------|
| `AnyTxEnvelope` | Transaction envelope for catch-all network |
| `AnyTypedTransaction` | Unsigned transaction for catch-all network |
| `TransactionBuilderError` | Error for transaction builders |

## Key Traits

| Trait | Description |
|-------|-------------|
| `Network` | Type info for network-specific RPC requests/responses |
| `NetworkWallet<N>` | Wallet for signing transactions on a network |
| `IntoWallet<N>` | Convert signer into `NetworkWallet` |
| `TransactionBuilder<N>` | Build transactions for a network |
| `TransactionBuilder4844` | EIP-4844 transaction builder fields |
| `TransactionBuilder7702` | EIP-7702 transaction builder fields |
| `TransactionBuilder7594` | EIP-7594 transaction builder fields |
| `BlockResponse` | Block JSON-RPC response trait |
| `TransactionResponse` | Transaction JSON-RPC response trait |
| `ReceiptResponse` | Receipt JSON-RPC response trait |
| `TxSigner<S>` | Async transaction signer |
| `TxSignerSync<S>` | Sync transaction signer |
| `FullSigner<N, S>` | Combined `Signer` + `TxSigner` |
| `FullSignerSync<N, S>` | Combined `SignerSync` + `TxSignerSync` |

## Type Aliases

| Alias | Description |
|-------|-------------|
| `AnyRpcHeader` | Catch-all header type |
| `AnyTransactionReceipt` | Catch-all receipt type |
| `BuildResult<N>` | Transaction builder result |
