# Truth Machine - Technical Documentation

## Overview

Truth Machine is a blockchain-based data integrity and timestamping system built on BSV. It provides immutable proof of data existence and integrity by recording cryptographic hashes on the blockchain.

---

## Features

- **Secure File Storage**: Upload files with blockchain-backed integrity verification
- **Timestamping**: Immutable proof of data existence at a specific time
- **Integrity Verification**: Download files with cryptographic proof of integrity
- **BEEF Integration**: [Background Evaluation Extended Format for complete transaction verification](https://hub.bsvblockchain.org/brc/transactions/0062)
- **Treasury Management**: Built-in token system for managing transaction fees
- **QR Code Support**: Easy funding through QR code scanning
- **Modern Web Interface**: User-friendly React-based frontend

---

## System Architecture

### Frontend (React + TypeScript)

- Modern React application with TypeScript
- Real-time treasury balance monitoring
- Intuitive file upload/download interface
- QR code generation for funding

### Backend (Node.js + Express)

- RESTful API endpoints for file operations
- [BEEF transaction format support](https://hub.bsvblockchain.org/brc/transactions/0062)
- Blockchain integration via WhatsOnChain
- MongoDB for file and transaction storage

---

## Quick Start Guide

Prerequisites: Latest versions of Docker Compose and Node.js. The `start.sh` script assumes macOS/Unix environment.

### Setup
```bash
git clone https://github.com/bsv-blockchain-demos/truth-machine.git
cd truth-machine
sh quickstart.sh
```

This script will install dependencies for frontend and backend, generate local keys, update your `.env` and `docker-compose.yml`, then launch the application.

### Reverse Proxy Setup

To route API callbacks, set up a public tunnel:
```bash
ngrok http 3030
```

Copy the public URL and update `DOMAIN` variable in `.env` or `docker-compose.yml` accordingly.

### Stopping and Restarting

Stop services:
```bash
docker compose down
```

Restart without destroying the environment:

```bash
docker compose up
```

---

## Running the Application

### Development Mode

```bash
cd back
npm run dev
cd ../front
npm run dev
```

### Production Mode

```bash
cd back npm run build
npm run start
cd ../front
npm run build
npm run preview
```

### Using Docker

Build images:

```bash
docker compose build
```

Run containers:

```bash
docker compose up
```

---

## Usage Guide

### Treasury Management

- Access Treasury section to see balance
- Fund treasury by scanning QR code with BSV
- Create write tokens for uploading files (1 token per upload)

### File Upload

1. Navigate to Upload section
2. Select a file for upload
3. System calculates cryptographic hash, writes transaction on-chain, securely stores file, and provides transaction ID and proof

### File Download

1. Navigate to Download section
2. Input file hash or transaction ID
3. Receive original file, timestamp proof, integrity verification, and BEEF transaction data

---

## API Endpoints

### File Operations

- `POST /api/upload` — Upload and timestamp file
- `GET /api/download/:hash` — Download file and proofs
- `GET /api/verify/:hash` — Verify file integrity

### Treasury Operations

- `GET /api/checkTreasury` — Get treasury balance
- `POST /api/fund/:tokens` — Create upload tokens

---

## Security Features

- [BEEF format for transaction verification](https://hub.bsvblockchain.org/brc/transactions/0062)
- Simplified Payment Verification (SPV)
- SHA-256 file hash verification
- Immutable blockchain-backed timestamping

---

## Development Environment

### Prerequisites

- Node.js 18+
- MongoDB 4.4+
- BSV wallet for testing

### Testing

Run backend tests:
```bash
cd back
npm test
```

Run frontend tests:
```bash
cd front
npm test
```
### Validation Criteria
- Functional tests for upload, download, and verification APIs
- Performance tests simulate upload and retrieval under load

## Performance & Scalability

### Current Metrics
- Response times typically under 500 ms for API operations
- Supports thousands of timestamped files with MongoDB indexing
- Scalable via Docker orchestrators or Kubernetes

### Scalability Considerations
- Token treasury ensures sufficient transaction fee coverage
- MongoDB sharding for large-scale file storage
- Caching of blockchain responses to reduce API load

## Maintenance & Support

### Monitoring
- Logs accessible via Docker or node logging systems
- Metrics dashboard can be integrated with external monitoring tools

### Troubleshooting
| Issue                           | Cause                       | Solution                                       |
|--------------------------------|-----------------------------|------------------------------------------------|
| API not reachable               | Backend not running          | Restart backend service, check logs            |
| File upload fails               | Insufficient tokens          | Fund treasury with BSV tokens                   |
| Blockchain write fails          | Network or API problem       | Check WhatsOnChain service status               |
| Verification returns false      | Data tampering or errors     | Confirm file integrity and latest blockchain state |

## Resources

- **Repository**: https://github.com/bsv-blockchain-demos/truth-machine
- **Business Documentation**: [Link to business-truth-machine.md](business-truth-machine.md)
- **Demo Environment**: https://truth-machine.bsvb.tech
- **Support Contact**: bsv-blockchain GitHub issues page

---
*For business context and ROI details, see: [Business Documentation](business-truth-machine.md)*
