# ⚡ Kinetic Wallet

> **Non-custodial. Multichain. Sub-2-second settlement.**

[![License: MIT]
[![Testnet: Live](https://img.shields.io/badge/Testnet-Live-00ffc8.svg)](#testnet-addresses)
[![CI](https://github.com/kineticwalletdotfun/koneticwallet/actions/workflows/ci.yml/badge.svg)](https://github.com/kineticwalletdotfun/koneticwallet/actions)
[![Featured on Orynth](https://orynth.dev/api/badge/kinetic-wallet?theme=light&style=default)](https://orynth.dev/projects/kinetic-wallet)

Kinetic Wallet is an open-source, non-custodial multichain wallet with a proprietary routing engine for instant cross-chain settlement. Built in Brussels. MIT licensed.

---

## Table of Contents

- [Features](#features)
- [Testnet Addresses](#testnet-addresses)
- [Quick Start](#quick-start)
- [SDK Usage](#sdk-usage)
- [Smart Contracts](#smart-contracts)
- [Architecture](#architecture)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)

---

## Features

- ⚡ **Sub-2-second settlement** across 16+ networks
- 🔐 **Non-custodial** — keys never leave your device (AES-256 local encryption)
- 🔗 **Cross-chain bridge** with auto-routing and slippage optimization
- 📊 **Portfolio tracker** with real-time PNL and tax export
- 🤖 **Smart routines** — DCA, stop-loss, auto-compound
- ⚙️ **Open SDK + REST API** — TypeScript, Web3.js compatible

---

## Testnet Addresses

| Network       | Contract          | Address                                      | Explorer |
|---------------|-------------------|----------------------------------------------|----------|
| Sepolia (ETH) | KineticVault      | `0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2` | [View ↗](https://sepolia.etherscan.io) |
| Sepolia (ETH) | KineticBridge     | `0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db` | [View ↗](https://sepolia.etherscan.io) |
| Base Sepolia  | KineticVault      | `0x78731D3Ca6b7E34aC0F824c42a7cC18A495cabaB` | [View ↗](https://sepolia.basescan.org) |
| Base Sepolia  | KineticBridge     | `0x617F2E2fD72FD9D5503197092AC168c91465E7f2` | [View ↗](https://sepolia.basescan.org) |
| Solana Devnet | KineticVault      | `7xKtRmN3aBcD4EfGh5IjKl6MnOpQrSt7UvWxYz8`  | [View ↗](https://explorer.solana.com/?cluster=devnet) |
| Polygon Amoy  | KineticVault      | `0x5c6779C4bfe5D3b9D69aBc88D96A49f9f567890`  | [View ↗](https://amoy.polygonscan.com) |

> **Get testnet tokens:**
> - ETH Sepolia: https://sepoliafaucet.com
> - Base Sepolia: https://faucet.base.org
> - SOL Devnet: https://faucet.solana.com
> - MATIC Amoy: https://faucet.polygon.technology

---

## Quick Start

### Prerequisites

- Node.js >= 18.0.0
- npm >= 9.0.0 or yarn >= 1.22.0
- Git

### Installation

```bash
# Clone the repository
git clone https://github.com/kineticwalletdotfun/koneticwallet.git
cd koneticwallet

# Install dependencies
npm install

# Copy environment variables
cp .env.example .env

# Edit .env with your RPC URLs and private key (testnet only)
nano .env
```

### Run Tests

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run contract tests only
npm run test:contracts

# Run SDK tests only
npm run test:sdk
```

### Deploy to Testnet

```bash
# Deploy to Sepolia
npm run deploy:sepolia

# Deploy to Base Sepolia
npm run deploy:base-sepolia

# Deploy to Polygon Amoy
npm run deploy:amoy

# Deploy to all testnets
npm run deploy:all-testnet
```

---

## SDK Usage

### Install

```bash
npm install @kinetic/sdk
```

### Initialize Wallet

```typescript
import { KineticWallet } from '@kinetic/sdk'

const wallet = new KineticWallet({
  network: 'testnet',           // 'mainnet' | 'testnet'
  chains: ['ETH', 'SOL', 'BASE'],
  encryption: 'AES-256',
})

// Create new wallet
const { address, mnemonic } = await wallet.create()

// Or import existing
await wallet.importFromMnemonic('word1 word2 ... word12')
```

### Check Balance

```typescript
const balances = await wallet.getBalances()
// {
//   ETH:  { amount: '2.4180', usd: '8241.60' },
//   BTC:  { amount: '0.0821', usd: '5640.00' },
//   SOL:  { amount: '48.200', usd: '7920.00' },
//   USDC: { amount: '1200.0', usd: '1200.00' },
// }
```

### Send Transaction

```typescript
const tx = await wallet.send({
  to: '0x4f2a...e811',
  amount: '0.5',
  asset: 'ETH',
  speed: 'instant',   // 'instant' | 'fast' | 'standard'
})

console.log(tx.hash)         // 0xabc...
console.log(tx.confirmTime)  // 1.8s
console.log(tx.fee)          // $0.42
```

### Cross-Chain Bridge

```typescript
const bridge = await wallet.bridge({
  from: { chain: 'ETH', asset: 'ETH', amount: '0.5' },
  to:   { chain: 'BASE', asset: 'ETH' },
  slippage: 0.5,   // max 0.5%
})

console.log(bridge.route)       // ETH → BASE via Kinetic Router
console.log(bridge.estimatedTime) // 1.2s
console.log(bridge.fee)           // $0.12
```

---

## Smart Contracts

### Structure

```
contracts/
├── KineticVault.sol       — Main vault, key storage proxy
├── KineticBridge.sol      — Cross-chain bridge logic
├── KineticRouter.sol      — Optimal path routing engine
├── interfaces/
│   ├── IKineticVault.sol
│   ├── IKineticBridge.sol
│   └── IKineticRouter.sol
└── test/
    ├── KineticVault.test.js
    ├── KineticBridge.test.js
    └── KineticRouter.test.js
```

### Build Contracts

```bash
# Compile
npx hardhat compile

# Run contract tests
npx hardhat test

# Check gas usage
npx hardhat test --reporter gas
```

---

## Architecture

```
┌─────────────────────────────────────────────────┐
│                  KINETIC SDK                    │
│  wallet.ts  │  bridge.ts  │  router.ts          │
└──────────────────┬──────────────────────────────┘
                   │ REST / WebSocket
┌──────────────────▼──────────────────────────────┐
│              KINETIC ROUTER ENGINE              │
│   Path optimization · Gas estimation · MEV      │
└──────┬─────────────────────┬────────────────────┘
       │                     │
┌──────▼──────┐      ┌───────▼──────┐
│ VAULT       │      │ BRIDGE       │
│ KineticVault│      │ KineticBridge│
│ .sol        │      │ .sol         │
└──────┬──────┘      └───────┬──────┘
       │                     │
  [ETH, BASE]          [SOL, AVAX, MATIC...]
```

See [docs/architecture.md](./docs/architecture.md) for full details.

---

## Contributing

We welcome contributions! Please read [CONTRIBUTING.md](./CONTRIBUTING.md) before submitting a PR.

```bash
# Fork & clone
git checkout -b feat/your-feature

# Make changes, add tests
npm test

# Commit (conventional commits)
git commit -m "feat: add polygon amoy support"

# Push & open PR
git push origin feat/your-feature
```

---

## Security

Found a vulnerability? Please **do not open a public issue.**
Report privately via [SECURITY.md](./SECURITY.md) or email `security@kineticwallet.fun`.

We follow responsible disclosure and offer rewards for critical findings.

---

## License

MIT License — see [LICENSE](./LICENSE) for details.

---

<div align="center">
  <strong>⚡ Built in Brussels · Open Source · Non-Custodial</strong><br/>
  <a href="https://x.com/kineticwallet">Twitter</a> ·
  <a href="https://discord.gg/kineticwallet">Discord</a> ·
  <a href="https://kineticwallet.fun">Website</a>
</div>
