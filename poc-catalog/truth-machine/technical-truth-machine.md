# Truth Machine - Technical Documentation

**POC ID**: `POC-2025-001`
**Version**: `1.0.8`
**Last Updated**: `August 2025`

## Architecture Overview

### System Architecture
The Truth Machine system consists of a **React + TypeScript frontend** and a **Node.js + Express backend**, integrated with the Bitcoin SV blockchain for immutable timestamping.

- Frontend provides a modern, user-friendly interface for file upload, treasury management, QR code scanning, and real-time treasury balance.
- Backend exposes RESTful APIs to handle file operations, create blockchain transactions using BEEF format, and store files and transaction data in MongoDB.

### Technology Stack
- **Frontend**: React, TypeScript
- **Backend**: Node.js, Express, BEEF transaction protocol
- **Database**: MongoDB
- **Infrastructure**: Docker, local development uses Docker Compose; designed for cloud deployment

### Key Components
1. **Frontend UI**: Manages user interactions, file upload/download, and treasury balance display
2. **Backend API**: Implements REST API endpoints for file and treasury management
3. **Blockchain Integration**: Uses WhatsOnChain API and BEEF transaction format for writing and verifying blockchain proofs
4. **Database Layer**: MongoDB stores uploaded files metadata, transaction records, and token balances

## Integration & APIs

### External Dependencies
| Service       | Purpose                            | Version | Documentation                              |
|---------------|----------------------------------|---------|--------------------------------------------|
| WhatsOnChain  | Blockchain transaction querying  | Latest  | https://developers.whatsonchain.com/       |
| MongoDB       | Document storage                  | 4.4+    | https://docs.mongodb.com/                    |

### API Endpoints
POST /api/upload
- Upload and timestamp a file on the blockchain

GET /api/download/:hash
- Download file and proofs by file hash

GET /api/verify/:hash
- Verify file integrity based on blockchain proof

GET /api/checkTreasury
- Get current treasury balance

POST /api/fund/:tokens
- Create write tokens for file uploads

## Implementation Guide

### Prerequisites
- Node.js v18 or higher
- MongoDB 4.4 or higher
- Docker and Docker Compose (for containerized setup)
- BSV wallet for funding transactions (optional)

### Setup Instructions
```bash
git clone https://github.com/bsv-blockchain-demos/truth-machine.gitcd
truth-machinesh
quickstart.sh
```

This will install dependencies for both frontend and backend, generate keys, and start the application with Docker Compose.

If using a local blockchain callback server, tools like `ngrok` can be used to expose webhook URLs by configuring the `DOMAIN` environment variable in the docker-compose.yml.

### Running the Application

**Development mode:**
```bash
cd back
npm run dev
cd
../front
npm run dev
```

**Production mode:**
```bash
cd back
npm run build
npm run start
cd ../front
npm run build
npm run preview
```

**Docker:**
```bash
docker compose build
docker compose up
```

### Configuration
Key environment variables
DOMAIN: your-public-domain-for-callbacks
MONGO_URL: your-mongodb-connection-string
BSV_WALLET_PRIVATE_KEY: blockchain wallet key

## Testing & Validation

### Test Coverage
- Unit tests available under `back` and `front` folders
- Run tests with `npm test` in respective directories

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
