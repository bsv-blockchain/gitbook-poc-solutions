# Certification Platform - Technical Documentation

**Demo ID**: `certification-platform`
**Version**: `1.0.0`
**Last Updated**: `2026-02-10`

## Architecture Overview

### System Architecture
Full-stack application with a Next.js frontend and Express.js backend, connecting to user wallets via WalletClient for certificate acquisition and AuthFetch for authenticated access to protected routes. The backend serves as a self-hosted certifier using MasterCertificate for issuance and auth middleware for mutual authentication.

### Technology Stack
- **Frontend**: Next.js, React, TypeScript
- **Backend**: Express.js, TypeScript
- **Blockchain SDK**: @bsv/sdk (MasterCertificate, VerifiableCertificate, AuthFetch, WalletClient)
- **Wallet Integration**: @bsv/wallet-toolbox-client (server-side Wallet)
- **Auth Layer**: @bsv/auth-express-middleware (BRC-31 mutual authentication)

### Key Components
1. **MasterCertificate Issuance**: Server-side certificate creation with field-level ECDH encryption
2. **Auth Middleware**: Express middleware implementing BRC-31 mutual authentication with session management
3. **WalletClient Integration**: Browser-side wallet connection for identity, certificate storage, and AuthFetch
4. **VerifiableCertificate Verification**: Server-side field decryption and certificate validation

## Integration & APIs

### External Dependencies
| Service | Purpose | Version | Documentation |
|---------|---------|---------|---------------|
| @bsv/sdk | Certificates, AuthFetch, WalletClient, cryptography | 1.10.1+ | docs.bsvblockchain.org |
| @bsv/wallet-toolbox-client | Server-side full Wallet with storage and chain services | 1.7.18+ | docs.bsvblockchain.org |
| @bsv/auth-express-middleware | Express middleware for BRC-31 mutual authentication | 1.2.3+ | docs.bsvblockchain.org |
| BSV Desktop | User key management, certificate storage, signing | Latest | desktop.bsvb.tech |

### API Endpoints
| Method | Endpoint | Auth | Purpose |
|--------|----------|------|---------|
| GET | `/api/info` | None | Certifier discovery (public key + certificate type) |
| POST | `/api/certify` | None | Issue encrypted, signed certificate for a subject |
| GET | `/protected/dashboard` | BRC-31 | Certificate-gated content access |
| POST | `/relinquish` | BRC-31 | Certificate revocation and session cleanup |

## Implementation Guide

### Prerequisites
- Node.js 18+
- BSV Desktop installed
- TypeScript 4.9+
- A wallet storage service URL (for backend Wallet initialization)

### Setup Instructions

```bash
# Clone repository
git clone https://github.com/bsv-blockchain-demos/certification-platform

# Start backend
cd certification-platform/backend
npm install
npm run dev
# Running on http://localhost:3002

# In another terminal, start frontend
cd certification-platform/frontend
npm install
npm run dev
# Running on http://localhost:3000
```

### Configuration
Backend configuration via environment variables:

| Variable | Purpose | Default |
|----------|---------|---------|
| `SERVER_PRIVATE_KEY` | 32-byte hex private key — server's certifier identity | Development key |
| `BSV_NETWORK` | `'test'` or `'main'` — determines chain for wallet services | `test` |
| `WALLET_STORAGE_URL` | Remote storage endpoint for the full Wallet | `https://storage.babbage.systems` |
| `PORT` | Express server port | `3002` |

Frontend configuration via `.env.local`:

| Variable | Purpose | Default |
|----------|---------|---------|
| `NEXT_PUBLIC_API_URL` | Backend API base URL | `http://localhost:3002` |

## Testing & Validation

### Test Coverage
- **Unit Tests**: Certificate issuance and field encryption
- **Integration Tests**: WalletClient connection, AuthFetch authentication flow
- **Manual Tests**: End-to-end certificate lifecycle (issuance → storage → gated access → revocation)

### Validation Criteria
- [ ] Backend issues encrypted, signed certificates via `/api/certify`
- [ ] Certificates stored in browser wallet with encrypted fields via direct acquisition
- [ ] Auth middleware authenticates requests using BRC-31 mutual authentication
- [ ] Protected dashboard accessible only to certificate holders
- [ ] Certificate revocation removes wallet cert and cleans up server session
- [ ] Full lifecycle works end-to-end: discovery → issuance → gated access → revocation

## Performance & Scalability

### Current Metrics
- **Certificate Issuance**: <1s (server-side MasterCertificate creation)
- **Wallet Interaction**: 2-3s per signature/acquisition
- **Auth Handshake**: 1-2s initial session negotiation, cached for subsequent requests

### Scalability Considerations
- SessionManager uses in-memory storage (suitable for single-server deployments)
- Certificate verification state tracked in-memory (upgrade to persistent storage for production)
- No on-chain transactions required for certificate issuance (off-chain signed certificates)
- Stateless certificate verification — any server with the certifier key can verify

## Maintenance & Support

### Monitoring
- **Logs**: Express server console logs with configurable auth middleware log levels
- **Metrics**: Certificate issuance counts, auth middleware session tracking
- **Alerts**: User-facing error messages for wallet connection and certification failures

### Troubleshooting
| Issue | Cause | Solution |
|-------|-------|----------|
| Wallet not found | BSV Desktop not installed or locked | Install wallet extension and unlock before use |
| Failed to get certificate from server | Backend not running or wrong port | Ensure backend is running; check `NEXT_PUBLIC_API_URL` |
| Certificate Required on dashboard | No certificate in wallet | Acquire a certificate on the main certify page first |
| Access denied after acquiring certificate | Auth middleware hasn't processed certificate | Check backend logs for `onCertificatesReceived`; verify certificate type and certifier match |

## Resources

- **Repository**: [github.com/bsv-blockchain-demos/certification-platform](https://github.com/bsv-blockchain-demos/certification-platform)
- **Code Tutorial**: [BSV Code Academy - Certification Platform](https://hub.bsvblockchain.org/bsv-code-academy/intermediate-path/intermediate/certification-platform)
- **Business Documentation**: [Business Overview](./business-certification-platform.md)
- **Demo Environment**: Run locally via npm run dev (backend + frontend)
- **Support Contact**: BSV blockchain demos team

---
*For business context and ROI details, see: [Business Documentation](./business-certification-platform.md)*
