---
name: alloy-api-pubsub
description: Alloy 1.8.3 pubsub module API — Ethereum JSON-RPC publish-subscribe service with subscriptions, streams, connection handles, and frontend transport.
---

## What I do

Provide the complete public API reference for `alloy::pubsub` (v1.8.3). Covers the pubsub transport layer for WebSocket subscriptions and notification streams.

## When to use me

Use this skill when you need low-level pubsub transport types, subscription management, or connection handle interfaces. Users typically interact via the `providers` module.

## Feature Flag

`pubsub`

## Structs

| Struct | Description |
|--------|-------------|
| `PubSubFrontend` | Transport composed of channel to running pubsub service |
| `RawSubscription` | Feed of notifications from server (untyped) |
| `Subscription<T>` | Typed feed of notifications |
| `SubscriptionStream<T>` | Stream yielding only expected types |
| `SubResultStream` | Stream of notifications by local ID |
| `SubAnyStream` | Stream of notifications (may yield unexpected types) |
| `ConnectionHandle` | Handle to backend (communicates with `ConnectionInterface`) |
| `ConnectionInterface` | Reciprocal of `ConnectionHandle` |
| `InFlight` | In-flight JSON-RPC request |

## Enums

| Enum | Description |
|------|-------------|
| `PubSubInstruction` | Instructions for the pubsub service |
| `SubscriptionItem<T>` | Typed subscription item (expected or serialized) |

## Traits

| Trait | Description |
|-------|-------------|
| `PubSubConnect` | Configuration objects with connection details for a backend |
