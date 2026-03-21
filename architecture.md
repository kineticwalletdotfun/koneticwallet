# Kinetic Wallet — Architecture

## Overview

Kinetic Wallet is composed of four layers:

```
┌──────────────────────────────────────────────────────┐
│                    CLIENT LAYER                      │
│         Web App · Browser Extension · Mobile         │
│              (React · TypeScript · PWA)              │
└────────────────────┬─────────────────────────────────┘
                     │ @kinetic/sdk
┌────────────────────▼─────────────────────────────────┐
│                     SDK LAYER                        │
│   KineticWallet  ·  KineticBridge  ·  KineticRouter  │
│        Key management lives here (AES-256)           │
└───────────┬─────────────────────┬────────────────────┘
            │ ethers.js / web3.js │ Solana web3.js
┌───────────▼──────────┐ ┌────────▼──────────────────┐
│   ROUTING ENGINE     │ │     BRIDGE RELAYER         │
│  Path optimization   │ │  Off-chain settlement      │
│  Gas estimation      │ │  Multi-chain listener      │
│  MEV protection      │ │  Refund management         │
└───────────┬──────────┘ └────────┬──────────────────┘
            │                     │
┌───────────▼─────────────────────▼──────────────────┐
│                  CONTRACT LAYER                     │
│  KineticVault.sol  ·  KineticBridge.sol             │
│  Deployed on: ETH · BASE · MATIC · AVAX · ARB · OP  │
└─────────────────────────────────────────────────────┘
```

## Key Design Decisions

### Non-Custodial by Default
All private keys are generated and stored locally using AES-256 encryption. The SDK never transmits key material to any server. The smart contracts serve as on-chain identity registries and permission managers — not key custodians.

### Router Pattern
The `KineticVault` contract does not execute transactions directly. Instead, it maintains a whitelist of approved `router` addresses. This allows the routing engine to be upgraded independently of the vault, without moving user funds.

### Bridge Relayer Model
Cross-chain bridging uses an optimistic relayer model:
1. User calls `KineticBridge.bridge()` on source chain → emits `BridgeInitiated`
2. Off-chain relayer listens for events across all chains
3. Relayer calls `settle()` on destination chain bridge contract
4. Relayer calls `markCompleted(bridgeId)` to finalize
5. If settlement fails within timeout, relayer calls `refund()` to return funds

### Spending Limits
Each vault has an optional per-transaction spending limit. This is a soft safety guard for testnet — on mainnet, users can set custom limits per connected dApp.

## Security Model

- Private keys: AES-256 encrypted, stored in browser localStorage / secure enclave (mobile)
- Seed phrases: Never transmitted over the network
- Smart contracts: Nonce-based replay protection on every `execute()` call
- Bridge: All bridge IDs are one-time-use (replay mapping)
- Relayer: Permissioned — only whitelisted relayer address can call `markCompleted` / `refund`

## Contract Upgrade Path

Contracts are **not upgradeable** in v1.0 (no proxy pattern). This is intentional for testnet simplicity and auditability. Mainnet v2.0 will introduce a minimal ERC-1967 transparent proxy for the bridge contract only — the vault will remain immutable.

## Gas Usage (Estimated)

| Operation | Gas (approx) | Cost @ 10 gwei |
|---|---|---|
| Deploy KineticVault | ~380,000 | ~$1.20 |
| Deploy KineticBridge | ~520,000 | ~$1.64 |
| `activate()` | ~28,000 | ~$0.09 |
| `approveRouter()` | ~46,000 | ~$0.15 |
| `execute()` simple transfer | ~55,000 | ~$0.17 |
| `bridge()` | ~72,000 | ~$0.23 |
