---
name: alloy-sol-macro
description: Use the alloy sol! macro to generate Rust bindings from Solidity — structs, enums, events, errors, user-defined types, ABI decoding, and derive attributes.
---

## What I do

Provide patterns for using alloy's `sol!` macro to generate type-safe Rust bindings from Solidity definitions.

## When to use me

Use this skill when you need to define Solidity types in Rust, decode ABI data, generate event/error bindings, or customize derives on generated types.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/sol-macro/examples/`

### Structs and Enums (`structs_enums.rs`)
- Simplest `sol!` usage: define a struct and enum
```rust
sol! {
    enum Bar { A, B, C }
    struct Foo { uint256 a; uint64 b; Bar c; }
}
let foo = Foo { a: U256::from(1), b: 2, c: Bar::A };
```

### Events and Errors (`events_errors.rs`)
- Full contract with events, errors, and functions
- Deploy, watch events via streams, catch custom-error reverts
```rust
sol! {
    #[sol(rpc, bytecode = "...")]
    contract MyContract {
        eventValueChanged(uint256 new_value);
        error Unauthorized();
        function setValue(uint256 v) external;
    }
}
// Watch events
let stream = contract.ValueChanged_filter().watch().await?.into_stream();
// Catch errors
let err = contract.setValue(1).call().await.unwrap_err();
let decoded = err.as_decoded_error::<MyContract::Unauthorized>();
```

### Decode Returns (`decode_returns.rs`)
- Decode raw hex return payload into typed structs
```rust
sol! { function getRoundData(uint80) returns (uint80, int256, uint256, uint256, uint80); }
let data = hex!("...");
let result = getRoundDataCall::abi_decode_returns(&data)?;
```

### User Defined Types (`user_defined_types.rs`)
- Custom value types, type aliases, generic arrays, tuples
```rust
sol! {
    type CustomType is uint256;
    type Bytes32Alias is bytes32;
}
let val = CustomType::from(U256::from(42));
let encoded = val.abi_encode();
```

### All Derives (`all_derives.rs`)
- `#![sol(all_derives)]` adds `Default`, `Debug`, `PartialEq`, `Eq`, `Hash`

### Extra Derives (`extra_derives.rs`)
- Combine `all_derives` with `extra_derives(serde::Serialize, serde::Deserialize)` for Serde support
