# DeFi Lending Protocol

A decentralized lending and borrowing platform where users can supply assets as collateral, earn interest, and borrow against their holdings using smart contracts on Ethereum.

---

## What is DeFi Lending Protocol?

DeFi Lending Protocol is a trustless money market that allows users to supply crypto assets to earn interest or use them as collateral to borrow other assets. Using smart contracts on Ethereum, the platform ensures transparent, automated interest accrual and fair liquidation based on real-time market conditions.

---

## Key Features

### 💰 **Supply & Earn Interest**
- Deposit USDC, ETH, or other supported assets
- Receive lTokens representing your share of the pool
- Earn interest automatically as borrowers pay fees
- Redeem your lTokens anytime for underlying assets + accrued interest

### 🏦 **Collateralized Borrowing**
- Use supplied assets as collateral to borrow other tokens
- Flexible collateral factors (75-80% depending on asset)
- No fixed loan terms - repay whenever you want
- Only pay interest on what you borrow

### 📈 **Dynamic Interest Rates**
- Rates adjust automatically based on supply and demand
- Higher utilization = higher rates for suppliers and borrowers
- Kinked interest rate model optimizes capital efficiency
- Target utilization of 80% for optimal market health

### ⚡ **Risk Management**
- Real-time health factor monitoring
- Automated liquidation when positions become undercollateralized
- 8% liquidation penalty protects lenders
- 50% close factor limits liquidation impact

### 🔒 **Secure & Transparent**
- All logic executed on-chain via audited smart contracts
- 10% reserve factor supports protocol sustainability
- Reentrancy protection and access controls
- Emergency pause functionality

---

## How It Works

1. **Connect Wallet**: Connect your Web3 wallet to the platform
2. **Supply Assets**: Deposit USDC, ETH, or other supported tokens to earn interest
3. **Enable as Collateral**: Authorize your supplied assets to be used as collateral
4. **Borrow**: Take out loans up to your collateral limit
5. **Monitor Health**: Keep your health factor above 1.0 to avoid liquidation
6. **Repay & Redeem**: Repay loans anytime and withdraw your supply + interest

---

## Interest Rate Model

The protocol uses a **kinked interest rate model** to balance supply and demand:

| Parameter | Value | Description |
|-----------|-------|-------------|
| **Base Rate** | 2% | Minimum interest rate when utilization = 0% |
| **Multiplier** | 20% | Rate increase per utilization point up to kink |
| **Jump Multiplier** | 109% | Steep rate increase after kink point |
| **Kink** | 80% | Optimal utilization rate target |

**Formula:**
```
If utilization ≤ 80%:
  Borrow Rate = 2% + (utilization × 20%)

If utilization > 80%:
  Borrow Rate = 2% + (80% × 20%) + ((utilization - 80%) × 109%)
```

---

## Risk Parameters

| Parameter | USDC | ETH | Description |
|-----------|------|-----|-------------|
| **Collateral Factor** | 80% | 75% | Maximum borrowing power per $1 of collateral |
| **Liquidation Threshold** | 85% | 82% | When positions become liquidatable |
| **Liquidation Penalty** | 8% | 8% | Bonus given to liquidators |
| **Reserve Factor** | 10% | 10% | Protocol fee taken from interest |
| **Close Factor** | 50% | 50% | Maximum % of debt liquidatable per transaction |

---

## Example Scenarios

### Conservative Strategy
```
Supply: $1,000 USDC
Borrow Limit: $800 (80% collateral factor)
Safe Borrow: $600 (75% utilization)
Health Factor: 1.33
Liquidation Risk: Low ✅
```

### Aggressive Strategy
```
Supply: $1,000 USDC
Borrow: $790 (98.75% utilization)
Health Factor: 1.01
Liquidation Risk: HIGH ⚠️ (close to liquidation)
```

---

## Project Structure

```
defi-lending-protocol/
├── contracts/          # Solidity smart contracts
│   ├── LToken.sol     # Individual lending markets
│   ├── Comptroller.sol # Risk management & governance
│   └── InterestRateModel.sol # Interest rate calculations
├── src/               # Next.js frontend application
│   ├── components/    # React components
│   ├── config/        # Contract ABIs and addresses
│   └── hooks/         # Wagmi hooks for blockchain interaction
├── script/            # Foundry deployment scripts
└── test/              # Comprehensive test suite
```

---

## Technology Stack

- **Smart Contracts**: Solidity 0.8.20, OpenZeppelin, Foundry
- **Frontend**: Next.js 14, TypeScript, Tailwind CSS
- **Web3 Integration**: Wagmi, Viem, RainbowKit
- **Blockchain**: Ethereum (Sepolia testnet ready)
- **Testing**: Foundry (Forge)

---

## Deployed Contracts (Sepolia Testnet)

### Core Contracts
- **Comptroller**: [`0x0b1c31213d3181fd9b9fd159288f84adb2825e97`](https://sepolia.etherscan.io/address/0x0b1c31213d3181fd9b9fd159288f84adb2825e97)
- **Interest Rate Model**: [`0xa7bc577f2e1ff05bc79eaaed8c907a548b9d70b6`](https://sepolia.etherscan.io/address/0xa7bc577f2e1ff05bc79eaaed8c907a548b9d70b6)
- **Price Oracle**: [`0x313ed33288c24768db927cfb7af0304149f426ff`](https://sepolia.etherscan.io/address/0x313ed33288c24768db927cfb7af0304149f426ff)

### Lending Markets (lTokens)
- **lUSDC**: [`0xbac6bf46b37490ac71f31735e9da3752c5664036`](https://sepolia.etherscan.io/address/0xbac6bf46b37490ac71f31735e9da3752c5664036)
- **lETH**: [`0xfaf79f14f3418d61516a25ce61af4e4b737cf7b8`](https://sepolia.etherscan.io/address/0xfaf79f14f3418d61516a25ce61af4e4b737cf7b8)

### Underlying Assets
- **USDC (Sepolia)**: [`0x94a9D9AC8a22534E3FaCa9F4e7F2E2cf85d5E4C8`](https://sepolia.etherscan.io/address/0x94a9D9AC8a22534E3FaCa9F4e7F2E2cf85d5E4C8)
- **WETH (Sepolia)**: [`0xfFf9976782d46CC05630D1f6eBAb18b2324d6B14`](https://sepolia.etherscan.io/address/0xfFf9976782d46CC05630D1f6eBAb18b2324d6B14)

---

## Getting Started

### Prerequisites
- Node.js 18+
- Foundry ([installation guide](https://book.getfoundry.sh/getting-started/installation))
- Git

### Installation

```bash
# Clone the repository
git clone https://github.com/RobinOppenstam/defi-lending-protocol
cd defi-lending-protocol

# Install dependencies
npm install

# Install Foundry dependencies
forge install
```

### Smart Contract Development

```bash
# Compile contracts
forge build

# Run tests
forge test

# Run tests with detailed output
forge test -vvv

# Generate coverage report
forge coverage

# Deploy to Sepolia testnet
forge script script/Deploy.s.sol --rpc-url sepolia --broadcast --verify
```

### Frontend Development

```bash
# Set up environment variables
cp .env.example .env
# Add your WALLETCONNECT_PROJECT_ID to .env

# Update contract addresses in src/config/contracts.ts after deployment

# Start development server
npm run dev

# Visit http://localhost:3000
```

---

## Core Contracts

### LToken
Individual lending market for each supported asset. Handles supply, borrow, repay, and redeem operations.

### Comptroller
Central risk management contract that tracks collateral, calculates borrow limits, and enforces liquidation rules.

### InterestRateModel
Implements the kinked interest rate curve that adjusts rates based on market utilization.

---

## Testing

The protocol includes comprehensive test coverage:

- ✅ Supply assets and receive lTokens
- ✅ Redeem lTokens for underlying assets
- ✅ Borrow against collateral
- ✅ Repay borrowed assets
- ✅ Interest rate calculations
- ✅ Exchange rate updates over time
- ✅ Liquidation mechanics
- ✅ Access controls and permissions
- ✅ Edge cases and failure modes

```bash
# Run all tests
forge test

# Run specific test file
forge test --match-path test/LToken.t.sol

# Run specific test function
forge test --match-test testSupplyAndRedeem
```

---

## Security Considerations

### Implemented Protections
- ✅ Reentrancy guards on all external calls
- ✅ Integer overflow protection (Solidity 0.8+)
- ✅ Access controls for admin functions
- ✅ Emergency pause functionality
- ✅ Liquidation safeguards

### Known Limitations
- ⚠️ **Price Oracle**: Requires Chainlink integration for production use
- ⚠️ **Governance**: Single owner instead of decentralized governance
- ⚠️ **Audits**: Not professionally audited - testnet only

---

## Roadmap

### Phase 1 ✅ (Complete)
- ✅ Core lending and borrowing functionality
- ✅ Kinked interest rate model
- ✅ Frontend interface with Web3 integration
- ✅ Comprehensive test suite

### Phase 2 🚧 (In Progress)
- 🔲 Chainlink price oracle integration
- 🔲 Liquidation bot implementation
- 🔲 Gas optimization
- 🔲 Additional token markets

### Phase 3 🔮 (Future)
- 🔲 Governance token and DAO
- 🔲 Flash loan functionality
- 🔲 Cross-chain deployment
- 🔲 Advanced analytics dashboard

---

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## ⚠️ Disclaimer

**This is a testnet implementation for educational purposes only.**

Do NOT use with real funds without:
- Professional security audits
- Proper price oracle integration
- Comprehensive testing on mainnet conditions
- Legal and regulatory compliance review

---

## Support

- 📖 [Documentation](#) (Coming soon)
- 🐛 [Report Issues](../../issues)
- 💬 [Join Discord](#) (Coming soon)

---

Built with ❤️ using Foundry, Next.js, and Wagmi
