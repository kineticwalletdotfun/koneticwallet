# Testnet Guide

Step-by-step guide to interact with Kinetic Wallet on testnet.

## 1. Prerequisites

- MetaMask or any EVM wallet
- Node.js >= 18 installed
- Testnet ETH (see faucets below)

## 2. Get Testnet Tokens

| Network | Faucet | Amount |
|---|---|---|
| Sepolia ETH | https://sepoliafaucet.com | 0.5 ETH/day |
| Base Sepolia | https://faucet.base.org | 0.1 ETH/day |
| Polygon Amoy | https://faucet.polygon.technology | 1 MATIC/day |
| Solana Devnet | https://faucet.solana.com | 2 SOL/day |

## 3. Connect to Testnet

Add Sepolia to MetaMask:
- Network Name: `Sepolia`
- RPC URL: `https://rpc.sepolia.org`
- Chain ID: `11155111`
- Currency: `ETH`

Add Base Sepolia:
- Network Name: `Base Sepolia`
- RPC URL: `https://sepolia.base.org`
- Chain ID: `84532`
- Currency: `ETH`

## 4. Interact via SDK

```bash
npm install @kinetic/sdk
```

```typescript
import { KineticWallet, KineticBridgeClient } from '@kinetic/sdk'

// Create wallet
const wallet = new KineticWallet({
  network: 'testnet',
  chains: ['ETH', 'BASE'],
  debug: true,
})

const { address, mnemonic } = await wallet.create()
console.log('Address:', address)
// ⚠️ Save your mnemonic securely!

// Check balance
const balances = await wallet.getBalances()
console.log('Balances:', balances)

// Send testnet ETH
const tx = await wallet.send({
  to:     '0xRecipientAddress',
  amount: '0.01',
  asset:  'ETH',
  speed:  'instant',
})
console.log('TX hash:', tx.hash)
console.log('Confirmed in:', tx.confirmTime)

// Bridge ETH → Base Sepolia
const bridge = new KineticBridgeClient({ network: 'testnet' })
const result = await bridge.bridge({
  from: { chain: 'ETH',  asset: 'ETH', amount: '0.005' },
  to:   { chain: 'BASE', asset: 'ETH' },
})
console.log('Bridge ID:', result.bridgeId)
```

## 5. Interact via Hardhat Console

```bash
# Start console connected to Sepolia
npx hardhat console --network sepolia

# Attach to deployed vault
const Vault = await ethers.getContractFactory("KineticVault")
const vault = Vault.attach("0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2")

# Read vault info
const info = await vault.vaultInfo()
console.log(info)

# Activate vault
await vault.activate()
```

## 6. Run Full Test Suite

```bash
git clone https://github.com/kineticwalletdotfun/koneticwallet.git
cd koneticwallet
npm install
cp .env.example .env
# Fill in your SEPOLIA_RPC_URL in .env

npm test
```

## 7. Known Testnet Limitations

- Bridge settlement can take up to 30s on testnet (vs <2s on mainnet targets)
- Relayer is centralized in v1.0-testnet — decentralized relayer network planned for mainnet
- SDK crypto methods are stubbed — real BIP-39/44 implementation lands in v1.1
- No UI for testnet yet — use SDK or Hardhat console

## 8. Reporting Issues

Found a bug? [Open an issue](https://github.com/kineticwalletdotfun/koneticwallet/issues) with:
- Network + contract address
- Transaction hash (if applicable)
- Steps to reproduce
- Expected vs actual behavior
