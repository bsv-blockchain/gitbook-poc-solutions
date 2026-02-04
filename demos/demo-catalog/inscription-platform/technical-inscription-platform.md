# Inscription Platform - Technical Documentation

**Demo ID**: `inscription-platform`
**Version**: `1.0.0`
**Last Updated**: `2026-02-02`

## Architecture Overview

### System Architecture
Browser-based single-page application connecting directly to user wallets via WalletClient, creating OP_RETURN transactions for data inscription on BSV blockchain.

### Technology Stack
- **Frontend**: React, Vite, TypeScript
- **Blockchain SDK**: @bsv/sdk
- **Wallet Integration**: WalletClient
- **Storage**: Browser localStorage, BSV blockchain

### Key Components
1. **WalletClient Hook**: Manages user wallet connection and identity
2. **InscriptionService**: Creates OP_RETURN transactions with data payloads
3. **HistoryManager**: Persists transaction history in browser localStorage
4. **Basket Manager**: Organizes inscriptions by type (text, json, hash-document, hash-image)

## Integration & APIs

### External Dependencies
| Service | Purpose | Version | Documentation |
|---------|---------|---------|---------------|
| @bsv/sdk | Transaction creation, script building | Latest | docs.bsvblockchain.org |
| BSV Desktop Wallet | User key management, signing | Latest | desktop.bsvb.tech |

### API Endpoints
No external API required - direct blockchain interaction via WalletClient.

## Implementation Guide

### Prerequisites
- Node.js 18+
- BSV Desktop Wallet installed
- Modern web browser with localStorage support
- Test BSV for transaction fees

### Setup Instructions

```bash
# Clone repository
git clone https://github.com/bsv-blockchain-demos/inscription-platform

# Install dependencies
cd inscription-platform
npm install

# Run development server
npm run dev
```

### Configuration
Wallet connection handled automatically via WalletClient. No environment variables required.

## Testing & Validation

### Test Coverage
- **Unit Tests**: Inscription service validation
- **Integration Tests**: WalletClient connection flow
- **Manual Tests**: End-to-end inscription creation

### Validation Criteria
- [ ] Successfully connect to BSV Desktop Wallet
- [ ] Create text inscriptions with OP_RETURN
- [ ] Create JSON inscriptions with validation
- [ ] Generate file hashes and store on-chain
- [ ] Display transaction history from localStorage

## Performance & Scalability

### Current Metrics
- **Transaction Creation**: <1s
- **Wallet Interaction**: 2-3s per signature
- **History Load**: <100ms (50 records)

### Scalability Considerations
- localStorage limited to ~5MB (sufficient for 1000s of records)
- Each inscription creates one blockchain transaction
- No backend infrastructure required

## Maintenance & Support

### Monitoring
- **Logs**: Browser console for client-side debugging
- **Metrics**: Transaction IDs viewable on blockchain explorers
- **Alerts**: User-facing error messages for wallet connection issues

### Troubleshooting
| Issue | Cause | Solution |
|-------|-------|----------|
| Wallet not connecting | BSV Desktop Wallet not installed/unlocked | Install wallet or unlock |
| Transaction fails | Insufficient BSV balance | Add funds from faucet |
| History not persisting | localStorage disabled | Enable browser storage |

## Resources

- **Repository**: [github.com/bsv-blockchain-demos/inscription-platform](https://github.com/bsv-blockchain-demos/inscription-platform)
- **Code Tutorial**: [BSV Code Academy - Inscription Platform](https://hub.bsvblockchain.org/bsv-code-academy/intermediate-path/intermediate/inscription-platform)
- **Business Documentation**: [Business Overview](./business-inscription-platform.md)
- **Demo Environment**: Run locally via npm run dev
- **Support Contact**: BSV blockchain demos team

---
*For business context and ROI details, see: [Business Documentation](./business-inscription-platform.md)*
