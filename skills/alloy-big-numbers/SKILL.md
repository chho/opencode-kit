---
name: alloy-big-numbers
description: Work with alloy U256 big numbers — creation, math operations, unit conversion, and comparison. Provides reference patterns from the alloy_examples big-numbers examples.
---

## What I do

Provide patterns for working with alloy's `U256` big number type, covering creation, math, unit conversion, and comparison.

## When to use me

Use this skill when you need to construct `U256` values, perform arithmetic on them, convert between wei/ether/gwei units, or compare large numbers in an Ethereum context using alloy.

## Examples Reference

Source: `~/Dev/solana/alloy_examples/examples/big-numbers/examples/`

Run with: `cargo run --example <name>` (inside the `big-numbers` example crate)

### Creating Instances (`create_instances.rs`)
- Construct `U256` from Rust primitives (`u8`, `u16`, `u32`, `u64`, `u128`)
- Parse from string via `FromStr`
- Use `parse_units` to attach decimal precision
```rust
use alloy::primitives::{U256, utils::parse_units};
let from_u64 = U256::from(100u64);
let from_str = "1000000000000000000".parse::<U256>()?;
let from_units = parse_units("1.5", 18)?; // 1.5 * 10^18
```

### Comparison & Equivalence (`comparison_equivalence.rs`)
- Standard operators: `==`, `<`, `<=`, `>`, `>=`
- Zero check: `U256::is_zero()`
```rust
assert!(a > b);
assert!(U256::ZERO.is_zero());
```

### Conversion (`conversion.rs`)
- Convert `U256` to native types: `.to::<u128>()`, `.to::<u64>()`
- Human-readable strings: `format_units(value, 18)`
```rust
let val: u128 = my_u256.to::<u128>();
let readable = format_units(my_u256, 18)?;
```

### Math Operations (`math_operations.rs`)
- Standard arithmetic: `+`, `-`, `*`, `/`, `%`
- Exponentiation: `.pow(U256::from(2))`
- Fixed-point multiplication pattern (multiply two 18-decimal values, divide by 10^18)
```rust
let result = (a * b) / U256::from(10).pow(U256::from(18));
```

### Math Utilities (`math_utilities.rs`)
- `parse_units("1.5", "ether")` — human-readable to wei
- `format_units(wei, "gwei")` — wei to human-readable
- Supports unit names: "ether", "gwei", "wei", or integer decimals
