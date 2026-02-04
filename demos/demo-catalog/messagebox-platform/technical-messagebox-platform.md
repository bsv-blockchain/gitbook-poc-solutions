# MessageBox Platform - Technical Documentation

**Demo ID**: `messagebox-platform`
**Version**: `1.0.0`
**Last Updated**: `2026-02-02`

## Architecture Overview

### System Architecture
Full-stack application with React frontend using WalletClient for identity management, Node.js backend for session handling, and MessageBox network for encrypted peer-to-peer message delivery with integrated BRC-29 payments.

### Technology Stack
- **Frontend**: React, TypeScript, Vite
- **Backend**: Node.js, Express, MongoDB
- **Blockchain SDK**: @bsv/sdk, @bsv/message-box-client
- **Infrastructure**: Cloud-hosted backend, MessageBox network nodes

### Key Components
1. **WalletClient**: Frontend wallet connection and identity derivation
2. **MessageBoxClient**: Identity certification and encrypted messaging
3. **PeerPayClient**: BRC-29 compliant identity-based payment tokens
4. **Session Manager**: Backend user session and wallet state management
5. **Database**: MongoDB for certified user registry

## Integration & APIs

### External Dependencies
| Service | Purpose | Version | Documentation |
|---------|---------|---------|---------------|
| @bsv/sdk | Transaction creation, key derivation | Latest | docs.bsvblockchain.org |
| @bsv/message-box-client | MessageBox protocol integration | Latest | github.com/bitcoin-sv/message-box |
| MongoDB | User and session storage | 5.0+ | mongodb.com/docs |
| BSV Desktop Wallet | User wallet integration | Latest | desktop.bsvb.tech |

### API Endpoints

```
POST /api/certify-identity
POST /api/send-message
POST /api/send-payment
GET /api/messages
POST /api/internalize-payment
```

## Implementation Guide

### Prerequisites
- Node.js 18+
- MongoDB 5.0+
- BSV Desktop Wallet installed
- MessageBox network access

### Setup Instructions

```bash
# Clone repository
git clone https://github.com/bsv-blockchain-demos/messagebox-platform
cd messagebox-platform

# Install dependencies
npm run install:all

# Configure environment
cp .env.example .env
# Edit .env with MongoDB URI and MessageBox host

# Start backend server
npm run dev:server

# Start frontend (in new terminal)
npm run dev:frontend
```

### Configuration

```
# Key configuration parameters
MONGODB_URI: mongodb://localhost:27017/messagebox
MESSAGEBOX_HOST: messagebox.babbage.systems
PORT: 3000
FRONTEND_URL: http://localhost:5173
```

## Testing & Validation

### Test Coverage
- **Unit Tests**: Identity certification, payment token creation
- **Integration Tests**: End-to-end message flow
- **Manual Tests**: Multi-user messaging scenarios

### Validation Criteria
- [ ] Users can certify identity on blockchain
- [ ] Encrypted messages delivered between users
- [ ] BRC-29 payments created and sent
- [ ] Recipients can internalize payments
- [ ] Sessions persist across browser refreshes

## Performance & Scalability

### Current Metrics
- **Message Delivery**: 2-5 seconds
- **Identity Certification**: 5-10 seconds
- **Payment Creation**: 1-3 seconds

### Scalability Considerations
- MessageBox network handles message routing
- MongoDB indexes on public keys for fast user lookup
- Backend can be horizontally scaled for multiple instances
- Frontend is stateless and cacheable

## Maintenance & Support

### Monitoring
- **Logs**: Backend server logs for certification/payment events
- **Metrics**: MongoDB query performance, MessageBox delivery rates
- **Alerts**: Failed certification transactions, MessageBox connection errors

### Troubleshooting
| Issue | Cause | Solution |
|-------|-------|----------|
| Identity certification fails | Insufficient BSV balance | Add funds to wallet |
| Messages not delivered | MessageBox network unavailable | Check network status |
| Payments not internalizing | Incorrect payment token format | Validate BRC-29 compliance |
| Database connection error | MongoDB not running | Start MongoDB service |

## Resources

- **Repository**: [github.com/bsv-blockchain-demos/messagebox-platform](https://github.com/bsv-blockchain-demos/messagebox-platform)
- **Code Tutorial**: [BSV Code Academy - MessageBox Platform](https://hub.bsvblockchain.org/bsv-code-academy/intermediate-path/intermediate/messagebox-platform)
- **Business Documentation**: [Business Overview](./business-messagebox-platform.md)
- **Demo Environment**: Requires backend + frontend running
- **Support Contact**: BSV blockchain demos team

---
*For business context and ROI details, see: [Business Overview](./business-messagebox-platform.md)*
