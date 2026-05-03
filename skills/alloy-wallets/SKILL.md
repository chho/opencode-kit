---
name: alloy-wallets
description: Work with alloy wallets and signers — private key, mnemonic, keystore, hardware wallets (Ledger, Trezor, Yubi), cloud KMS (AWS, GCP), multi-signer wallets, message signing, and EIP-712 permits.
---

## What I do

Provide patterns for creating and using alloy wallet signers: local, hardware, cloud KMS, and multi-signer aggregation.

## When to use me

Use this skill when you need to create a signer, sign messages, verify signatures, set up hardware wallet signers, use cloud KMS, or aggregate multiple signers into one wallet.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/wallets/examples/`

### Private Key Signer (`private_key_signer.rs`)
- Create signer from raw private key, build provider with wallet, send tx
```rust
let signer: PrivateKeySigner = anvil.keys()[0].clone().into();
let provider = ProviderBuilder::new().wallet(signer).connect_http(url);
```

### Mnemonic Signer (`mnemonic_signer.rs`)
- Derive from BIP-39 mnemonic phrase with derivation path and index
```rust
let signer = MnemonicBuilder::<English>::default()
    .phrase("word1 word2 ...")
    .index(0)?
    .build()?;
let random_signer = MnemonicBuilder::<English>::default().build_random()?;
```

### Keystore Signer (`keystore_signer.rs`)
- Decrypt Web3 Secret Storage JSON keystore file
```rust
let signer = LocalSigner::decrypt_keystore(keystore_path, "password")?;
```

### Create Keystore (`create_keystore.rs`)
- Encrypt private key into a keystore JSON file
```rust
LocalSigner::encrypt_keystore(path, &mut rng, private_key, "password")?;
```

### Sign Message (`sign_message.rs`)
- Sign arbitrary bytes, recover address from signature
```rust
let sig = signer.sign_message(b"hello").await?;
let recovered = sig.recover_address_from_msg(b"hello")?;
```

### Verify Message (`verify_message.rs`)
- Synchronous sign via `SignerSync`, then recover and verify
```rust
let sig = signer.sign_message_sync(b"hello")?;
assert_eq!(sig.recover_address_from_msg(b"hello")?, signer.address());
```

### Sign Permit Hash (`sign_permit_hash.rs`)
- EIP-712 typed data signing: define struct, build domain, compute hash, sign
```rust
sol! { struct Permit { address owner; address spender; uint256 value; uint256 nonce; uint256 deadline; } }
let domain = eip712_domain! { name: "MyToken", version: "1", chain_id: 1, verifying_contract: addr };
let hash = permit.eip712_signing_hash(&domain);
let sig = signer.sign_hash(hash).await?;
```

### AWS KMS Signer (`aws_signer.rs`)
- Create signer from AWS KMS key
```rust
let signer = AwsSigner::new(kms_client, key_id, chain_id).await?;
```

### GCP KMS Signer (`gcp_signer.rs`)
- Create signer from Google Cloud KMS key ring

### Ledger Signer (`ledger_signer.rs`)
- Connect to Ledger hardware wallet via `LedgerSigner::new(HDPath::LedgerLive)`

### Trezor Signer (`trezor_signer.rs`)
- Connect to Trezor hardware wallet via `TrezorSigner::new(HDPath::TrezorLive)`

### Yubi Signer (`yubi_signer.rs`)
- Connect to YubiHSM device via USB

### Ethereum Wallet (`ethereum_wallet.rs`)
- Aggregate multiple heterogeneous signers (local, Ledger, AWS) into one `EthereumWallet`
- Route transactions by specifying `from` field
```rust
let mut wallet = EthereumWallet::new(default_signer);
wallet.register_signer(ledger_signer);
wallet.register_signer(aws_signer);
let provider = ProviderBuilder::new().wallet(wallet).connect_http(url);
```
