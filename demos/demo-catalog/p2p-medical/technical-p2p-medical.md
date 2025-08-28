# Secure Med Drop-Off - Technical Documentation

**Demo ID**: `demo-2025-006`
**Version**: `1.0.0`
**Last Updated**: `August 2025`

---

## Overview

Secure Med Drop-Off is a demo web application built in TypeScript for confidential peer-to-peer medical file transfer with blockchain-backed logging and permissions. It uses Bitcoin SV to anchor transaction records, ensuring every file drop is verifiable, auditable, and private.

---

## Features

- Browser-based upload and download of medical files
- File transfer events are logged immutably on the blockchain for auditability
- Fine-grained permission control for sender and receiver roles
- Data encryption (in transit and at rest)
- Demo code structure ready for extension

---

## System Architecture

### Frontend (TypeScript, React/Vite)

- User dashboard for submitting, picking up, and viewing files and their transaction history
- Permission control UI

### Backend (Node.js, Express or similar)

- REST APIs for file upload, download, permission management, and blockchain event logging
- Encrypted storage for files; integration with Bitcoin SV for transaction anchoring

---

## Quick Start

### Prerequisites

- Node.js and npm installed
- Vite development server for frontend (see output above, usually http://localhost:8080/)

### Running Locally

```bash
npm i
npm run dev
```
Access the app at [http://localhost:8080/](http://localhost:8080/) or on your LAN (example: http://192.168.1.42:8080/).

---

## Usage Guide

- **Upload:** User chooses medical file and drops it off securely via browser
- **Transfer:** The file is associated with recipient permissions and timestamped on-chain
- **Retrieve:** Authorized recipient downloads the file; event is also logged
- **Audit:** Full history of all transfers viewable; blockchain explorer integration for verification

---

## Security Features

- All files are encrypted before storage or transfer
- Blockchain-based audit trail ensures tamper-evidence
- Only authorized users (per role or address) can access specific files

---

## Testing & Validation

- Manual tests for file upload, download, permissions, and audit
- Review blockchain logs for each transaction to confirm correct behavior

---

## Troubleshooting & Support

- Ensure Vite dev server and backend API are running
- Use correct URL for device (localhost or network IP)
- Review logs and browser console; check blockchain connectivity for audit logs

---

## Resources
- **Business Documentation:** [business-p2p-medical.md](business-p2p-medical.md)
- **Repository:** [GitHub - bsv-blockchain-demos/p2p-medical](https://github.com/bsv-blockchain-demos/p2p-medical)
- **Demo URL:** http://localhost:8080/

---

*Refer to the business document for workflow use cases and operational scenarios.*
