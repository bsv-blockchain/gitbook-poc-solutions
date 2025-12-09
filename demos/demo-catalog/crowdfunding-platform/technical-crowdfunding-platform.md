# Crowdfunding Platform - Technical Documentation

**Demo ID**: `demo-2025-014`
**Version**: `1.0.0`
**Last Updated**: `December 2025`

## Architecture Overview

### System Architecture

```
┌─────────────────────────────────────────┐
│         Frontend (Next.js/React)        │
│  - WalletClient for user payments       │
│  - createAction for transactions        │
│  - internalizeAction for token claiming │
│  - listOutputs for token viewing        │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│         Backend (Next.js API)           │
│  - Auth middleware (BRC-103)            │
│  - Payment middleware (BRC-103/104)     │
│  - Wallet Toolbox for server wallet     │
│  - PushDrop token creation              │
│  - JSON file state persistence          │
└─────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────┐
│           BSV Blockchain                │
│  - Investment transactions              │
│  - Individual token claim transactions  │
└─────────────────────────────────────────┘
```

### Technology Stack
- **Frontend**: Next.js, React, TypeScript, @bsv/sdk (WalletClient)
- **Backend**: Next.js API Routes, TypeScript, @bsv/sdk (Wallet Toolbox)
- **Blockchain**: BSV Blockchain
- **Middleware**: @bsv/payment-express-middleware
- **State Persistence**: JSON file storage
- **Infrastructure**: Node.js, npm

### Key Components

1. **Frontend Wallet Integration**: React hooks for WalletClient connection and transaction management
2. **402 Payment Protocol**: HTTP payment flow with auth and payment middleware
3. **BRC-29 Key Derivation**: Secure payment addressing with protocol ID [2, '3241645161d8']
4. **Backend Wallet**: Wallet Toolbox implementation for server-side operations
5. **Token Distribution**: Individual PushDrop token claiming pattern
6. **State Management**: JSON file persistence keyed by wallet identity

## Integration & APIs

### External Dependencies

| Service | Purpose | Version | Documentation |
|---------|---------|---------|---------------|
| @bsv/sdk | BSV blockchain operations | Latest | [BSV SDK Docs](https://docs.bsvblockchain.org/) |
| @bsv/wallet-toolbox | Backend wallet management | Latest | [Wallet Toolbox](https://fast.brc.dev/) |
| @bsv/payment-express-middleware | Payment protocol middleware | Latest | [Middleware Docs](https://docs.bsvblockchain.org/) |

### API Endpoints

#### GET /api/wallet-info
Returns backend wallet identity key for BRC-29 derivation.

#### POST /api/invest
Submit investment using 402 payment flow. First request returns 402 with derivation parameters. Second request with x-bsv-payment header completes the transaction.

#### GET /api/status
Returns campaign progress, goal, raised amount, and investor list.

#### POST /api/complete
Individual token claiming endpoint. Creates and sends PushDrop token to investor when goal is reached.

## Implementation Guide

### Prerequisites
- Node.js (v18 or higher)
- npm package manager
- BSV Desktop Wallet installed
- Environment variables configured in .env

### Setup Instructions

```bash
# Clone repository
git clone https://github.com/bsv-blockchain-demos/crowfunding-workshop-demo
cd crowfunding-workshop-demo

# Install dependencies
npm install

# Setup backend wallet (creates private key and funds with 10,000 sats)
npm run setup

# Start the application
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

### Configuration

Key configuration parameters in `.env`:

```
PRIVATE_KEY=<backend-wallet-private-key>
STORAGE_URL=https://storage.babbage.systems
NETWORK=main
```

## Key Implementation Patterns

### 1. Frontend Wallet Integration

```typescript
import { useState, useEffect } from 'react'
import { WalletClient } from '@bsv/sdk'

export const useWallet = () => {
  const [wallet, setWallet] = useState<WalletClient | null>(null)
  const [identityKey, setIdentityKey] = useState<string | null>(null)

  async function initWallet() {
    const w = new WalletClient()
    const { publicKey } = await w.getPublicKey({ identityKey: true })
    setWallet(w)
    setIdentityKey(publicKey)
  }

  useEffect(() => { initWallet() }, [])
  return { wallet, identityKey }
}
```

### 2. 402 Payment Flow

1. Frontend sends POST to `/api/invest`
2. Backend responds with 402 and derivation prefix
3. Frontend derives payment key using BRC-29
4. Frontend creates transaction with `createAction`
5. Frontend resubmits with `x-bsv-payment` header
6. Backend validates and internalizes payment

### 3. BRC-29 Key Derivation

```typescript
const brc29ProtocolID: WalletProtocol = [2, '3241645161d8']

const { publicKey: derivedPublicKey } = await wallet.getPublicKey({
  counterparty: backendIdentityKey,
  protocolID: brc29ProtocolID,
  keyID: `${derivationPrefix} ${derivationSuffix}`,
  forSelf: false  // Deriving for payee
})

const lockingScript = new P2PKH().lock(
  PublicKey.fromString(derivedPublicKey).toAddress()
).toHex()
```

### 4. Payment Middleware

```typescript
import { createPaymentMiddleware, createAuthMiddleware } from '@bsv/payment-express-middleware'
import { wallet } from '../src/wallet'

export const BRC29_PROTOCOL_ID: [number, string] = [2, '3241645161d8']
export const DERIVATION_PREFIX = 'crowdfunding'

export async function getPaymentMiddleware() {
  return createPaymentMiddleware({
    wallet,
    calculateRequestPrice: calculateInvestmentPrice
  })
}
```

### 5. Individual Token Claiming

```typescript
// Backend creates token for investor
const pushdrop = new PushDrop(wallet)
const lockingScript = await pushdrop.lock(
  [ciphertext],
  [0, 'token list'],
  '1',
  identityKey  // investor's identity key
)

const result = await wallet.createAction({
  description: `Create token: ${tokenDescription}`,
  outputs: [{
    lockingScript: lockingScript.toHex(),
    satoshis: 1,
    basket: 'crowdfunding',
    outputDescription: 'Crowdfunding token'
  }],
  options: { randomizeOutputs: false }
})

// Frontend internalizes received token
await wallet.internalizeAction({
  tx: data.tx,
  outputs: [{
    outputIndex: 0,
    protocol: 'basket insertion',
    insertionRemittance: { basket: 'crowdfunding' }
  }],
  description: 'Internalize crowdfunding token'
})
```

### 6. State Persistence

```typescript
interface StoredData {
  walletIdentity: string
  crowdfunding: CrowdfundingState
}

export function loadCrowdfundingData(walletIdentity: string): CrowdfundingState {
  if (existsSync(DATA_FILE)) {
    const data = readFileSync(DATA_FILE, 'utf-8')
    const stored: StoredData = JSON.parse(data)
    if (stored.walletIdentity === walletIdentity) {
      return stored.crowdfunding
    }
  }
  return defaultState
}
```

## Testing & Validation

### Validation Criteria
- [ ] Frontend wallet connects successfully to BSV Desktop Wallet
- [ ] 402 payment flow completes with BRC-29 key derivation
- [ ] Backend internalizes investment transactions correctly
- [ ] Campaign progress updates in real-time
- [ ] Token claiming works when goal is reached
- [ ] State persists across server restarts
- [ ] Multiple investors can claim tokens individually

## Performance & Scalability

### Current Metrics
- **Transaction Cost**: ~1-2 satoshis per investment (BSV network fees)
- **Token Distribution**: Individual claiming pattern distributes costs
- **State Storage**: JSON file persistence for lightweight operation
- **Response Time**: Sub-second for API endpoints

### Scalability Considerations
- Individual token claiming reduces backend transaction size
- JSON file storage suitable for single-server deployments
- Can migrate to database for multi-server environments
- BRC-29 derivation enables deterministic payment addressing

## Project Structure

```
crowdfunding-project/
├── pages/
│   ├── index.tsx           # Main UI
│   ├── tokens.tsx          # Token viewer
│   └── api/
│       ├── invest.ts       # Investment endpoint with payment middleware
│       ├── complete.ts     # Individual token claiming
│       ├── status.ts       # Campaign status
│       └── wallet-info.ts  # Backend wallet identity
├── src/
│   ├── wallet.ts           # Backend wallet initialization
│   ├── setupWallet.ts      # Setup script for backend wallet
│   └── types.ts            # TypeScript type definitions
├── lib/
│   ├── wallet.ts           # Frontend wallet hook
│   ├── middleware.ts       # Auth & payment middleware
│   ├── crowdfunding.ts     # Crowdfunding state management
│   └── storage.ts          # JSON file persistence
└── .env                    # Configuration
```

## Maintenance & Support

### Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| 402 payment fails | Incorrect BRC-29 derivation | Verify protocol ID matches middleware |
| Token claiming fails | Goal not reached | Check campaign status endpoint |
| State not persisting | File permissions | Verify write access to project directory |
| Wallet connection fails | BSV Desktop Wallet not running | Start BSV Desktop Wallet application |

## Important Concepts

### randomizeOutputs: false
Critical for payment middleware. The middleware needs to know the exact output index for transaction internalization.

### P2PK vs P2PKH
PushDrop tokens use P2PK (Pay-to-Public-Key), not P2PKH. Script contains raw public key and cannot be found by searching address on explorer.

### Individual Token Claiming Pattern
Unlike batch distributions, each investor claims their token individually:
1. Reduces backend transaction fees (spread across investors)
2. Gives investors control over when they claim
3. Prevents single large transaction failure from blocking all tokens

## Resources

- **Repository**: [GitHub - crowfunding-workshop-demo](https://github.com/bsv-blockchain-demos/crowfunding-workshop-demo)
- **Business Documentation**: [business-crowdfunding-platform.md](business-crowdfunding-platform.md)
- **BSV SDK Documentation**: [docs.bsvblockchain.org](https://docs.bsvblockchain.org/)
- **Wallet Toolbox**: [fast.brc.dev](https://fast.brc.dev/)

---
*For business context and use cases, see: [Business Documentation](business-crowdfunding-platform.md)*
