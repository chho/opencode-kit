---
name: alloy-api-providers
description: Alloy 1.8.3 providers module API — Ethereum blockchain interface with ProviderBuilder, Provider trait, MulticallBuilder, PendingTransaction, EthCall, DynProvider, fillers, layers, and subscriptions.
---

## What I do

Provide the complete public API reference for `alloy::providers` (v1.8.3). Covers connecting to Ethereum nodes, building providers with fillers/layers, making RPC calls, batching, multicall, and watching transactions.

## When to use me

Use this skill when you need the exact type/trait/method names for provider construction, RPC methods, multicall patterns, transaction watching, or provider layering.

## Feature Flag

`providers`

## Sub-modules

| Module | Description |
|--------|-------------|
| `bindings` | Multicall3 bindings |
| `ext` | Extended provider APIs |
| `fillers` | Transaction fillers (gas, nonce, chain, wallet) |
| `layers` | Layer implementations |
| `network` | Network types |
| `transport` | Transport types |
| `utils` | Provider utilities |

## Key Structs

| Struct | Description |
|--------|-------------|
| `ProviderBuilder<L, F, P>` | Build a `Provider` from layers and fillers |
| `RootProvider<P>` | Base provider managing RPC client + heartbeat |
| `DynProvider` | Type-erased provider wrapper |
| `EthCall<P, B>` | Builder for `eth_call` request |
| `EthCallMany<P>` | Builder for `eth_callMany` request |
| `MulticallBuilder<D, P, N>` | Multicall3 aggregation builder |
| `CallBuilder<D, P>` | Single call item for multicall |
| `CallItem<D>` | Mapped call type for aggregate calls |
| `CallItemBuilder<D>` | Helper to build `CallItem` |
| `PendingTransaction` | Transaction awaiting confirmation |
| `PendingTransactionBuilder<P>` | Configure pending tx watcher |
| `PendingTransactionConfig` | Config for watching pending tx |
| `GetSubscription<P, R>` | Subscription request builder |
| `WatchBlocks<P>` | Poll new blocks |
| `WatchHeaders<P>` | Poll new block headers |
| `SubFullBlocks<P>` | Subscribe to full blocks (pubsub) |
| `Stack<Inner, Outer>` | Stack of two providers |
| `Identity` | Identity layer (no-op) |
| `ConnectionConfig` | Built-in transport connection config |
| `WsConnect` | WebSocket connection details |
| `WebSocketConfig` | WebSocket configuration |
| `IpcConnect` | IPC connection object |
| `Web3Signer` | Remote signer via `eth_signTransaction` |
| `Failure` | Multicall failure representation |

## Enums

| Enum | Description |
|------|-------------|
| `SendableTx<T>` | Transaction to send (builder or envelope) |
| `ProviderCall<P, B, R>` | Primary future for `Provider` methods |
| `MulticallError` | Multicall errors |
| `PendingTransactionError` | Pending tx watch errors |
| `WatchTxError` | Heartbeat transaction watch errors |

## Key Traits

| Trait | Description |
|-------|-------------|
| `Provider<N>` | Ethereum JSON-RPC interface — all RPC methods |
| `ProviderLayer<P, N>` | Layering abstraction (tower-like) |
| `WalletProvider<N>` | Provider containing `NetworkWallet` |
| `Network` | Network type info for RPC |
| `CallInfoTrait` | Convert `CallItem` to aggregate call structs |
| `Caller<P, B>` | Convert `EthCall` to `ProviderCall` |
| `MulticallItem` | Type convertible to singular call item |

## Functions

| Function | Description |
|----------|-------------|
| `builder()` | Access `ProviderBuilder` with minimal generics |

## Constants

| Constant | Description |
|----------|-------------|
| `MULTICALL3_ADDRESS` | Default Multicall3 contract address |
| `ARB_SYS_ADDRESS` | ArbSys precompile address (Arbitrum) |

## Type Aliases

| Alias | Description |
|-------|-------------|
| `FilterPollerBuilder<R>` | Polls `eth_getFilterChanges` |
| `Result` | Multicall result type |
| `BoxedFut<T>` | Boxed future for `ProviderCall` |
