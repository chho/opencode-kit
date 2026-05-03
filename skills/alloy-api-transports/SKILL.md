---
name: alloy-api-transports
description: Alloy 1.8.3 transports module API — Ethereum JSON-RPC transport abstraction with Transport trait, HTTP/WS/IPC/mock implementations, BoxTransport, DualTransport, error types, and transport layers.
---

## What I do

Provide the complete public API reference for `alloy::transports` (v1.8.3). Covers the transport abstraction layer for Ethereum JSON-RPC communication.

## When to use me

Use this skill when you need transport-level types, error handling, `BoxTransport` for dynamic dispatch, `DualTransport` for fallback, or want to implement custom transports.

## Feature Flag

`transports`

## Sub-modules

| Module | Feature Flag | Description |
|--------|-------------|-------------|
| `http` | `transport-http` | HTTP transport |
| `ws` | `transport-ws` | WebSocket transport |
| `ipc` | `transport-ipc` | IPC transport |
| `mock` | — | Mock transport for testing |
| `layers` | — | Transport layer implementations |
| `utils` | — | Transport building utilities |

## Structs

| Struct | Description |
|--------|-------------|
| `BoxTransport` | Boxed, Clone-able `Transport` trait object |
| `DualTransport<A, B>` | Dispatches to one of two inner transports |
| `HttpError` | HTTP error (e.g. 429 rate limit) |

## Enums

| Enum | Description |
|------|-------------|
| `Authorization` | Auth type: Basic, Bearer, or raw |
| `RpcError<E>` | RPC error (deserialization, transport, etc.) |
| `TransportErrorKind` | Transport error classification |

## Key Traits

| Trait | Description |
|-------|-------------|
| `Transport` | Manages JSON-RPC request/response lifecycle |
| `TransportConnect` | Connection details for a transport |
| `IntoBoxTransport` | Convert to boxed transport |
| `DualTransportHandler<Req>` | Dispatch logic for dual transport |

## Type Aliases

| Alias | Description |
|-------|-------------|
| `TransportResult<T>` | `Result<T, TransportError>` |
| `TransportError` | `RpcError<TransportErrorKind>` |
| `RpcResult<T>` | JSON-RPC request result |
| `BoxFuture<T>` | Pin-boxed future |
| `Pbf<T>` | Pin-boxed future |
| `RpcFut<T>` | RPC-level request future |
| `TransportFut<T>` | Transport-level request future |
