---
name: alloy-api-ens
description: Alloy 1.8.3 ens module API — ENS name resolution, namehash, forward/reverse lookups, ENS registry/resolver/reverse-registrar contract bindings.
---

## What I do

Provide the complete public API reference for `alloy::ens` (v1.8.3). Covers ENS name resolution, namehash computation, and provider extension for ENS lookups.

## When to use me

Use this skill when you need to resolve ENS names to addresses, perform reverse lookups, compute ENS namehashes, or interact with ENS contracts.

## Feature Flag

`ens`

## Sub-modules (Contract Bindings)

| Module | Description |
|--------|-------------|
| `EnsRegistry` | ENS Registry contract bindings |
| `EnsResolver` | ENS Resolver interface bindings |
| `ReverseRegistrar` | ENS Reverse Registrar contract bindings |

## Enums

| Enum | Description |
|------|-------------|
| `EnsError` | ENS resolution error |
| `NameOrAddress` | ENS name or Ethereum address |

## Traits

| Trait | Description |
|-------|-------------|
| `ProviderEnsExt` | Extension trait on Provider for ENS calls (`resolve_name`, `resolve_address`) |

## Functions

| Function | Description |
|----------|-------------|
| `namehash(name)` | ENS namehash per EIP-137 |
| `reverse_address(addr)` | Reverse-registrar name for an address |

## Constants

| Constant | Description |
|----------|-------------|
| `ENS_ADDRESS` | ENS registry address (`0x00000000000C2E074eC69A0dFb2997BA6C7d2e1e`) |
| `ENS_REVERSE_REGISTRAR_DOMAIN` | Reverse registrar domain (`addr.reverse`) |
