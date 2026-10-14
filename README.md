# Mint Contract

A Hardhat-based smart contract project for a configurable ERC20 minting system. The project includes a minting contract that accepts one ERC20 as payment and mints another ERC20 to the caller at a predefined exchange rate, along with deployment and test utilities for local and Rinkeby-based workflows.

## Overview

This repository demonstrates a token minting flow where:

- a user deposits a supported buy token into the contract,
- the contract calculates the mintable token amount using the configured rate,
- the mintable token is minted to the user,
- the contract owner can pause minting, update the rate, or withdraw the buy token balance.

The main contract is implemented in [contracts/MintContract.sol](contracts/MintContract.sol), and the project is configured for Hardhat development and deployment.

## Features

- Configurable minting rate via `mintRate`
- Owner-controlled pause functionality via `pause(bool)`
- Token address updates for the mintable and buy tokens
- ERC20 withdrawal for the contract owner
- Deployment and testing scripts for Hardhat
- Etherscan verification support

## Contract Behavior

The `MintContract` contract exposes the following core logic:

- `mint(uint256 _amount)`: transfers the configured buy token from the caller to the contract and mints the equivalent value of the mintable token
- `valueOf(uint256 _amount)`: returns the mintable token amount that will be minted for a given input amount
- `pause(bool _state)`: enables or disables minting, restricted to the owner
- `setRate(uint256 _newRate)`: updates the minting rate
- `setBuyToken(address _newBT)`: changes the token used for payment
- `setMintableToken(address _newMT)`: changes the token being minted
- `withdrawECR20()`: transfers the contract's buy-token balance to the owner

## Project Structure

- [contracts/MintContract.sol](contracts/MintContract.sol) — main minting contract
- [contracts/mocks/MT.sol](contracts/mocks/MT.sol) — mock mintable token
- [contracts/mocks/USDT.sol](contracts/mocks/USDT.sol) — mock stablecoin-like token
- [scripts/deploy.js](scripts/deploy.js) — deployment script
- [test/sample-test.js](test/sample-test.js) — contract tests
- [hardhat.config.js](hardhat.config.js) — Hardhat configuration
- [package.json](package.json) — scripts and dependencies

## Prerequisites

Before running the project, make sure you have:

- Node.js 16+ recommended
- npm or yarn
- A local Ethereum node or access to a supported test network
- A funded wallet private key for deployment
- An Etherscan API key if you plan to verify contracts

## Installation

```bash
npm install
```

## Configuration

Create a `.env` file in the project root with the deployment values required by [hardhat.config.js](hardhat.config.js):

```bash
PK_DEPLOYER=your_private_key_here
SCANKEY=your_etherscan_api_key
```

The Hardhat config is set up for:

- `localhost`
- `hardhat`
- `rinkeby`

## Available Scripts

```bash
npm run test
npm run compile
npm run clean
npm run deploy
```

### Script Notes

- `npm run test` runs the Hardhat test suite.
- `npm run compile` compiles the Solidity contracts.
- `npm run deploy` runs the deployment script against the configured `rinkeby` network.

## Deployment

1. Update the token addresses in [scripts/deploy.js](scripts/deploy.js) to match your deployment targets.
2. Ensure your `.env` file contains the required deployment and verification values.
3. Run the deployment script:

```bash
npm run deploy
```

> The current deployment configuration targets the `rinkeby` network, so the command assumes your environment is prepared for that network.

## Testing

Run the project tests with:

```bash
npm run test
```

The test suite covers deployment and minting flow behavior, including:

- verifying ownership and token setup,
- approving the buy token for the contract,
- minting the target token through the contract,
- confirming the expected mint amount.

## Example Workflow

A typical usage flow is:

1. Deploy the mintable token and buy token contracts.
2. Set the mintable token vault to the `MintContract` address if required by the token implementation.
3. Approve the buy token transfer allowance for the mint contract.
4. Call `mint(amount)` with the buy token amount you want to convert.
5. The contract transfers the buy token and issues the corresponding mintable token balance.

## Security Notes

This project is a development-oriented smart contract example and should be reviewed carefully before production use. Key areas to verify in a production deployment include:

- token implementation compatibility,
- access control and ownership assumptions,
- rate calculation correctness,
- reentrancy and transfer safety,
- ownership withdrawal logic and emergency controls.

## License

This project is licensed under the ISC License, as defined in [package.json](package.json).

---

Built with Hardhat and OpenZeppelin patterns for token minting workflows.