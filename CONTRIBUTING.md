# Contributing to Kinetic Wallet

Thank you for your interest in contributing! ⚡

## Getting Started

```bash
git clone https://github.com/kineticwalletdotfun/koneticwallet.git
cd koneticwallet
npm install
cp .env.example .env
npm test
```

## Workflow

1. Fork the repo and create a feature branch
2. Branch naming: `feat/description`, `fix/description`, `docs/description`
3. Write tests for any new logic
4. Ensure all tests pass: `npm test`
5. Commit using [Conventional Commits](https://www.conventionalcommits.org/)
6. Open a Pull Request against `develop`

## Commit Convention

```
feat: add polygon amoy support
fix: correct nonce increment in vault
docs: update SDK bridge example
test: add bridge refund test case
chore: bump hardhat to 2.22
```

## Pull Request Checklist

- [ ] Tests added/updated
- [ ] `npm test` passes
- [ ] `npm run lint` passes
- [ ] Docs updated if needed
- [ ] No `.env` or private keys committed

## Code Style

- Solidity: follow the existing NatSpec comment style
- TypeScript: strict mode, no `any`
- Use `TODO:` comments for stubs that need real implementation

## Questions?

Open a Discussion or join our [Discord](https://discord.gg/kineticwallet).
