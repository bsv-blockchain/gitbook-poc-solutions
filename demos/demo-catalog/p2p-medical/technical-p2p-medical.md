# P2P Medical - Technical Documentation

**Demo ID**: `p2p-medical`
**Version**: `1.0.0`
**Last Updated**: `2026-04-15`

## Architecture Overview

### System Architecture
Browser-first application: the patient's client resolves the recipient doctor's public key, derives a shared secret via ECDH, encrypts the medical file in-browser with AES-256-GCM, and uploads the ciphertext to one or more UHRP storage providers (content-addressed by SHA-256). A PushDrop token carrying the file metadata is minted on-chain and the doctor is notified via MessageBox. The doctor fetches ciphertext from UHRP, verifies the hash against the on-chain metadata, and decrypts locally with their wallet. Every access emits an on-chain event that both parties can audit.

### Technology Stack
- **Frontend**: React, TypeScript, browser-native WebCrypto (ECDH, AES-256-GCM, SHA-256)
- **Blockchain SDK**: @bsv/sdk (PushDrop, WalletClient, MessageBox client, identity resolution)
- **Storage**: UHRP (Universal Hash Resolution Protocol) — one or multiple providers selectable at upload time
- **Messaging**: MessageBox protocol for recipient notification
- **Wallet Integration**: BRC-100-compatible wallets (BSV Desktop) for key derivation, signing, and decryption

### Key Components
1. **Recipient Resolver**: Looks up the selected doctor's identity public key for ECDH targeting
2. **In-Browser Encryption Module**: Derives ECDH shared secret, performs AES-256-GCM encryption, computes SHA-256 of ciphertext
3. **UHRP Uploader**: Distributes ciphertext across selected storage providers, returns content-addressed URLs
4. **PushDrop Metadata Token**: On-chain token carrying file hash, recipient pointer, and MIME/type metadata
5. **MessageBox Notifier**: Delivers a recipient-targeted message with pointers to the UHRP content and the on-chain token
6. **Audit Logger**: Writes an on-chain event for each access (upload, fetch, decrypt) visible to the patient
7. **Educational Layer**: Landing-page interactive cards and per-step tooltips explaining the stack in context

## Integration & APIs

### External Dependencies
| Service | Purpose | Version | Documentation |
|---------|---------|---------|---------------|
| @bsv/sdk | PushDrop, WalletClient, MessageBox, identity & crypto primitives | Latest | docs.bsvblockchain.org |
| UHRP Providers | Content-addressed storage (multi-provider optional) | Latest | docs.bsvblockchain.org |
| BSV Desktop | User key management, ECDH decryption, signing | Latest | desktop.bsvb.tech |
| MessageBox | Recipient notification channel | Latest | docs.bsvblockchain.org |

### Core Flow
| Step | Actor | Action |
|------|-------|--------|
| 1 | Patient | Selects doctor recipient; resolves recipient public key |
| 2 | Patient (browser) | ECDH + AES-256-GCM encryption of file; computes SHA-256 |
| 3 | Patient | Uploads ciphertext to one or multiple UHRP providers |
| 4 | Patient | Mints PushDrop token carrying metadata (hash, pointers) |
| 5 | Patient | Sends MessageBox notification to doctor |
| 6 | Doctor | Downloads ciphertext from UHRP, verifies SHA-256 |
| 7 | Doctor (wallet) | Derives shared secret via ECDH, decrypts locally |
| 8 | Both | Access events recorded on-chain; audit history visible to patient |

## Implementation Guide

### Prerequisites
- Node.js 18+
- BSV Desktop (BRC-100 compatible) installed and unlocked
- Access to at least one UHRP storage provider

### Setup Instructions

```bash
# Clone repository
git clone https://github.com/bsv-blockchain-demos/p2p-medical

# Install dependencies
cd p2p-medical
npm install

# Configure environment (UHRP providers, MessageBox endpoint, network)
cp .env.example .env.local

# Run application
npm run dev
```

Live deployment: [p2p-medical.bsvblockchain.tech](https://p2p-medical.bsvblockchain.tech)

### Configuration
| Variable | Purpose | Default |
|----------|---------|---------|
| `BSV_NETWORK` | `'test'` or `'main'` — determines chain for wallet services | `main` |
| `UHRP_PROVIDERS` | Comma-separated list of UHRP provider endpoints | Multiple providers |
| `MESSAGEBOX_HOST` | MessageBox service endpoint for recipient notification | Latest |

## Testing & Validation

### Test Coverage
- **Unit Tests**: Encryption module (ECDH derivation, AES-256-GCM round-trip, SHA-256 hashing)
- **Integration Tests**: UHRP upload/download with single and multi-provider configs, PushDrop minting, MessageBox notification delivery
- **Manual Tests**: Full patient-to-doctor flow including recipient-only decryption and sender-cannot-decrypt verification

### Validation Criteria
- [ ] File is encrypted in-browser before leaving the client
- [ ] Ciphertext SHA-256 matches the on-chain PushDrop metadata
- [ ] Only the recipient's wallet can decrypt — sender cannot decrypt their own file
- [ ] Multi-provider UHRP upload succeeds with partial provider failure tolerated
- [ ] MessageBox notification reaches the recipient with valid UHRP pointers
- [ ] Every access event (upload, fetch, decrypt) is written on-chain and visible in audit history

## Performance & Scalability

### Current Metrics
- **In-browser encryption**: sub-second for typical medical files (images, PDFs)
- **UHRP upload**: depends on provider and file size; multi-provider parallelizable
- **PushDrop minting**: 1-2s typical broadcast time
- **Decryption**: sub-second after wallet unlock

### Scalability Considerations
- All heavy cryptography runs client-side — no server-side secrets or plaintext custody
- UHRP multi-provider upload is parallelizable and resilient to single-provider outages
- On-chain audit events scale with access frequency; batching is a future optimization
- No centralized database for file content — content-addressing via SHA-256 guarantees integrity

## Maintenance & Support

### Monitoring
- **Logs**: Client-side console and structured error reporting for encryption, upload, and broadcast steps
- **Metrics**: UHRP provider availability, broadcast success rate, MessageBox delivery rate
- **Alerts**: User-facing errors for wallet disconnection, provider failure, and hash verification mismatch

### Troubleshooting
| Issue | Cause | Solution |
|-------|-------|----------|
| Wallet not found | BSV Desktop not installed or locked | Install wallet and unlock before use |
| Recipient decryption fails | Recipient public key mismatch or wrong wallet | Confirm recipient selection; recipient must use the same wallet identity used for ECDH |
| UHRP fetch fails | Provider outage or object pruned | Re-upload with multi-provider selection; content-addressing allows seamless failover |
| Hash verification mismatch | Corrupted download or wrong object | Refetch from an alternate UHRP provider |

## Resources

- **Repository**: [github.com/bsv-blockchain-demos/p2p-medical](https://github.com/bsv-blockchain-demos/p2p-medical)
- **Live Demo**: [p2p-medical.bsvblockchain.tech](https://p2p-medical.bsvblockchain.tech)
- **Business Documentation**: [Business Overview](./business-p2p-medical.md)
- **Support Contact**: BSV blockchain demos team

---
*For business context and ROI details, see: [Business Documentation](./business-p2p-medical.md)*
