# Pollr - Technical Documentation

**Demo ID**: `pollr`
**Version**: `0.1.1`
**Last Updated**: `October 15, 2025`

## Architecture Overview

### System Architecture
Pollr is a full-stack decentralized polling application built on the BSV blockchain. The architecture consists of a React frontend for user interactions and a backend service for managing poll data and blockchain operations. The system leverages MetaNet Client for wallet integration and stores poll results immutably on the BSV blockchain.

### Technology Stack
- **Frontend**: React 18.3.1, TypeScript 5.8.3, Material-UI 7.0.2
- **Backend**: Node.js with TypeScript 5.2.2, MongoDB 6.11.0
- **Blockchain SDK**: @bsv/sdk v1.6.22 (backend), v1.7.6 (frontend)
- **Build Tools**: Webpack 5.74.0, Babel
- **Infrastructure**: MetaNet Client overlay network

### Key Components
1. **Polling Interface**: React-based UI for creating, viewing, and participating in polls
2. **Wallet Integration**: MetaNet Client (MNC) authentication and blockchain operations
3. **Backend API**: RESTful services for poll management and data persistence
4. **Database Layer**: MongoDB for poll storage and user interactions
5. **Blockchain Overlay**: @bsv/overlay for decentralized data management

## Integration & APIs

### External Dependencies
| Service | Purpose | Version | Documentation |
|---------|---------|---------|---------------|
| @bsv/sdk | BSV blockchain operations | v1.6.22/v1.7.6 | [BSV SDK Docs](https://docs.bsvblockchain.org/) |
| @bsv/overlay | Overlay services integration | v0.4.6 | [Overlay Documentation](https://docs.bsvblockchain.org/) |
| @bsv/uhrp-react | Universal Hash Resolution Protocol | Latest | [UHRP Docs](https://docs.bsvblockchain.org/) |
| MongoDB | Database for poll storage | v6.11.0 | [MongoDB Docs](https://docs.mongodb.com/) |
| Material-UI | UI component library | v7.0.2 | [MUI Docs](https://mui.com/) |

### Key Application Routes
- `/` - Active Polls (browsing current polls)
- `/create` - Create New Poll
- `/my-polls` - Personal Poll Management
- `/completed` - Completed Polls Archive
- `/poll/:id` - Individual Poll Details

## Implementation Guide

### Prerequisites
- Node.js (v18 or higher)
- npm package manager
- MongoDB instance (local or cloud)
- MetaNet Client browser extension
- BSV blockchain concepts knowledge
- LARS and CARS CLI tools for deployment

### Setup Instructions

```bash
# Clone repository
git clone https://github.com/p2ppsr/Pollr.git

# Navigate to project directory
cd Pollr

# Install dependencies
npm i

# Configure local environment (LARS)
npm run lars

# Start development server
npm run start

# Build for production
npm run build

# Deploy to cloud (CARS)
npm run deploy
```

### Configuration

Key configuration parameters:
```typescript
// Frontend configuration
interface PollrConfig {
  metanetRequired: boolean
  overlayNetwork: string
  apiEndpoint: string
}

// Backend configuration
interface BackendConfig {
  mongodbUri: string
  bsvNetwork: 'mainnet' | 'testnet'
  overlayServices: string[]
}
```

### Browser Access Note
If encountering host access issues, use:
```bash
brave-browser --disable-web-security --user-data-dir="/tmp/brave_dev"
```

## Testing & Validation

### Test Coverage
- **Unit Tests**: Backend services (Jest framework)
- **Integration Tests**: Blockchain operations and wallet integration
- **Manual Testing**: Poll creation, voting flows, and result verification

### Validation Criteria
- [x] MetaNet Client wallet authentication
- [x] Poll creation and submission to blockchain
- [x] Vote casting and immutable recording
- [x] Real-time poll result updates
- [x] Completed poll archival
- [ ] Multi-choice poll options
- [ ] Advanced poll analytics

## Performance & Scalability

### Current Metrics
- **Response Time**: < 500ms for poll interactions
- **Blockchain Write**: Standard BSV transaction time
- **Resource Usage**: Optimized React rendering with Material-UI
- **Database Queries**: Indexed MongoDB operations

### Scalability Considerations
- Frontend scales horizontally with static hosting
- Backend requires load balancing for high traffic
- MongoDB sharding for large poll datasets
- Blockchain overlay services for decentralized scaling
- Consider caching layer for frequently accessed polls

## Maintenance & Support

### Monitoring
- **Logs**: Server-side logging for backend operations
- **Metrics**: MetaNet Client transaction monitoring
- **Alerts**: Database connection and blockchain service health checks

### Troubleshooting
| Issue | Cause | Solution |
|-------|-------|----------|
| MetaNet Client not detected | Extension not installed | Install MNC browser extension and refresh |
| Transaction rejection | Local topical hosts issue | Use alternative browser configuration or network |
| Poll not loading | Database connection error | Check MongoDB URI and connection status |
| Vote not recording | Blockchain transaction failure | Verify wallet balance and network connectivity |

## Production Considerations

### Security
- Implement rate limiting on poll creation
- Validate vote authenticity using blockchain signatures
- Secure MongoDB with proper authentication
- Use environment variables for sensitive configuration

### Best Practices
- Implement proper error boundaries in React
- Add comprehensive logging for debugging
- Use TypeScript strict mode for type safety
- Regular security audits of smart contract interactions

## Resources

- **Repository**: [GitHub - p2ppsr/Pollr](https://github.com/p2ppsr/Pollr)
- **Business Documentation**: [Business Pollr Documentation](./business-pollr.md)
- **Live Demo**: [pollr.gg](https://pollr.gg/)
- **BSV SDK Documentation**: [BSV Blockchain SDK](https://docs.bsvblockchain.org/)
- **Support Contact**: p2ppsr Development Team

---
*For business context and ROI details, see: [Business Pollr Documentation](./business-pollr.md)*
