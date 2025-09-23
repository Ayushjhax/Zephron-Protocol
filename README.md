## Zephron Protocol

Programmable, collateralized GOLD on Solana + a user-friendly Digital Portfolio Platform.

- **MVP GitHub**: [link-placeholder]
- **MVP Demo Video**: [link-placeholder]

> Add your media later:
> - System design images: place in `docs/` and embed below
> - Architecture diagram: place in `docs/` and embed below
> - Demo screenshots/GIFs: place in `docs/` and embed below


### Badges
- **License**: MIT
- **Chain**: Solana (Anchor)
- **Frontend**: Next.js (Turborepo Monorepo)


## Table of Contents
- [Vision](#vision)
- [Problem](#problem)
- [Solution Overview](#solution-overview)
  - [Digital Portfolio Platform](#digital-portfolio-platform)
  - [Programmable Gold Protocol](#programmable-gold-protocol)
- [Core Mechanics](#core-mechanics)
  - [Deposit & Mint](#deposit--mint)
  - [Redeem & Burn](#redeem--burn)
  - [Liquidation](#liquidation)
  - [Risk Variables](#risk-variables)
- [Devnet Deployments](#devnet-deployments)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Clone & Install](#clone--install)
  - [Environment Variables](#environment-variables)
  - [Run the Frontend](#run-the-frontend)
  - [Build & Test the Program](#build--test-the-program)
  - [Deploy to Devnet](#deploy-to-devnet)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)
- [Acknowledgements](#acknowledgements)


## Vision
For millions of households, gold is the primary store of wealth—but remains a dead, illiquid asset in physical form. Zephron Protocol creates a trusted, instant digital bridge that transforms static gold into a liquid, programmable asset for borrowing, spending, and participating in modern finance.


## Problem
- Physical gold offers true ownership but suffers from high custodial costs, poor liquidity, slow T+2 settlement, and minimal utility.
- The market is bifurcated between physical gold and fragmented digital experiences. Users face a hard trade-off between trust and utility.


## Solution Overview
Zephron is a vertically integrated, two-pronged ecosystem:

### Digital Portfolio Platform
- An intuitive on-ramp to convert INR to tokenized gold with banking-style UX.
- Provides asset management, deposits, and collateralized lending.

### Programmable Gold Protocol
- On-chain, transparent, and decentralized layer that makes gold value programmable.
- Users collateralize assets to mint a GOLD-pegged digital asset, unlocking liquidity without selling holdings.
- Lending and solvency are governed by strict on-chain mathematics and real-time oracles.

> [Architecture Diagram Placeholder]
>
> Embed later: `docs/architecture.png`


## Core Mechanics
### Deposit & Mint
Lock SOL collateral into a PDA vault and mint GOLD (a synthetic asset pegged to gold) against it, maintaining safe over-collateralization.

### Redeem & Burn
Repay GOLD and burn it to unlock and withdraw the corresponding amount of SOL collateral.

### Liquidation
If a position’s Health Factor (HF) falls below the minimum threshold, it becomes eligible for permissionless liquidation. Liquidators burn GOLD to repay debt and receive SOL collateral plus a bonus.

### Risk Variables
- **L**: Lamports of SOL locked as collateral
- **D_gold**: Amount of GOLD minted (debt, 9 decimals)
- **P_sol_usd**: SOL/USD price from Pyth
- **P_gold_usd**: GOLD/USD price from Pyth
- **HF**: Health Factor (collateral value vs. debt value)
- **HF_min**: Minimum HF before liquidation (e.g., 1.0)
- **Bonus%**: Liquidation bonus (e.g., 10%)

> [System Design Placeholder]
>
> Embed later: `docs/system-design-protocol.png`


## Devnet Deployments
- **Program ID**: `EGtHEv1xJP3aA3fT5JVB7H2UXoR6s7rB6iYjkifDqdvQ`
- **Key Transactions**:
  - Initialize: `https://explorer.solana.com/tx/5nQmjPPLXavsWMMmTauj6Lo23QkC9pRG1WUK8HpAdBWdyJQnUQHtm4w1VCKwY2vUhXwQWLtMF5wyqasFT4EKBXQ5?cluster=devnet`
  - Deposit & Mint: `https://explorer.solana.com/tx/5DGJHuLAW1fq85Pgp9eA9cJiW1E75dUpoxqLZHMvpL2Y2rFqZU92y1YpqJHDiHLegu2dk1PFuRNs2FM5px9YvXFh?cluster=devnet`
  - Redeem & Burn: `https://explorer.solana.com/tx/4PGQosQtyKnyXhh4MBoAoW3aLuQnAjk8BREk1P5QEYvfTeRk8yykVB5w4y6cS4aDGxajGJxw3J87pwAnwZiPHg9v?cluster=devnet`
  - Update Config: `https://explorer.solana.com/tx/4ggxS1Ktsim19wxw76LbVADA9huHUs3TN8SQaYSZMBLxDinRCZQ7LDchWc5kwH5P9e7qnoCZeqDnP376xAGhtpHs?cluster=devnet`
  - Liquidate: `https://explorer.solana.com/tx/224iqrkKs8aaQ5PZrax6MsFthH5Sxt3WzXJPpKfYYkYRrqkM6JaDj85wnc2odaQgcdnUXwTDXgNptB6tffXKQmbg?cluster=devnet`
- **Pyth Oracles**:
  - GOLD/USD PriceUpdateV2: `https://explorer.solana.com/address/2uPQGpm8X4ZkxMHxrAW1QuhXcse1AHEgPih6Xp9NuEWW?cluster=devnet`
  - SOL/USD PriceUpdateV2: `https://explorer.solana.com/address/7UVimffxr9ow1uXYxsr4LHAcV58mLzhmwaeKvJ1pjLiE?cluster=devnet`

> [Demo Video Placeholder]
>
> Embed later: `docs/demo.mp4`


## Repository Structure
```
/Frontend
  apps/
    buyer/             # Next.js app (user portfolio & flows)
  packages/
    central/           # Shared atoms/hooks
    db/                # Prisma client & migrations
    ui/                # UI library components
/program
  programs/gold/       # Anchor program for GOLD protocol
  tests/               # Anchor mocha tests
  target/idl/          # IDL output
```


## Getting Started
### Prerequisites
- Node.js 18+
- npm or pnpm
- Rust toolchain (stable)
- Solana CLI (`solana --version`)
- Anchor CLI (`anchor --version`)

### Clone & Install
```bash
# Clone
git clone https://github.com/zephron-labs/zephron-protocol.git
cd zephron-protocol

# Install (root)
npm install

# Install workspace deps if needed
cd Frontend/apps/buyer && npm install
```

### Environment Variables
Create environment files and fill in secrets.

- Frontend `Frontend/apps/buyer/.env.local`:
```
NEXT_PUBLIC_SOLANA_CLUSTER=devnet
NEXTAUTH_SECRET=your_secret
# Payment providers (optional for demo)
RAZORPAY_KEY_ID=
RAZORPAY_KEY_SECRET=
STRIPE_SECRET_KEY=
``` 

- Program: usually no env file required. Ensure Solana CLI is set to devnet and you have a funded keypair.

```bash
solana config set --url https://api.devnet.solana.com
solana airdrop 2 # on devnet
```

### Run the Frontend
```bash
cd Frontend/apps/buyer
npm run dev
# App: http://localhost:3000
```

### Build & Test the Program
```bash
cd program
npm install
anchor build
npm test
```

### Deploy to Devnet
```bash
cd program
anchor deploy
# Verify IDL at target/idl/gold.json
```


## Roadmap
- Expand collateral types beyond SOL
- Oracle redundancy and failover
- Governance and parameter management
- Integrate automated keepers for liquidations
- Mobile-first portfolio app


## Contributing
Contributions are welcome! Please:
- Open an issue to discuss substantial changes
- Submit focused PRs with clear descriptions
- Follow existing code style and linting

> [Contribution Guide Placeholder] — add `CONTRIBUTING.md` later if desired.


## Security
- Do not use in production without a formal audit
- Report vulnerabilities privately via issues marked as security or contact maintainers
- Protocol depends on oracle integrity (Pyth) and proper risk parameters


## License
MIT — see [LICENSE](./LICENSE).


## Acknowledgements
- Pyth Network for reliable on-chain price feeds
- Solana and Anchor contributors
- The broader open-source community
