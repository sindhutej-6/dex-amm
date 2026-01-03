# DEX AMM Project

## Overview
This project implements a simplified Decentralized Exchange (DEX) using an Automated Market Maker (AMM) model similar to Uniswap V2. It enables decentralized token trading without order books or centralized intermediaries. Users can add liquidity, remove liquidity, swap tokens, and earn trading fees as liquidity providers.

The DEX uses the constant product formula (x * y = k) to determine prices and ensure continuous liquidity.

---

## Features
- Initial and subsequent liquidity provision
- Liquidity removal with proportional share calculation
- Token swaps using constant product formula (x * y = k)
- 0.3% trading fee for liquidity providers
- LP token minting and burning via internal liquidity accounting

---

## Architecture
The system consists of the following components:

- **DEX.sol**  
  Core smart contract implementing liquidity pools, swaps, pricing logic, and fee distribution.

- **MockERC20.sol**  
  Simple ERC-20 token contract used for testing purposes.

The DEX contract internally tracks token reserves and liquidity shares. Liquidity providers are issued proportional LP balances, which represent ownership of the pool. All swaps and liquidity actions update reserves explicitly to maintain consistency.

---

## Mathematical Implementation

### Constant Product Formula
The DEX follows the invariant:

x * y = k

yaml
Copy code

Where:
- `x` = reserve of Token A  
- `y` = reserve of Token B  
- `k` = constant product

After every swap, the product of reserves remains constant or increases slightly due to fees retained in the pool.

---

### Fee Calculation
Each swap applies a **0.3% trading fee**.  
Only **99.7%** of the input amount is used in the swap calculation.

Formula used:

amountInWithFee = amountIn * 997
numerator = amountInWithFee * reserveOut
denominator = (reserveIn * 1000) + amountInWithFee
amountOut = numerator / denominator

yaml
Copy code

The deducted fee remains in the pool, increasing total liquidity and benefiting liquidity providers.

---

### LP Token Minting

#### Initial Liquidity
For the first liquidity provider:

liquidityMinted = sqrt(amountA * amountB)

sql
Copy code

The first provider sets the initial price of the pool.

#### Subsequent Liquidity
Liquidity must match the existing reserve ratio.  
LP tokens are minted proportionally:

liquidityMinted = (amountA * totalLiquidity) / reserveA

yaml
Copy code

---

## Setup Instructions

### Prerequisites
- Docker and Docker Compose installed
- Git

---

### Installation

1. Clone the repository:
```bash
git clone https://github.com/sindhutej-6/dex-amm
cd dex-amm
Start Docker environment:

bash
Copy code
docker-compose up -d
Compile contracts:

bash
Copy code
docker-compose exec app npm run compile
Run tests:

bash
Copy code
docker-compose exec app npm test
Check coverage:

bash
Copy code
docker-compose exec app npm run coverage
Stop Docker:

bash
Copy code
docker-compose down
Running Tests Locally (without Docker)
bash
Copy code
npm install
npm run compile
npm test
Contract Addresses
This project is not deployed to a public testnet.
Deployment can be performed using:

bash
Copy code
npm run deploy
Known Limitations
Supports only a single trading pair

No slippage protection (minAmountOut)

No deadline parameter for transactions

No flash swaps

These features can be added as future enhancements.

Security Considerations
Solidity 0.8+ built-in overflow and underflow protection

Input validation for zero values and insufficient liquidity

Explicit reserve tracking to avoid balance desynchronization

Reentrancy-safe design (no external calls before state updates)

Use of OpenZeppelin ERC-20 implementation for token safety

yaml
Copy code

```