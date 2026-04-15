# P2P Medical - Business Overview

**Demo ID**: `p2p-medical`
**Status**: `Production`
**Last Updated**: `2026-04-15`

## Executive Summary

### Problem Statement
Medical file exchange today relies on centralized portals, email, fax, or courier services that expose sensitive patient data to intermediaries, offer no cryptographic guarantee of confidentiality, and lack a verifiable audit trail. Patients have no way to prove who accessed their records, and providers have no trustworthy way to confirm a file has not been tampered with in transit.

### Solution Overview
A browser-based peer-to-peer medical file sharing application where patients select a doctor as the intended recipient, encrypt the file in-browser using ECDH + AES-256-GCM so only that doctor's wallet can decrypt it, distribute the encrypted payload across one or multiple UHRP storage providers for availability, mint a PushDrop token on-chain carrying the file metadata, and notify the recipient through MessageBox. Every access — including the sender's own view of events — is recorded on-chain as part of an immutable audit history.

### Key Benefits
- End-to-end encryption with one-directional ECDH key agreement — not even the sender can decrypt after upload
- Patient-controlled sharing with cryptographic recipient targeting, no platform custody of plaintext data
- On-chain audit trail of every access event, visible to the patient in real time
- Multi-provider UHRP distribution for redundancy and availability without vendor lock-in
- Educational, tooltip-driven UX that makes the open-source BSV stack practical to understand

## Business Impact

### Target Users
- **Primary**: Patients sharing medical files (X-rays, scans, lab reports) with doctors or specialists
- **Secondary**: Clinics, radiology providers, and telehealth platforms requiring HIPAA/GDPR-aligned file delivery with audit trails

### Success Metrics
| Metric | Baseline | Target | Timeline |
|--------|----------|---------|----------|
| Encrypted files exchanged | 0 | 1,000 | 6 months |
| Unique patient-doctor pairs | 0 | 250 | 6 months |
| Auditable access events recorded on-chain | 0 | 5,000 | 12 months |

### ROI Analysis
- **Investment Required**: Frontend/backend development, UHRP storage subscriptions, minimal server infrastructure
- **Expected Return**: Reduced reliance on centralized health record portals, removal of intermediary custody risk, auditable compliance with data protection regulations
- **Break-even Point**: 3-6 months for healthcare integrators replacing legacy portal licensing

## Implementation Roadmap

### Phase 1: Core Secure Exchange - Months 1-2
- Patient-to-doctor recipient selection and identity resolution
- In-browser ECDH + AES-256-GCM encryption
- UHRP single-provider upload and SHA-256 content addressing
- PushDrop metadata token minting
- MessageBox recipient notification
- **Resources Needed**: 1 frontend developer, 1 backend developer

### Phase 2: Redundancy, Audit & Education - Months 3-4
- Multi-provider UHRP distribution with availability fallback
- On-chain audit history with patient-facing access log
- Interactive landing page explaining every stack component
- Contextual tooltips and guides across upload, minting, broadcast, decryption flows
- **Resources Needed**: 1 full-stack developer, 1 UX/content designer

## Stakeholders

- **Business Owner**: Healthcare Solutions Division
- **Technical Lead**: BSV Development Team
- **Key Users**: Patients, doctors, radiology and specialist clinics, telehealth platforms

## Related Resources

- **Technical Documentation**: [Technical Documentation](./technical-p2p-medical.md)
- **Live Demo**: [p2p-medical.bsvblockchain.tech](https://p2p-medical.bsvblockchain.tech)
- **Repository**: [github.com/bsv-blockchain-demos/p2p-medical](https://github.com/bsv-blockchain-demos/p2p-medical)

---
*For technical implementation details, see: [Technical Documentation](./technical-p2p-medical.md)*
