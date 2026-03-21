# Changelog

All notable changes to Kinetic Wallet will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.0.0-testnet] — 2026-03-21

### Added
- `KineticVault.sol` — non-custodial vault with spending limits, router approval, and nonce-based replay protection
- `KineticBridge.sol` — cross-chain bridge contract with relayer model and refund mechanism
- `IKineticVault` and `IKineticBridge` interfaces
- SDK v1 with `KineticWallet`, `KineticBridgeClient` TypeScript classes
- Full TypeScript type definitions
- Hardhat config with Sepolia, Base Sepolia, Polygon Amoy, Avalanche Fuji
- Deployment scripts with automatic contract verification
- GitHub Actions CI: lint, test, security scan, gas report
- Testnet deployments on Sepolia and Base Sepolia

### Notes
- **TESTNET ONLY** — contracts are not audited
- All SDK methods that require real cryptography are stubbed with `TODO` comments
- Mainnet launch planned for Q4 2026 after audit

---

## Upcoming

### [1.1.0] — April 2026
- Cross-chain routing engine live
- 16+ network support (AVAX, MATIC, ARB, OP)
- Real BIP-39/BIP-44 key derivation in SDK
- Bridge relayer service deployed

### [1.2.0] — June 2026
- Portfolio tracker API
- Tax export (CSV/PDF)
- iOS + Android beta

### [2.0.0] — Q4 2026
- Mainnet launch
- Formal audit complete
- Hardware wallet (Ledger, Trezor) support
- SDK v2 with React hooks
