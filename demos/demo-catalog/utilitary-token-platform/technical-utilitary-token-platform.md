# Utilitary Token Platform - Technical Documentation

**Demo ID**: `utilitary-token-platform`
**Version**: `1.0.0`
**Last Updated**: `2026-02-02`

## Architecture Overview

### System Architecture
Multi-tier architecture with React frontend for token management, Node.js overlay service for transaction validation, MongoDB for UTXO indexing, and blockchain-based token storage following PushDrop (BRC-48) standard.

### Technology Stack
- **Frontend**: React, TypeScript, Vite
- **Backend**: Node.js, Express
- **Database**: MongoDB with indexed queries
- **Blockchain SDK**: @bsv/sdk, @bsv/overlay-tools
- **Infrastructure**: Cloud-hosted overlay service

### Key Components
1. **Topic Manager**: Validates token transactions and enforces balance conservation rules
2. **Lookup Service**: Indexes token UTXOs in MongoDB for fast queries
3. **Frontend Wallet**: WalletClient integration with two-phase signing
4. **MessageBox Integration**: Peer-to-peer token transfer notifications
5. **Token Service**: PushDrop token creation and transfer logic

## Integration & APIs

### External Dependencies
| Service | Purpose | Version | Documentation |
|---------|---------|---------|---------------|
| @bsv/sdk | Transaction and token creation | Latest | docs.bsvblockchain.org |
| @bsv/overlay-tools | Overlay service framework | Latest | github.com/bitcoin-sv/overlay-tools |
| MongoDB | UTXO indexing and storage | 5.0+ | mongodb.com/docs |
| BSV Desktop Wallet | User wallet and signing | Latest | desktop.bsvb.tech |

### API Endpoints

```
POST /submit
POST /lookup
GET /tokens/:tokenId
GET /balance/:identityKey
```

## Implementation Guide

### Prerequisites
- Node.js 18+
- MongoDB 5.0+
- BSV Desktop Wallet installed
- TypeScript 4.9+
- Test BSV for transaction fees

### Setup Instructions

```bash
# Clone repository
git clone https://github.com/sirdeggen/demo-day.git
cd demo-day/tokenization

# Set up overlay service
cd overlay
npm install
npm run build
npm start

# Set up frontend (in new terminal)
cd ../frontend
npm install
npm run dev
```

### Configuration

```
# Overlay service configuration
MONGODB_URI: mongodb://localhost:27017/tokens
PORT: 3000

# Frontend configuration
VITE_OVERLAY_URL: http://localhost:3000
VITE_MESSAGEBOX_HOST: messagebox.babbage.systems
```

## Testing & Validation

### Test Coverage
- **Unit Tests**: Token validation logic, balance conservation
- **Integration Tests**: Overlay admission flow, MongoDB indexing
- **End-to-end Tests**: Mint, transfer, and query operations

### Validation Criteria
- [ ] Overlay accepts valid mint transactions
- [ ] Overlay rejects transfers with invalid balance conservation
- [ ] MongoDB correctly indexes token UTXOs
- [ ] Frontend successfully mints tokens
- [ ] Two-phase signing completes transactions
- [ ] MessageBox delivers transfer notifications
- [ ] Recipient can claim transferred tokens

## Performance & Scalability

### Current Metrics
- **Transaction Validation**: 50-100ms per transaction
- **Database Query**: <50ms for balance lookups
- **Token Mint**: 3-5 seconds (including wallet signing)

### Scalability Considerations
- MongoDB indexes on tokenId and outpoint for fast queries
- Overlay service stateless, can run multiple instances
- UTXO model scales linearly with token supply
- Frontend batching for multiple token operations

## Maintenance & Support

### Monitoring
- **Logs**: Overlay service admission/rejection logs
- **Metrics**: MongoDB collection size, query performance
- **Alerts**: Database connection errors, validation failures

### Troubleshooting
| Issue | Cause | Solution |
|-------|-------|----------|
| Transaction rejected | Balance conservation failed | Check input/output amounts match |
| Tokens not appearing | MongoDB indexing error | Verify overlay admission succeeded |
| Cannot spend tokens | Missing customInstructions | Ensure storage when creating outputs |
| Wallet signing fails | Insufficient BSV | Add funds from faucet |

## Resources

- **Repository**: [github.com/bsv-blockchain-demos/utility-tokens](https://github.com/bsv-blockchain-demos/utility-tokens)
- **Code Tutorial**: [BSV Code Academy - Utilitary Token Platform](https://hub.bsvblockchain.org/bsv-code-academy/intermediate-path/intermediate/utilitary-token-platform)
- **Business Documentation**: [Business Overview](./business-utilitary-token-platform.md)
- **Demo Environment**: Requires overlay + frontend running
- **Support Contact**: BSV blockchain demos team

---
*For business context and ROI details, see: [Business Overview](./business-utilitary-token-platform.md)*
