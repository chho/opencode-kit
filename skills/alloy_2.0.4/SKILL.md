---
name: alloy
description: Alloy 2.0.4 Rust library reference for connecting applications to Ethereum-based blockchains. Covers providers, signers, contract interaction, transports, fillers, sol! macro, primitives, and RPC types. Load this when writing Rust code that interacts with Ethereum, sending transactions, querying chain state, or encoding/decoding Solidity types.
---

# Alloy 2.0.4 — Ethereum Blockchain Library

Alloy connects Rust applications to Ethereum-based blockchains. It is a rewrite of ethers-rs with async-first design.

- **Docs:** https://docs.rs/alloy/2.0.4/alloy/
- **Book:** https://alloy.rs/
- **Examples:** https://github.com/alloy-rs/examples
- **MSRV:** Rust 1.91
- **License:** Apache-2.0 OR MIT

---

## Installation

```toml
# Full feature set
alloy = { version = "2", features = ["full"] }

# For ProviderBuilder::connect_anvil* helpers
alloy = { version = "2", features = ["provider-anvil-node"] }

# For alloy::node_bindings re-export only
alloy = { version = "2", features = ["node-bindings"] }

# Both full and node-bindings (full does NOT include node-bindings)
alloy = { version = "2", features = ["full,node-bindings"] }
```

---

## Crate Structure

| Module | Crate | Description |
|---|---|---|
| `alloy::consensus` | alloy-consensus | Ethereum consensus interface |
| `alloy::contract` | alloy-contract | Interact with on-chain contracts |
| `alloy::dyn_abi` | alloy-dyn-abi | Dynamic Solidity type encoder |
| `alloy::eips` | alloy-eips | EIP implementations |
| `alloy::ens` | alloy-ens | ENS utilities |
| `alloy::genesis` | alloy-genesis | Genesis file definitions |
| `alloy::json_abi` | alloy-json-abi | Full JSON-ABI implementation |
| `alloy::network` | alloy-network | Network abstraction for RPC types |
| `alloy::node_bindings` | alloy-node-bindings | Execution-layer client bindings (Anvil, Geth, Reth) |
| `alloy::primitives` | alloy-primitives | Low-level types: Address, U256, I256, FixedBytes, Bytes, etc. |
| `alloy::providers` | alloy-provider | Interface with an Ethereum blockchain |
| `alloy::pubsub` | alloy-pubsub | JSON-RPC publish-subscribe tower service |
| `alloy::rlp` | alloy-rlp | Ethereum RLP encoding |
| `alloy::rpc` | alloy-rpc-client + alloy-rpc-types | JSON-RPC client and types |
| `alloy::serde` | alloy-serde | Serde utilities |
| `alloy::signers` | alloy-signer + backends | Ethereum signer abstraction and implementations |
| `alloy::sol_types` | alloy-sol-types | Solidity type modeling, ABI and EIP-712 codec |
| `alloy::transports` | alloy-transport + backends | Transport abstraction (HTTP, WS, IPC) |

---

## Providers

### Building a Provider

```rust
use alloy::providers::{Provider, ProviderBuilder};

// Recommended fillers (gas estimation, nonce, chain ID) — auto-enabled with new()
let provider = ProviderBuilder::new()
    .connect("https://eth.llamarpc.com")
    .await?;

// With wallet (local signer)
let provider = ProviderBuilder::new()
    .wallet(signer)
    .connect("http://localhost:8545")
    .await?;

// Custom network (e.g. Optimism)
let provider = ProviderBuilder::new_with_network::<Optimism>()
    .connect("https://mainnet.optimism.io")
    .await?;

// HTTP only (no async connect)
let provider = ProviderBuilder::new()
    .connect_http("https://eth.llamarpc.com".parse().unwrap());

// Type-erased provider
let provider: DynProvider = ProviderBuilder::new()
    .connect("http://localhost:8080")
    .await?
    .erased();
```

### ProviderBuilder Key Methods

| Method | Description |
|---|---|
| `ProviderBuilder::new()` | Create with recommended fillers |
| `ProviderBuilder::default()` | Create with no fillers |
| `.disable_recommended_fillers()` | Opt out of recommended fillers |
| `.with_recommended_fillers()` | Add recommended fillers manually |
| `.wallet(signer)` | Add a WalletFiller for local signing |
| `.filler(f)` | Add a custom TxFiller |
| `.layer(l)` | Add a tower layer |
| `.with_gas_estimation()` | Add gas estimation filler |
| `.with_nonce_management(m)` | Add nonce management filler |
| `.with_simple_nonce_management()` | Simple nonce filler |
| `.with_cached_nonce_management()` | Cached nonce filler |
| `.with_eip1559_estimator(e)` | Custom EIP-1559 estimator |
| `.with_blob_gas_estimation()` | Blob gas estimation filler |
| `.with_chain(chain)` | Set poll interval based on chain block time |
| `.connect(url)` | Connect via URL (auto-detects HTTP/WS/IPC) |
| `.connect_http(url)` | Connect HTTP transport |
| `.connect_ws(url)` | Connect WebSocket transport |
| `.connect_ipc(path)` | Connect IPC transport |
| `.on_provider(provider)` | Build on existing provider |
| `.on_builtin(url)` | Connect to built-in transport |

### Provider Trait — Core Methods

**Chain Info:**
- `get_chain_id()` — chain ID
- `get_block_number()` — latest block number
- `get_block_number_by_id(block_id)` — block number for BlockId
- `get_gas_price()` — current gas price in wei
- `get_blob_base_fee()` — blob base fee per blob gas
- `get_fee_history(count, last_block, percentiles)` — historical gas info
- `estimate_eip1559_fees()` — EIP-1559 fee estimation
- `syncing()` — sync status

**Account:**
- `get_balance(address)` — ETH balance (defaults to latest block)
- `get_transaction_count(address)` — nonce for address
- `get_code_at(address)` — bytecode at address
- `get_storage_at(address, key)` — storage slot value
- `get_proof(address, keys)` — Merkle proofs
- `get_account(address)` — TrieAccount info
- `get_account_info(address)` — Account info

**Blocks:**
- `get_block(block_id)` — block by hash/tag/number (add `.full()` for tx bodies)
- `get_block_by_hash(hash)` — block by hash
- `get_block_by_number(number)` — block by number
- `get_block_receipts(block_id)` — all receipts in a block
- `get_header(block_id)` — block header
- `get_header_by_hash(hash)` — header by hash
- `get_header_by_number(number)` — header by number

**Transactions:**
- `send_transaction(tx)` — broadcast transaction → PendingTransactionBuilder
- `send_raw_transaction(bytes)` — broadcast raw RLP
- `send_tx_envelope(envelope)` — broadcast envelope
- `send_transaction_sync(tx)` — send + wait for receipt (EIP-7966)
- `get_transaction_by_hash(hash)` — transaction by hash
- `get_transaction_receipt(hash)` — receipt by hash
- `get_raw_transaction_by_hash(hash)` — raw EIP-2718 encoded tx

**Calls & Estimation:**
- `call(tx)` — eth_call (no broadcast), returns EthCall builder
- `call_many(bundles)` — eth_callMany
- `estimate_gas(tx)` — gas estimation
- `multicall()` — Multicall3 builder
- `simulate(payload)` — simulate transactions on top of state
- `create_access_list(request)` — EIP-2930 access list

**Filters & Logs:**
- `get_logs(filter)` — query logs
- `new_filter(filter)` / `new_block_filter()` / `new_pending_transactions_filter(full)` — create filters
- `get_filter_changes(id)` — poll filter
- `uninstall_filter(id)` — remove filter
- `watch_blocks()` — poll new block hashes → stream
- `watch_full_blocks()` — poll + fetch full blocks → stream
- `watch_headers()` — poll new headers → stream
- `watch_pending_transactions()` — poll pending tx hashes → stream
- `watch_full_pending_transactions()` — poll full pending txs → stream
- `watch_logs(filter)` — poll logs matching filter → stream

**Subscriptions (pubsub — WS/IPC only):**
- `subscribe_blocks()` — new block headers stream
- `subscribe_full_blocks()` — full block bodies stream
- `subscribe_pending_transactions()` — pending tx hashes stream
- `subscribe_full_pending_transactions()` — full pending txs stream
- `subscribe_logs(filter)` — log stream matching filter
- `subscribe(params)` / `subscribe_to(method)` — generic subscriptions
- `unsubscribe(id)` — cancel subscription

**Raw RPC:**
- `raw_request(method, params)` — typed raw JSON-RPC
- `raw_request_dyn(method, params)` — type-erased raw JSON-RPC

---

## Fillers (Transaction Auto-Fill)

Fillers automatically populate missing transaction fields before sending.

| Filler | Description |
|---|---|
| `GasFiller` | Populates gas limit and gas price/maxFeePerGas |
| `NonceFiller` | Fills nonces using a NonceManager |
| `ChainIdFiller` | Populates chain ID |
| `BlobGasFiller` | Fills max_fee_per_blob_gas for blob txs |
| `WalletFiller` | Signs transactions locally |
| `JoinFill<L, R>` | Compose two fillers (L runs before R) |

**Nonce Managers:**
- `SimpleNonceManager` — fetches tx count for each new account
- `CachedNonceManager` — caches nonces in memory

**Control Flow:**
- `FillerControlFlow::Ready` — filler is ready, no work needed
- `FillerControlFlow::Missing` — filler has work to do

---

## Contract Interaction

### sol! Macro — Type-Safe ABI Bindings

```rust
use alloy::sol;
use alloy::primitives::{Address, U256};

sol! {
    #[sol(rpc)]
    #[sol(bytecode = "0x...")]
    contract ERC20 {
        function balanceOf(address owner) external view returns (uint256);
        function transfer(address to, uint256 amount) external returns (bool);
        event Transfer(address indexed from, address indexed to, uint256 value);
    }
}

// Deploy (if bytecode provided)
let contract = ERC20::deploy(&provider, ()).await?;

// Connect to existing contract
let contract = ERC20::new(token_address, &provider);

// Read (eth_call)
let balance = contract.balanceOf(owner).call().await?._0;

// Write (send transaction)
let receipt = contract.transfer(to, amount).send().await?.watch().await?;
```

### sol! Without RPC — Pure Type Definitions

```rust
sol! {
    struct MyStruct {
        uint256 a;
        bytes32 b;
        address[] c;
    }

    function myFunc(uint256 x, bool flag) external returns (bytes32 result);

    event MyEvent(uint256 indexed value, string message);

    error InsufficientBalance(uint256 available, uint256 required);

    type MyValueType is uint256;
}
```

### CallBuilder

- `contract.funcName(args)` → returns `CallBuilder`
- `.call()` — eth_call (read, no broadcast)
- `.send()` — broadcast as transaction
- `.value(amount)` — attach ETH value
- `.from(address)` — set from address
- `.gas(limit)` — set gas limit
- `.block(id)` — set block ID for call

### Event Queries

```rust
let filter = ERC20::Transfer::new().from(from_addr).to(to_addr);
let logs = filter.watch().await?.into_stream().take(10);
// or
let logs = filter.query().await?;
```

### EIP-712 Signing

```rust
use alloy::sol_types::eip712_domain;

let domain = eip712_domain!(name: "MyDomain", version: "1");
let signing_hash = my_struct.eip712_signing_hash(&domain);
```

---

## Signers

| Module | Crate | Description |
|---|---|---|
| `signers::local` | alloy-signer-local | Private key, keystore, mnemonic, YubiHSM |
| `signers::aws` | alloy-signer-aws | AWS KMS |
| `signers::gcp` | alloy-signer-gcp | GCP KMS |
| `signers::ledger` | alloy-signer-ledger | Ledger hardware wallet |
| `signers::trezor` | alloy-signer-trezor | Trezor hardware wallet |
| `signers::turnkey` | alloy-signer-turnkey | Turnkey |

### Core Traits

- `Signer` — async Ethereum signer (sign transactions, messages, typed data)
- `SignerSync` — sync Ethereum signer

### Local Signer Example

```rust
use alloy::signers::local::PrivateKeySigner;

let signer: PrivateKeySigner = "0x...".parse()?;
let provider = ProviderBuilder::new()
    .wallet(signer)
    .connect("http://localhost:8545")
    .await?;
```

### Web3 Signer (Remote Signing)

```rust
// Uses eth_signTransaction on the remote node
let web3_signer = Web3Signer(provider.clone());
```

---

## Transports

| Transport | Module | Feature |
|---|---|---|
| HTTP | `transports::http` | `transport-http` |
| WebSocket | `transports::ws` | `transport-ws` |
| IPC | `transports::ipc` | `transport-ipc` |

**Auto-detection:** `ProviderBuilder::new().connect(url)` auto-detects the transport:
- `http://` / `https://` → HTTP
- `ws://` / `wss://` → WebSocket
- Path starting with `/` or `.//` → IPC

**Transport Trait:** `Transport` — manages JSON-RPC request/response lifecycle.

---

## Primitives

Core types from `alloy_primitives`:

| Type | Description |
|---|---|
| `Address` | 20-byte Ethereum address (EIP-55 checksum) |
| `U256`, `U128`, `U64` | Unsigned integers |
| `I256` | Signed 256-bit integer |
| `FixedBytes<N>` | Fixed-size byte array |
| `B256` / `B512` | 32-byte / 64-byte hash |
| `Bytes` | Dynamic byte vector |
| `Log<DATA>` | Ethereum log |

**Compile-time macros:** `address!("0x...")`, `b256!("0x...")`, `fixed_bytes!("0x...")`, `bytes!("0x...")`

```rust
use alloy::primitives::{address, b256, U256, Address, Bytes};

let addr = address!("0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045");
let hash = b256!("0x1234...");
let amount = U256::from(1_000_000);
```

---

## RPC Types

Organized by namespace:

| Module | Namespace |
|---|---|
| `alloy-rpc-types-eth` | `eth_*` |
| `alloy-rpc-types-admin` | `admin_*` |
| `alloy-rpc-types-debug` | `debug_*` |
| `alloy-rpc-types-engine` | `engine_*` |
| `alloy-rpc-types-trace` | `trace_*` |
| `alloy-rpc-types-txpool` | `txpool_*` |
| `alloy-rpc-types-beacon` | Beacon Node API |
| `alloy-rpc-types-mev` | MEV bundle |
| `alloy-rpc-types-anvil` | Anvil dev node |
| `alloy-rpc-types-tenderly` | Tenderly node |

---

## Network Abstraction

Alloy is network-generic. The default network is `Ethereum`.

```rust
// Use with custom networks via op-alloy, etc.
let provider = ProviderBuilder::new_with_network::<Optimism>()
    .connect(rpc_url)
    .await?;
```

**Key Traits:**
- `Network` — captures type info for network-specific RPC types
- `WalletProvider` — provider containing a wallet
- `ProviderLayer` — tower-style layer for providers

---

## Common Patterns

### Send ETH

```rust
let tx = TransactionRequest::default()
    .to(recipient)
    .value(U256::from(1e18 as u64));
let pending = provider.send_transaction(tx).await?;
let receipt = pending.watch().await?;
```

### Watch for Confirmations

```rust
let tx_hash = provider.send_transaction(tx)
    .await?
    .with_required_confirmations(2)
    .with_timeout(Some(Duration::from_secs(60)))
    .watch()
    .await?;
```

### Multicall

```rust
let results = provider.multicall()
    .add(contract.balanceOf(addr_1))
    .add(contract.balanceOf(addr_2))
    .call()
    .await?;
```

### Subscribe to New Blocks (WebSocket)

```rust
use futures::StreamExt;
let sub = provider.subscribe_blocks().await?;
let mut stream = sub.into_stream().take(5);
while let Some(header) = stream.next().await {
    println!("new block: {header:#?}");
}
```

### Poll Logs (HTTP Alternative to Subscriptions)

```rust
use futures::StreamExt;
let filter = Filter::new()
    .address(token_addr)
    .event_signature(transfer_sig);
let poller = provider.watch_logs(&filter).await?;
let mut stream = poller.into_stream().flat_map(futures::stream::iter).take(5);
while let Some(log) = stream.next().await {
    println!("{log:#?}");
}
```

---

## no_std Support

Limited to these crates: `alloy-eips`, `alloy-genesis`, `alloy-serde`, `alloy-consensus`. Most network crates require `std`.
