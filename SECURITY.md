# Security Policy

## Supported Versions

| Version         | Status           |
|-----------------|------------------|
| 1.0.0-testnet   | ✅ Active (testnet only) |
| < 1.0.0         | ❌ Not supported |

> ⚠️ **TESTNET ONLY** — Do not use current contracts with real funds. Contracts are unaudited.

## Reporting a Vulnerability

**Do not open a public GitHub issue for security vulnerabilities.**

### Contact

- **Email:** security@kineticwallet.fun
- **PGP Key:** Available on request
- **Response time:** Within 48 hours

### What to Include

1. Description of the vulnerability
2. Steps to reproduce
3. Potential impact assessment
4. Suggested fix (optional)

### Responsible Disclosure

We follow a **90-day responsible disclosure** policy:
- We acknowledge receipt within 48 hours
- We aim to patch critical issues within 7 days
- We will credit researchers in our changelog (unless anonymity is requested)

### Scope

**In scope:**
- Smart contracts (`KineticVault.sol`, `KineticBridge.sol`)
- SDK cryptographic operations
- Key management / encryption logic
- Bridge relay logic

**Out of scope:**
- Frontend UI bugs
- Third-party dependency vulnerabilities (report upstream)
- Rate limiting / DoS on public APIs

## Bug Bounty

A formal bug bounty program is planned for mainnet launch (Q4 2026).
Critical testnet findings may receive early access rewards at our discretion.
