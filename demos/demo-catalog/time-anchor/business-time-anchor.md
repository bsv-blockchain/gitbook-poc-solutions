# Time Anchor with BSV Open Source Stack - Business Overview

**Demo ID**: `demo-2025-10`
**Status**: `Working POC`
**Last Updated**: `2026-05-26`

## Executive Summary

### Problem Statement
Ensuring the integrity and authenticity of files — software builds, contracts, evidence, datasets, design assets — over time is critical for security, auditability, and trust. Current solutions either rely on centralized notaries, leave audit trails that can be retroactively edited, or scatter integrity evidence across systems that themselves require trust. Organizations need a way to prove, in a publicly verifiable manner, that a file existed in a specific form at a specific time.

### Solution Overview
Time Anchor is a cross-platform desktop application (Windows, macOS, Linux) that anchors file integrity proofs to the BSV Blockchain. The SHA-256 hash of any file is recorded on-chain inside an `OP_FALSE OP_RETURN` output, producing an immutable, publicly verifiable timestamp. Optionally, the file itself is encrypted client-side using a wallet-derived key and published to UHRP (BRC-26) storage so it can be recalled later by anyone with the matching key. The application uses the `@bsv/sdk` `WalletClient` to delegate signing, key derivation, encryption, and broadcast to a locally running BSV wallet (BSV Desktop or MetaNet Client), and broadcasts transactions through the BSV Overlay Network on the `tm_desktopintegrity` topic for lookup via the `ls_desktopintegrity` service.

### Key Benefits
- **Tamper-evident records**: Immutable blockchain timestamps prove a file existed in its exact form at a specific moment
- **Privacy-preserving storage**: Files are encrypted client-side before any network transit; storage hosts only see ciphertext
- **Independent verifiability**: Anyone holding the file can confirm its anchoring without privileged access — only its bytes and an overlay endpoint are needed
- **No custodial keys**: All signing and encryption happens inside the user's wallet; the application never touches private keys
- **Open standards**: Built entirely on BSV open-source primitives — `@bsv/sdk`, Overlay Services, UHRP / BRC-26, BEEF transactions
- **Cross-platform**: Single Electron codebase produces installers for Windows, macOS, and Linux

## Business Impact

### Target Users
- **Primary**: Software publishers, security teams, compliance and audit functions, evidence custodians, IP rights holders
- **Secondary**: Regulators, open-source maintainers, journalists, archivists, legal teams handling document provenance

### Success Metrics
| Metric                         | Baseline               | Target                       | Timeline          |
|-------------------------------|------------------------|------------------------------|-------------------|
| Files anchored per session     | None                   | Routine use across asset class | 6 months        |
| Verification round-trip time   | N/A                    | Under 10 s end-to-end        | Available today   |
| Independent verifications      | None                   | Third parties verify without coordination | 6-12 months |
| Adoption of open-source stack  | Pilot usage            | Increased community adoption | 12 months         |

### ROI Analysis
- **Investment Required**: Wallet setup, optional self-hosted overlay/UHRP endpoints, integration time for teams wishing to embed anchoring into their workflow
- **Expected Return**: Reduced risk of undetected tampering, lower cost of audit and dispute resolution, defensible evidence for compliance and legal proceedings
- **Break-even Point**: Realized as soon as a single integrity dispute can be resolved by reference to on-chain proof instead of forensic reconstruction

## Implementation Roadmap

### Phase 1: Working POC – Delivered Q2 2026
- Desktop application with anchor, recall, and verify workflows
- BSV transaction creation via `@bsv/sdk` `WalletClient` integration with BSV Desktop / MetaNet Client
- Overlay broadcast and lookup on `tm_desktopintegrity` / `ls_desktopintegrity`
- Client-side encryption and UHRP / BRC-26 publication of encrypted file payloads
- Local audit logs and per-session derived encryption keys
- Cross-platform installers (Windows / macOS / Linux)

### Phase 2: Hardening & Scale – H2 2026
- Lift the 25 MB file size limit through chunking or external-hash flows
- Configurable UHRP retention (currently fixed at 30 days)
- Batch anchoring for high-volume use cases (CI build artifacts, document repositories)
- Multi-wallet support and shared-team key derivation patterns
- Optional self-hosted overlay / UHRP deployment guide

### Phase 3: Ecosystem & Integrations – 2027
- CLI and headless variants for CI/CD and server-side anchoring
- SDK wrappers for embedding anchoring into existing applications
- Integrations with common evidence/audit platforms

## Stakeholders

- **Business Owner**: Product & Security Managers
- **Technical Lead**: Blockchain Integration Architect
- **Key Users**: Security auditors, developers, compliance teams, evidence custodians

---
