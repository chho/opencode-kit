<p align="center">
  <h1 align="center">My OpenCode Config</h1>
  <p align="center">A curated collection of <strong>skills</strong> and <strong>commands</strong> for <a href="https://opencode.ai">OpenCode</a> — supercharging AI-assisted development with domain-specific knowledge.</p>
</p>

---

## Overview

This repository provides a personal OpenCode configuration that equips the AI coding assistant with deep, context-aware expertise across multiple domains. Each **skill** is a self-contained knowledge module that OpenCode can load on-demand to deliver accurate, framework-specific guidance.

## Features

- **51 specialized skills** spanning Rust language, blockchain development, web frameworks, and browser extensions
- **Custom slash commands** for common developer workflows
- **Conventional commit enforcement** with interactive grouping and confirmation
- **Zero external dependencies** — pure configuration, ready to use

---

## Skills

Skills are organized by domain. Each lives in its own directory under `skills/` with a `SKILL.md` file containing structured instructions, code examples, and API references.

### Rust Language

Fundamental to advanced coverage of the [Rust](https://www.rust-lang.org/) programming language — from core syntax to concurrent systems.

| Skill | Description |
|-------|-------------|
| `rust-core` | Language fundamentals — variables, data types, functions, structs, enums, pattern matching |
| `rust-ownership` | Ownership rules, borrowing, references, slices, and lifetime annotations |
| `rust-traits-generics` | Generics, trait bounds, impl Trait, where clauses, associated types, operator overloading |
| `rust-collections-error-handling` | Vec, String, HashMap, and error handling with Result, panic!, and the `?` operator |
| `rust-concurrency-async` | Threads, mpsc channels, Mutex, Arc, Send/Sync, async/await, futures, tokio |
| `rust-advanced` | Smart pointers, closures, iterators, unsafe Rust, macros (declarative & procedural) |
| `rust-project-cargo` | Cargo project setup, testing, dependencies, workspaces, release profiles, publishing |

### Alloy (Rust Ethereum Library)

Comprehensive coverage of the [alloy](https://github.com/alloy-rs/alloy) Rust library for Ethereum development — from primitive types to advanced DeFi patterns.

| Skill | Description |
|-------|-------------|
| `alloy-primitives` | Bytes, addresses, fixed-size byte arrays, keccak256 hashing |
| `alloy-providers` | HTTP, WebSocket, IPC, batch RPC, multicall, mocking |
| `alloy-transactions` | Transaction building, signing, estimation, and sending |
| `alloy-contracts` | Contract deployment and interaction via `sol!` macro |
| `alloy-ens` | ENS name resolution and reverse lookups |
| `alloy-wallets` | Wallet management and key handling |
| `alloy-subscriptions` | Event subscriptions and real-time blockchain data |
| `alloy-queries` | Blockchain data querying patterns |
| `alloy-fillers` | Provider filler layers (gas, nonce, chain ID) |
| `alloy-layers` | Provider layer stacking and composition |
| `alloy-node-bindings` | Anvil and Geth dev node integration |
| `alloy-sol-macro` | `sol!` macro usage for compile-time ABI bindings |
| `alloy-big-numbers` | U256 and arbitrary-precision arithmetic |
| `alloy-comparison` | Alloy vs ethers-rs migration and comparison |
| `alloy-advanced` | AnyNetwork, EIP-712, foundry-fork-db, Uniswap V2 arbitrage |

#### Alloy API Submodules

| Skill | Description |
|-------|-------------|
| `alloy-api-primitives` | Core primitive types API reference |
| `alloy-api-serde` | Serialization and deserialization |
| `alloy-api-rlp` | Recursive Length Prefix encoding |
| `alloy-api-rpc` | RPC types and client |
| `alloy-api-json-abi` | JSON ABI parsing and inspection |
| `alloy-api-dyn-abi` | Dynamic ABI type construction |
| `alloy-api-sol-types` | Solidity type system in Rust |
| `alloy-api-signers` | Signer implementations |
| `alloy-api-providers` | Provider trait and implementations |
| `alloy-api-transports` | Transport layer abstractions |
| `alloy-api-pubsub` | Pub/sub transport for subscriptions |
| `alloy-api-network` | Network-specific type configuration |
| `alloy-api-consensus` | Consensus types (transactions, headers) |
| `alloy-api-contract` | Contract call and instance APIs |
| `alloy-api-eips` | EIP-related type definitions |
| `alloy-api-ens` | ENS API types and resolution |
| `alloy-api-genesis` | Genesis block configuration |
| `alloy-api-node-bindings` | Node binding API reference |

### Astro (Web Framework)

Full-stack documentation for the [Astro](https://astro.build) web framework — from project setup to production deployment.

| Skill | Description |
|-------|-------------|
| `astro-core` | Installation, project structure, components, routing, styling |
| `astro-ssr` | Server-side rendering, actions, middleware, server islands |
| `astro-integrations` | UI framework integrations (React, Vue, Svelte, etc.) |
| `astro-cms` | Headless CMS integration guides |
| `astro-deploy` | Deployment guides for 20+ hosting platforms |
| `astro-backend` | Backend patterns, API endpoints, data fetching |
| `astro-testing-ts` | Testing strategies and TypeScript configuration |
| `astro-extension-api` | Extension API reference |
| `astro-reference` | Full API and configuration reference |

### Other

| Skill | Description |
|-------|-------------|
| `chrome-extension` | Chrome Extension development (Manifest V3) |
| `smart-commit` | Intelligent git commit workflow |

> **Total: 51 skills** (7 Rust · 15 Alloy · 19 Alloy API · 9 Astro · 1 Chrome Extension · 1 Workflow)

---

## Commands

Custom slash commands live in the `commands/` directory.

| Command | Description |
|---------|-------------|
| `/smart-commit` | Analyzes `git diff`, groups changes into logical commits with conventional messages, and commits step-by-step with user confirmation |

---

## Project Structure

```
.
├── commands/                  # Custom slash commands
│   └── smart-commit.md
├── skills/                    # Knowledge modules
│   ├── rust-*/                # Rust language skills
│   ├── alloy-*/               # Alloy (Rust Ethereum) skills
│   ├── astro-*/               # Astro framework skills
│   ├── chrome-extension/      # Chrome Extension development
│   └── smart-commit/          # Smart commit workflow
├── .gitignore
├── LICENSE                    # MIT License
└── README.md
```

> **Note:** The `.opencode/` directory is gitignored and contains the runtime plugin dependency (`@opencode-ai/plugin`).

---

## License

This project is licensed under the [MIT License](LICENSE).
