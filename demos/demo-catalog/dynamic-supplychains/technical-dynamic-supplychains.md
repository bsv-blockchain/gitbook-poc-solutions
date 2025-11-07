# Supply Chain Action Builder - Technical Documentation

**Demo ID**: `demo-2025-013`
**Version**: `1.0.0`
**Last Updated**: `November 2025`

---

## Overview

Supply Chain Action Builder is a blockchain-based supply chain management system built with Next.js 14 and BSV blockchain. It enables users to create, transfer, and track multi-stage action chains with on-chain data storage using the PushDrop protocol, ownership verification through cryptographic locks, and comprehensive transfer tracking.

---

## Features

- Create multi-stage action chains with minimum 2 stages and title requirement
- Store stage data on-chain using BSV SDK PushDrop protocol
- Transfer chains between users with receiver public key verification
- Track ownership through locks collection and transfer records
- View detailed stage information with transaction history
- Real-time pending chain count with 30-second auto-refresh
- Pre-built templates for common supply chain workflows
- Title validation for chain transfers
- Detailed stage view with expandable full-screen modal

---

## System Architecture

### Frontend (Next.js 14 with App Router)

- React-based user interface with App Router architecture
- Context API for state management
- Tailwind CSS for responsive styling
- Real-time updates for pending chains badge

### Blockchain Layer (BSV Blockchain)

- @bsv/sdk for blockchain interactions
- PushDrop protocol for on-chain data storage with AES encryption
- Transaction and Wallet APIs for chain creation and transfers
- Ownership verification through cryptographic locks

### Database (MongoDB)

- Locks collection for active ownership tracking (deleted on transfer)
- ChainTransfer collection for transfer history with `continued` status flags
- Stage data storage with blockchain transaction references

---

## Quick Start

### Prerequisites

- Node.js and npm installed
- MongoDB instance running
- BSV wallet for blockchain interactions

### Environment Setup

Create a `.env.local` file:

```
MONGODB_URI=your_mongodb_connection_string
```

### Running Locally

```bash
npm install
npm run dev
```

Access the application at [http://localhost:3000/](http://localhost:3000/)

Click "Connect Wallet" in the navbar to begin using the application.

---

## Usage Guide

### Creating Action Chains

1. Navigate to the Create section
2. Enter a title for your action chain
3. Optionally select from pre-built templates:
   - Soil to Table (agricultural supply chain)
   - Plastic Product Lifecycle
   - Aircraft Parts Lifecycle
4. Add stages with descriptions (minimum 2 required)
5. Finalize the chain to commit to blockchain

### Transferring Chains

1. Specify receiver's public key when creating/continuing a chain
2. Title is required for transfer validation
3. Chain ownership transfers to receiver
4. Original lock is deleted, transfer record created

### Receiving Chains

1. View pending chains in the "Received" page inbox
2. Click on chains to view details
3. Continue chains by adding new stages
4. Choose to keep or transfer to another user

### Viewing Stage Details

- Click on stage cards for detailed information
- Expand view for full-screen modal with complete transaction history
- Access blockchain transaction details for verification

---

## API Routes

The application includes eight primary API endpoints:

- Stage management operations
- Chain creation and finalization
- Lock management for ownership
- Transfer tracking and updates

See the repository for complete API documentation.

---

## Security Features

- PushDrop protocol with AES encryption for on-chain data
- Cryptographic ownership verification through locks
- Public key authentication for transfers
- Blockchain-based immutable audit trail

**Important Security Note**: This is a demo implementation and should not be considered cryptographically secure for production use. The encryption is not suitable for real or sensitive supply chain data.

---

## Templates

### Soil to Table
Multi-stage agricultural supply chain workflow

### Plastic Product Lifecycle
Tracking plastic products from production through recycling

### Aircraft Parts Lifecycle
Managing aircraft components through manufacturing, testing, and certification

---

## Testing & Validation

- Test chain creation with minimum stage requirements
- Verify ownership tracking through locks collection
- Validate transfer functionality with receiver keys
- Confirm blockchain transaction recording for each stage
- Test title validation for transfers
- Verify pending count auto-refresh functionality

---

## Troubleshooting & Support

### Common Issues

- Ensure MongoDB URI is correctly set in `.env.local`
- Verify wallet connection in navbar
- Check that chains have minimum 2 stages before finalizing
- Confirm title is provided when transferring chains
- Review browser console for error messages

### Blockchain Connectivity

- Verify BSV wallet connectivity
- Check transaction status on blockchain explorer
- Ensure sufficient balance for transaction fees

---

## Resources

- **Business Documentation:** [business-dynamic-supplychains.md](business-dynamic-supplychains.md)
- **Repository:** [GitHub - bsv-blockchain-demos/dynamic-supplychains](https://github.com/bsv-blockchain-demos/dynamic-supplychains)
- **Live Demo:** [https://dynamic-supplychains.bsvb.tech](https://dynamic-supplychains.bsvb.tech)

---

*Refer to the business document for use cases and operational scenarios.*
