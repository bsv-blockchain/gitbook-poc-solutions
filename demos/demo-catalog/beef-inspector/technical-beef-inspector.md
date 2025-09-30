# Beef Inspector - Technical Documentation

**Demo ID**: `beef-inspector`
**Version**: `1.0.0`
**Last Updated**: `September 30, 2025`

## Architecture Overview

### System Architecture
Beef Inspector is a web-based application that parses BEEF (Background Evaluation Extended Format) hex payloads and presents the embedded transaction data in a human-readable format. The architecture focuses on client-side parsing to ensure security and privacy of sensitive transaction data.

### Technology Stack
- **Frontend**: React/TypeScript for interactive interface
- **Parsing Engine**: Custom BEEF parser using BSV SDK
- **Visualization**: Chart libraries for transaction ancestry graphs
- **Infrastructure**: Static web hosting (client-side only)

### Key Components
1. **BEEF Parser**: Core engine that decodes binary BEEF format into structured data
2. **Transaction Viewer**: Displays individual transaction details and inputs/outputs
3. **SPV Proof Inspector**: Visualizes Simplified Payment Verification data
4. **Ancestry Graph**: Shows transaction chain relationships

## Integration & APIs

### External Dependencies
| Service | Purpose | Version | Documentation |
|---------|---------|---------|---------------|
| @bsv/sdk | BSV blockchain operations | Latest | [BSV SDK Docs](https://docs.bsvblockchain.org/) |
| React | UI framework | 18+ | [React Docs](https://react.dev/) |
| Chart.js | Data visualization | Latest | [Chart.js Docs](https://www.chartjs.org/) |

### BEEF Format Structure
```
BEEF Format Components:
- Version (4 bytes)
- BUMPs (BSV Unified Merkle Paths)
- Transaction data with ancestry
- SPV proofs for verification
```

## Implementation Guide

### Prerequisites
- Understanding of BEEF format structure
- BSV blockchain transaction knowledge
- Basic web development skills
- Access to BEEF hex payload samples

### Setup Instructions

```bash
# Clone repository
git clone https://github.com/bsv-blockchain-demos/beef-inspector.git

# Navigate to project directory
cd beef-inspector

# Install dependencies
npm install

# Start development server
npm run dev
```

### Usage Instructions

#### Inspecting BEEF Hex Payloads

1. **Paste BEEF Hex String**: Copy your BEEF hex payload into the input field
   ```
   Example BEEF hex:
   0100beef01fe636d0c0007021400fe507c0c7aa754cef1f7889d5fd395cf1f785dd7de98eed895dbedfe4e5bc70d1502ac4e164f5bc16746bb0868404292ac8318bbac3800e4aad13a014da427adce3e010b00bc4ff395efd11719b277694cface5aa50d085a0bb81f613f70313acd28cf4557010400574b2d9142b8d28b61d88e3b2c3f44d858411356b49a28a4643b6d1a6a092a5201030051a05fc84d531b5d250c23f4f886f6812f9fe3f402d61607f977b4ecd2701c1901000
   ```

2. **Parse and Inspect**: Click "Parse BEEF" to decode the hex data

3. **View Results**:
   - Transaction details (inputs, outputs, fees)
   - SPV proof information
   - Merkle path verification
   - Ancestry chain visualization

#### Key Features

- **Transaction Breakdown**: View individual transaction components
- **Endianness Handling**: Automatic handling of little-endian/big-endian format differences
- **Proof Verification**: Validate SPV proofs against merkle roots
- **Data Export**: Export parsed data in JSON format

## Testing & Validation

### Test Coverage
- **Unit Tests**: BEEF parsing logic validation
- **Integration Tests**: End-to-end hex payload processing
- **Manual Testing**: Real-world BEEF payload verification

### Validation Criteria
- [ ] Successfully parse valid BEEF hex payloads
- [ ] Display accurate transaction data and ancestry
- [ ] Correctly verify SPV proofs
- [ ] Handle invalid or malformed hex inputs gracefully
- [ ] Maintain data privacy (client-side only processing)

## Performance & Scalability

### Current Metrics
- **Parse Time**: < 1 second for typical BEEF payloads
- **Memory Usage**: Efficient handling of large transaction chains
- **Client-side Processing**: No server dependencies for enhanced privacy

### Scalability Considerations
- Client-side architecture ensures unlimited concurrent users
- No backend infrastructure reduces operational complexity
- Can handle large BEEF payloads through optimized parsing algorithms

## Maintenance & Support

### Monitoring
- **Logs**: Client-side error logging and debugging
- **Metrics**: Usage analytics for feature optimization
- **Alerts**: Error boundary implementation for graceful failure handling

### Troubleshooting
| Issue | Cause | Solution |
|-------|-------|----------|
| Parse error on valid BEEF | Endianness format issue | Verify hex string format and try alternate endianness |
| Invalid hex format | Malformed input data | Validate hex string contains only valid characters (0-9, A-F) |
| Missing transaction data | Incomplete BEEF payload | Ensure full BEEF structure is provided including ancestry |
| SPV verification fails | Invalid merkle proofs | Check block header data and merkle path consistency |

## Advanced Features

### Transaction Ancestry Graph
- Visual representation of transaction relationships
- Interactive exploration of transaction chains
- Identification of transaction dependencies

### SPV Proof Analysis
- Merkle path verification
- Block header validation
- Proof completeness checking

### Export Capabilities
- JSON export of parsed transaction data
- CSV export for analysis tools
- Raw hex data extraction

## Resources

- **Repository**: [GitHub - Beef Inspector](https://github.com/bsv-blockchain-demos/beef-inspector)
- **Business Documentation**: [Business Beef Inspector Documentation](./business-beef-inspector.md)
- **BEEF Format Specification**: [BSV BEEF Documentation](https://docs.bsvblockchain.org/guides/sdks/concepts/beef)
- **BSV SDK Reference**: [BSV Blockchain SDK](https://docs.bsvblockchain.org/)
- **Support Contact**: BSV Blockchain Demos Team

---
*For business context and ROI details, see: [Business Beef Inspector Documentation](./business-beef-inspector.md)*