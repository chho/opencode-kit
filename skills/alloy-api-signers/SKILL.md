---
name: alloy-api-signers
description: Alloy 1.8.3 signers module API — Ethereum signer abstraction with Signer/SignerSync traits, local/AWS/GCP/Ledger/Trezor/Turnkey backends, and Signature type.
---

## What I do

Provide the complete public API reference for `alloy::signers` (v1.8.3). Covers the signer abstraction trait and all signer backend sub-modules.

## When to use me

Use this skill when you need signer type names, trait signatures, or backend module structure for signing transactions or messages.

## Feature Flag

`signers`

## Sub-modules (Signer Backends)

| Module | Feature Flag | Description |
|--------|-------------|-------------|
| `local` | `signer-local` | Local signer (private key, mnemonic, keystore, YubiHSM) |
| `aws` | `signer-aws` | AWS KMS signer |
| `gcp` | `signer-gcp` | GCP KMS signer |
| `ledger` | `signer-ledger` | Ledger hardware wallet signer |
| `trezor` | `signer-trezor` | Trezor hardware wallet signer |
| `turnkey` | `signer-turnkey` | Turnkey signer |
| `k256` | default | secp256k1 elliptic curve |
| `utils` | default | Signature utility functions |

## Structs

| Struct | Description |
|--------|-------------|
| `Signature` | Ethereum ECDSA signature |

## Enums

| Enum | Description |
|------|-------------|
| `Error` | Generic signer error |
| `Either<L, R>` | General-purpose sum type |
| `UnsupportedSignerOperation` | Unsupported operation error |

## Key Traits

| Trait | Description |
|-------|-------------|
| `Signer` | Async Ethereum signer |
| `SignerSync` | Sync Ethereum signer |

## Macros

| Macro | Description |
|-------|-------------|
| `sign_transaction_with_chain_id` | Utility for chain ID handling in signers |

## Type Aliases

| Alias | Description |
|-------|-------------|
| `Result<T>` | Result type alias for `Error` |
