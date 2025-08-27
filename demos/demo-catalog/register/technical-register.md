# Register (BlockMed) - Technical Documentation

**Demo ID**: `demo-2025-002`
**Version**: `1.0.0`
**Last Updated**: `August 2025`

---

## Overview

Register (BlockMed) is a blockchain-based prescription management system using Decentralized Identifiers (DIDs) and Verifiable Credentials (VCs) on the Bitcoin SV overlay network with extended QuarkID packages. The system enables secure prescription workflows with actors including patients, doctors, pharmacies, and insurers.

---

## Features

- Blockchain-backed digital prescription issuance and verification
- Immutable tracking of prescriptions to prevent fraud and double spending
- Decentralized Identity (DID) management and Verifiable Credentials issuance
- Token-based authorization system for prescription-related transactions
- Web-based frontend dashboard for all actors
- Overlay service for managing on-chain DID and credential transactions

---

## System Architecture

### Frontend (React + TypeScript)

- User interface for actor management, prescription workflows, and dashboard interaction
- Built with React, TypeScript, and state management libraries
- Supports multi-role access (patient, doctor, pharmacy, insurer)

### Backend (Node.js + Express)

- REST API for actor, prescription, and token management
- Implements business logic, MongoDB data persistence, and integration with BSV overlay network
- Provides secure endpoints for creating and verifying credentials and prescriptions

### Overlay Service (LARS)

- Manages blockchain communication, DID registration, and transaction confirmation
- Utilizes extended QuarkID packages for managing identities and VCs on Bitcoin SV
- Runs as a separate microservice to scale blockchain communication efficiently

---

## Quick Start Guide

### Prerequisites

- Node.js v18 or newer
- Docker v20 or newer
- npm v8 or newer
- Git
- (Optional) Metanet Desktop for funding PLATFORM_FUNDING_KEY

### Clone and Setup

```bash
git clone git@github.com:sirdeggen/register.git
cd register
make quickstart
```

This installs dependencies, builds QuarkID, sets environment, and runs frontend, backend, and overlay services.

---

## Running Individually

- Install dependencies:
```bash
make install
```
- Build QuarkID:
```bash
make build-quarkid
```
- Setup environment:
```bash
make setup-env
```
- Run services:
```bash
make run
```
Or individually in terminals:
```bash
make run-backend
make run-front
make run-overlay
```
---

## Usage Guide

### Actor Setup

- Create actors via backend script:
```bash
npx tsx back/src/scripts/seedActors.ts
```

- Or create manually via frontend at <http://localhost:5173>, "Actor Management" section. Roles: Patient, Doctor, Pharmacy, Insurance.

### Prescription Workflow

- Login as Doctor → create prescriptions
- Patient → share prescriptions with Pharmacy
- Pharmacy → verify and dispense medications
- Patient → confirm medication receipt
- Insurance → verify coverage via shared prescriptions

---

## API Endpoints

- `/v1/actors` - DID and actor management
- `/v1/prescriptions` - Create and manage prescriptions
- `/v1/enhanced/prescriptions` - Token-authorized prescription workflows
- `/v1/shared-prescriptions` - Prescription sharing management

(See backend route files `back/src/routes/` for full API docs)

---

## Security Features

- Decentralized Identifiers (DIDs) for verified identity management
- Verifiable Credentials for secure prescription proofs
- Blockchain immutability for tamper-proof tracking
- Token authorization to prevent prescription abuse

---

## Testing

- Backend tests:
```bash
cd back
npm test
```
- Frontend tests:
```bash
cd front
npm test
```

---

## Performance & Scalability

- Modular microservice architecture
- MongoDB for scalable document storage
- Overlay network for efficient blockchain interaction
- Supports multiple concurrent prescription workflows

---

## Troubleshooting

| Issue                       | Cause                              | Solution                                        |
|-----------------------------|------------------------------------|------------------------------------------------|
| Backend not running          | Environment setup missing           | Run `make setup-env` then restart services     |
| MongoDB connection failed   | Overlay not running or config error | Ensure overlay running and env variables set   |
| DID creation errors         | Overlay or network issues           | Check overlay logs and blockchain connectivity |
| Prescription workflow stuck | Token authorization failure         | Verify token availability and permissions      |

---

##  Resources

- **Business Documentation:** [business-register.md](business-register.md)
- **Repository:** [GitHub - bsv-blockchain-demos/register](https://github.com/bsv-blockchain-demos/register)
- **Demo Site:** (Not publicly listed; see repo for local deployment)

---

*For business context and workflow descriptions, refer to the business document.*
