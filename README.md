# DEX AMM Project

## Overview
This project implements a simplified Decentralized Exchange (DEX) using an Automated Market Maker (AMM) model inspired by Uniswap V2. It allows users to trade two ERC-20 tokens without intermediaries by relying on on-chain liquidity pools and deterministic pricing logic.

The DEX is fully non-custodial, permissionless, and transparent, enabling users to add liquidity, remove liquidity, and perform token swaps while earning trading fees as liquidity providers.

---

## Features
- Initial and subsequent liquidity provision
- Liquidity removal with proportional share calculation
- Token swaps using constant product formula (x * y = k)
- 0.3% trading fee for liquidity providers
- LP token minting and burning
- Event emission for all major actions

---

## Architecture
The system is composed of a single DEX smart contract and two ERC-20 mock tokens used for testing.

- **DEX.sol**
  - Manages liquidity pools
  - Tracks token reserves
  - Handles swaps and fee distribution
  - Maintains LP balances internally using a mapping

- **MockERC20.sol**
  - Simple ERC-20 token
  - Used for testing liquidity and swaps

Liquidity provider shares are tracked internally rather than using a separate LP token contract, keeping the design simple while preserving correctness.

---

## Mathematical Implementation

### Constant Product Formula
The DEX follows the constant product AMM model:

x * y = k

Where:
- `x` = reserve of token A  
- `y` = reserve of token B  
- `k` = constant value  

After every swap, the reserves are updated such that the product `k` never decreases (ignoring rounding).

---

### Fee Calculation
A 0.3% fee is applied to every swap.

Formula used:
amountInWithFee = amountIn * 997
amountOut = (amountInWithFee * reserveOut) / (reserveIn * 1000 + amountInWithFee)

- 99.7% of the input amount is used for price calculation
- 0.3% remains in the pool
- Fees increase pool reserves, benefiting liquidity providers proportionally

---

### LP Token Minting

**Initial Liquidity Provider**
liquidityMinted = sqrt(amountA * amountB)

The first provider sets the initial price ratio.

**Subsequent Liquidity Providers**
liquidityMinted = (amountA * totalLiquidity) / reserveA

Liquidity must be added in the existing pool ratio to preserve pricing.

---

## Setup Instructions

### Prerequisites
- Docker and Docker Compose installed
- Git

---

### Installation

1. Clone the repository:
```bash
git clone <your-repo-url>
cd dex-amm
Contract Addresses

This project is intended for local development and evaluation.
If deployed to a testnet, contract addresses and block explorer links can be added here.
Known Limitations

Supports only a single trading pair

No slippage protection (minimum output parameters)

No deadline or expiration checks

No flash swaps

Integer division rounding may cause minor precision loss
Known Limitations

Supports only a single trading pair

No slippage protection (minimum output parameters)

No deadline or expiration checks

No flash swaps

Integer division rounding may cause minor precision loss