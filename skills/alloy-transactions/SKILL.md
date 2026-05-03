---
name: alloy-transactions
description: Build, sign, send, trace, and decode Ethereum transactions with alloy — ETH/ERC20 transfers, EIP-1559/4844/7702/7594 tx types, gas pricing, access lists, debug tracing, and private transactions.
---

## What I do

Provide comprehensive patterns for building, signing, sending, tracing, and decoding Ethereum transactions using alloy.

## When to use me

Use this skill when you need to send ETH or ERC20 transfers, build typed transactions (legacy, EIP-1559, blob, EIP-7702), trace transactions, decode calldata, or estimate gas costs.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/transactions/examples/`

### Transfer ETH (`transfer_eth.rs`)
- Simplest: send 100 wei from Alice to Bob
```rust
let tx = TransactionRequest::default().with_to(bob).with_value(U256::from(100));
provider.send_transaction(tx).await?.watch().await?;
```

### Transfer ERC20 (`transfer_erc20.rs`)
- Deploy ERC20 on forked Anvil, then call `transfer()`

### Encode/Decode EIP-1559 (`encode_decode_eip1559.rs`)
- Manually construct `TxEip1559`, apply signature, verify hash, recover signer
```rust
let tx = TxEip1559 { to: TxKind::Call(addr), value: U256::from(1), .. };
let signed = tx.into_signed(signature);
let recovered = recover_signer(&signed)?;
```

### Send Legacy Transaction (`send_legacy_transaction.rs`)
- Pre-EIP-1559 with explicit `gas_price` and `nonce`

### Send EIP-1559 Transaction (`send_eip1559_transaction.rs`)
- `max_fee_per_gas` / `max_priority_fee_per_gas` fields

### Send EIP-4844 Blob Transaction (`send_eip4844_transaction.rs`)
- Blob-carrying transaction via `SidecarBuilder`
```rust
let sidecar = SidecarBuilder::<SimpleCoder>::new(&blob_data).build()?;
let tx = TransactionRequest::default().with_blob_sidecar(sidecar);
```

### Send EIP-7594 Transaction (`send_eip7594_transaction.rs`)
- PeerDAS/Osaka format blob transaction

### Send EIP-7702 Transaction (`send_eip7702_transaction.rs`)
- Set code on EOA via authorization list
```rust
let auth = Authorization { chain_id: U256::from(1), address: contract_addr, nonce };
let signed_auth = auth.sign_hash_sync(&signer, &auth.hash())?;
let tx = TransactionRequest::default().with_authorization_list(vec![signed_auth]);
```

### Send Raw Transaction (`send_raw_transaction.rs`)
- Build, sign locally, encode EIP-2718, broadcast raw bytes
```rust
let tx = wallet.sign_transaction(builder).await?;
provider.send_raw_transaction(tx.encoded_2718()).await?;
```

### Send Private Transaction (`send_private_transaction.rs`)
- Send to Flashbots Protect RPC for MEV-protected submission

### Gas Price in USD (`gas_price_usd.rs`)
- Query gas price + Chainlink ETH/USD feed for USD cost

### Decode Input (`decode_input.rs`)
- Decode raw calldata into typed structs via `sol!` + `SolCall::abi_decode`

### Decode Receipt Log (`decode_receipt_log.rs`)
- Decode event logs from transaction receipts

### Access List (`with_access_list.rs`)
- Pre-compute access list for gas savings via `create_access_list`
```rust
let al = provider.create_access_list(tx).await?;
let tx_with_al = tx.with_access_list(al.access_list);
```

### Trace Call (`trace_call.rs`)
- Parity-style `trace_call` for simulated transaction tracing

### Trace Call Many (`trace_call_many.rs`)
- Trace multiple sequential transactions via `trace_call_many`

### Trace Transaction (`trace_transaction.rs`)
- `debug_traceTransaction` with struct log tracer, call tracer, JS tracer

### Debug Trace Call Many (`debug_trace_call_many.rs`)
- Bundle tracing via `debug_traceCallMany` on Reth

### Permit2 Signature Transfer (`permit2_signature_transfer.rs`)
- Gasless ERC20 transfer via Permit2 with EIP-712 signed permit
