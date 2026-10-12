# Krypt

A React and Solidity learning project that connects an Ethereum wallet to a transfer interface and a separate on-chain transaction log. The repository name is `Blockchain`; the application is called Krypt.

**Status: source available; current deployment and transaction flow have not been verified.** The checked-in network configuration uses legacy Ropsten settings and needs replacement before the app can be exercised on a current test network.

## What the code demonstrates

- Wallet connection and account/balance reads through MetaMask and Ethers.js.
- React context for form state, loading state and transaction history.
- A wallet request to transfer ETH to a recipient address.
- A Solidity contract that stores sender, recipient, amount, message, timestamp and keyword.
- History rendering with keyword-based GIF lookup.
- React/Vite interface styling with Tailwind CSS.

## How the transfer flow works

```text
React form → MetaMask ETH-transfer request
           → separate contract call to record metadata
           → wait for the contract receipt → refresh history
```

These are **two separate blockchain transactions**. The `Transactions` contract records metadata; it does not execute or verify the ETH transfer. If the recording step fails, the ETH transfer may still have been submitted. The displayed history is a contract log, not an independently verified payment ledger.

## Repository map

| Path | Responsibility |
| --- | --- |
| `client/` | React 17, Vite and Tailwind interface |
| `client/src/context/TransactionContext.jsx` | Wallet interaction and transaction state |
| `client/src/utils/constants.js` | Contract address and ABI configuration |
| `client/src/hooks/useFetch.jsx` | GIF lookup |
| `smart_contract/contracts/Transactions.sol` | Transaction metadata storage |
| `smart_contract/scripts/deploy.js` | Hardhat deployment script |
| `smart_contract/hardhat.config.js` | Legacy deployment configuration |

## Local interface setup

Install Node.js/npm compatible with the repository's older Vite and Hardhat dependencies. From the repository root:

```sh
cd client
npm install
npm run dev
```

This starts the interface only. Wallet/history operations also require a deployed contract, a matching ABI/address and MetaMask connected to the same network. No working contract deployment is claimed here.

## Before exercising wallet operations

1. Replace the legacy network configuration with a local development chain or a supported test network.
2. Remove hard-coded credentials from the deployment configuration and read secrets from ignored environment files. Treat any committed RPC key and wallet private key as exposed; revoke/replace them before reuse. Removing a value from the latest file does not remove it from Git history.
3. Deploy `Transactions.sol`, then update the client contract address and ABI.
4. Use a separate development wallet and test funds.
5. Verify transfer and log failure handling, account/network switching and transaction confirmation states.

## Limits and further work

The current source needs stronger input validation and recovery when wallet requests fail. The smart-contract test file is a scaffold; it is not evidence of coverage for `Transactions.sol`. Dependency upgrades, contract tests and a reproducible test-network demo would make this a stronger portfolio project.

No security audit, production readiness or transaction-volume claim is made.

## License

No root LICENSE file is present. The contract declares `UNLICENSED`; the earlier README's MIT-license statement was unsupported.
