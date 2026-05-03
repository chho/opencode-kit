---
name: alloy-api-json-abi
description: Alloy 1.8.3 json_abi module API — full Ethereum JSON-ABI implementation with Function, Event, Error, Constructor, Param types, and ABI item iteration.
---

## What I do

Provide the complete public API reference for `alloy::json_abi` (v1.8.3). Covers JSON-ABI parsing and representation: functions, events, errors, constructors, parameters.

## When to use me

Use this skill when you need to parse or construct JSON-ABI objects, iterate over ABI items, convert between JSON-ABI and Solidity source, or work with contract artifact ABIs.

## Feature Flag

`json-abi`

## Sub-modules

| Module | Description |
|--------|-------------|
| `parser` | Solidity type parser |

## Structs

| Struct | Description |
|--------|-------------|
| `JsonAbi` | Full JSON contract ABI |
| `Function` | JSON ABI function |
| `Event` | JSON ABI event |
| `Error` | JSON ABI error |
| `Constructor` | JSON ABI constructor |
| `Fallback` | JSON ABI fallback function |
| `Receive` | JSON ABI receive function |
| `Param` | JSON parameter specification |
| `EventParam` | Solidity event parameter |
| `ContractObject` | Contract ABI + bytecode + deployed bytecode |
| `ToSolConfig` | Config for `JsonAbi::to_sol` |
| `IntoItems` | Iterator over ABI items (owned) |
| `Items` | Iterator over ABI items (borrowed) |

## Enums

| Enum | Description |
|------|-------------|
| `AbiItem` | A JSON ABI item (function/event/error/constructor/fallback/receive) |
| `InternalType` | Contract internal type (Solidity type, struct, enum, contract, etc.) |
| `StateMutability` | Function state mutability (pure/view/nonpayable/payable) |
