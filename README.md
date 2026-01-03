# DEX AMM Project

## Overview
This project implements a simplified Decentralized Exchange (DEX) using the Automated Market Maker (AMM) model, inspired by Uniswap V2. The DEX allows users to add and remove liquidity, swap between two ERC-20 tokens, and earn trading fees as liquidity providers, all without intermediaries.

The exchange follows a constant product formula (x * y = k) to determine token prices and ensure continuous liquidity.

---

## Features
- Initial and subsequent liquidity provision
- Liquidity removal with proportional share calculation
- Token swaps using constant product formula (x * y = k)
- 0.3% trading fee distributed to liquidity providers
- LP token accounting using internal liquidity mapping
- Fully tested with 25+ Hardhat test cases
- Dockerized environment for consistent execution

---

## Architecture
The project consists of the following components:

- **DEX.sol**  
  Core AMM smart contract that manages liquidity pools, swaps, pricing, and fee distribution.

- **MockERC20.sol**  
  Simple ERC-20 token contract used for testing purposes.

- **DEX.test.js**  
  Comprehensive test suite validating liquidity management, swaps, fees, prices, events, and edge cases.

The DEX contract tracks reserves internally and updates them after each liquidity or swap operation to maintain consistency.

---

## Mathematical Implementation

### Constant Product Formula
The AMM uses the invariant:

x * y = k

yaml
Copy code

Where:
- x = reserve of Token A  
- y = reserve of Token B  
- k = constant value

After each swap, k remains the same or increases slightly due to fees retained in the pool.

---

### Fee Calculation
A 0.3% trading fee is applied to every swap.

Formula used:

amountInWithFee = amountIn * 997
numerator = amountInWithFee * reserveOut
denominator = (reserveIn * 1000) + amountInWithFee
amountOut = numerator / denominator

yaml
Copy code

The fee remains in the pool, increasing total liquidity and benefiting liquidity providers.

---

### LP Token Minting

#### Initial Liquidity
For the first liquidity provider:

liquidityMinted = sqrt(amountA * amountB)

sql
Copy code

The first provider sets the initial price.

#### Subsequent Liquidity
Liquidity must match the existing reserve ratio:

liquidityMinted = (amountA * totalLiquidity) / reserveA

yaml
Copy code

---

## Setup Instructions

### Prerequisites
- Node.js (v18+ recommended)
- Docker & Docker Compose
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
Running Tests Locally (Without Docker)
bash
Copy code
npm install
npx hardhat compile
npx hardhat test
Contract Addresses
Not deployed to a public testnet. Deployment can be done using:

bash
Copy code
npm run deploy
Known Limitations
Supports only a single token pair

No slippage protection (minAmountOut)

No deadline parameter for transactions

No flash swap functionality

These features can be added as enhancements.

Security Considerations
Reentrancy protection using OpenZeppelin’s ReentrancyGuard

Input validation for zero values and insufficient liquidity

Safe ERC-20 transfers via IERC20 interface

Solidity 0.8+ built-in overflow and underflow checks

Reserves updated explicitly to avoid balance desynchronization

Conclusion
This project demonstrates a complete AMM-based DEX with correct mathematical modeling, fee distribution, and extensive test coverage. It provides a strong foundation for understanding DeFi liquidity pools and decentralized trading mechanisms.
```