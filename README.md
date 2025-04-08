# Bitcoin-Stacks Asset Bridge (BSAB) Smart Contract

Enterprise-grade cross-chain bridge protocol for secure asset transfers between Bitcoin and Stacks blockchains.

## Overview

### Key Features

- **Bi-Directional Peg System**: 1:1 asset conversion with proof-of-reserves
- **Validator Consortium**: Multi-sig authorization model with configurable thresholds
- **Dynamic Security Protocols**:
  - Configurable deposit limits ($100 - $100,000 equivalent)
  - 6-block Bitcoin confirmation requirement
  - Real-time balance synchronization
- **Emergency Safeguards**:
  - Global pause functionality
  - Privileged withdrawal mechanism
  - Signature format enforcement

## Technical Specifications

### Environment

- **Smart Contract Language**: Clarity 2.0+
- **Dependencies**: STX-native token standard
- **Consensus**: Federated validator model

### Protocol Parameters

| Constant                 | Value             | Description                  |
| ------------------------ | ----------------- | ---------------------------- |
| `MIN-DEPOSIT-AMOUNT`     | 100,000 sat       | Minimum transfer amount      |
| `MAX-DEPOSIT-AMOUNT`     | 1,000,000,000 sat | Maximum transfer amount      |
| `REQUIRED-CONFIRMATIONS` | 6 blocks          | Bitcoin transaction maturity |

## Architecture

### Core Components

1. **Bridge Controller**

   - Pause/Resume functionality
   - Validator set management
   - Global state monitoring

2. **Deposit Processing**

   - Bitcoin TX validation
   - Confirmations tracking
   - Recipient address whitelisting

3. **Withdrawal System**

   - Balance escrow management
   - BTC address format enforcement
   - Withdrawal request finalization

4. **Signature Engine**
   - ECDSA secp256k1 validation
   - Multi-sig aggregation
   - Replay attack prevention

## Function Reference

### Administrative Functions

- `initialize-bridge`: Activates bridge protocol (Deployer-only)
- `pause-bridge`: Emergency shutdown trigger
- `add-validator`: Registers new verification node

### User Operations

```clarity
(initiate-deposit
  (tx-hash (buff 32))
  (amount uint)
  (recipient principal)
  (btc-sender (buff 33))
)

(confirm-deposit
  (tx-hash (buff 32))
  (signature (buff 65))
)

(withdraw
  (amount uint)
  (btc-recipient (buff 34))
```

### Monitoring Endpoints

- `get-deposit`: Returns deposit status by BTC TX hash
- `get-bridge-balance`: Shows user's bridged assets
- `validator-status`: Checks validator authorization

## Security Model

### Multi-Signature Workflow

1. Deposit initiation with BTC TX proof
2. Validator threshold signature collection
3. Balance escrow update after 6 confirmations
4. Withdrawal authorization via BTC address proof

### Audit Considerations

- All deposits require 2/3 validator approval
- Balance non-repudiation through cryptographic proofs
- Timelock enforcement on withdrawal finality

### Deployment Checklist

1. Verify validator node configurations
2. Initialize bridge controller
3. Set minimum Bitcoin confirmation height
4. Activate balance monitoring
