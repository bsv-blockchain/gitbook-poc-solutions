# Prescription Tokens - Technical Documentation

**Demo ID**: `demo-2025-003`
**Version**: `1.0.0`
**Last Updated**: `August 2025`

---

## Overview

Prescription Tokens is a blockchain-based demo illustrating secure prescription data capture and transfer using tokens on the BSV blockchain. It models a workflow where doctors create prescription tokens, which are passed to patients and pharmacies to verify and dispense medications securely.

---

## Features

- Tokenization of prescriptions for secure transfer and redemption
- Immutable record of prescription origination anchored on BSV
- Clear roles: Doctor (creator), Patient (holder), Pharmacy (redeemer)
- Simple web interface to interact with tokens
- Demo-level codebase for extension and integration

---

## System Architecture

### Frontend (TypeScript, React)

- Provides a web UI for creating, sending, and redeeming prescription tokens
- Displays token status and related transaction info

### Backend (Node.js, Express)

- Manages token lifecycle, prescription metadata, and blockchain interactions
- Interfaces with BSV network to anchor transactions
- RESTful APIs for frontend communication

---

## Quick Start

### Prerequisites

- Node.js and npm installed
- Basic familiarity with BSV blockchain concepts

### To Run

```bash
npm i
npm run build
lars start
```

Visit [http://localhost:3000](http://localhost:3000) on your browser to interact with the demo.

---

## Usage Guide

### Prescription Token Workflow

- **Doctor:** Create a prescription token and assign to a patient
- **Patient:** Receive and hold the token
- **Pharmacy:** Redeem the token to verify and dispense medication

Tokens map prescription data immutably on the BSV blockchain, enabling secure tracking and audit.

---

## API Endpoints

- APIs to create, transfer and redeem tokens
- Endpoints to query token status and transaction proofs

(Specific endpoints are defined in the repo backend code)

---

## Security Features

- Blockchain-anchored token creation provides tamper-proof provenance
- Tokens ensure strict control over prescription usage and one-time redemption
- Transparent audit trail with cryptographic verification

---

## Testing & Validation

- Unit and integration tests for token lifecycle and blockchain anchoring
- Manual UI testing for prescription creation and redemption flows

---

## Troubleshooting & Support

- Check logs for API and blockchain transaction errors
- Verify BSV node or network connectivity
- Report issues on the GitHub repo Issues page

---

## Resources

- **Repository:** [GitHub - bsv-blockchain-demos/prescription-tokens](https://github.com/bsv-blockchain-demos/prescription-tokens)
- **Demo URL:** [Prescriptions Site](https://prescription-tokens.vercel.app/)
- **Related Docs:** Business and Register demos for ecosystem context

---

*For business context, see the corresponding business-prescription-tokens.md document.*

