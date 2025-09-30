# Preimage - Technical Documentation

**Demo ID**: `preimage`
**Version**: `1.0.0`
**Last Updated**: `September 30, 2025`

## Architecture Overview

### System Architecture
Preimage is an early-stage web application built using modern web technologies through the Lovable development platform. It is designed to parse BSV transaction preimages and provide analysis of their components, though core parsing functionality is still in development.

### Technology Stack
- **Frontend**: React, TypeScript
- **UI Framework**: shadcn-ui
- **Styling**: Tailwind CSS
- **Build Tool**: Vite
- **Development Platform**: Lovable
- **Infrastructure**: Static web hosting

### Planned Components
1. **Preimage Parser**: Core engine to decode transaction preimage structure (to be implemented)
2. **Script Analyzer**: Convert script code to Assembly format
3. **Component Inspector**: Display preimage parts (nVersion, hashPrevouts, etc.)
4. **User Interface**: Modern React-based interface using shadcn-ui

## Integration & APIs

### External Dependencies
| Service | Purpose | Version | Documentation |
|---------|---------|---------|---------------|
| @bsv/sdk | BSV blockchain operations | Latest | [BSV SDK Docs](https://docs.bsvblockchain.org/) |
| React | UI framework | 18+ | [React Docs](https://react.dev/) |
| sCrypt | Smart contract framework | Latest | [sCrypt Docs](https://scrypt.io/docs/) |

### Preimage Structure Components
```
SigHashPreimage Structure:
- nVersion: Transaction version
- hashPrevouts: Hash of all input outpoints
- hashSequence: Hash of all input sequences
- outpoint: Current input's outpoint
- scriptCode: Script being executed
- amount: Input amount in satoshis
- nSequence: Input sequence number
- hashOutputs: Hash of all outputs
- nLocktime: Transaction lock time
- sighashType: Signature hash type
```

## Implementation Guide

### Prerequisites
- Understanding of BSV transaction structure
- Knowledge of sCrypt smart contract development
- Familiarity with Bitcoin script and Assembly language
- Access to transaction preimage data

### Setup Instructions

```bash
# Clone repository
git clone https://github.com/bsv-blockchain-demos/preimage.git

# Navigate to project directory
cd preimage

# Install dependencies
npm i

# Start development server
npm run dev
```

### Alternative Development Options
- Edit through Lovable platform
- Use preferred IDE
- Edit directly on GitHub
- Use GitHub Codespaces

### Usage Instructions

#### Planned Preimage Inspection Workflow

1. **Input Preimage Data**: Interface for pasting transaction preimage hex strings
   ```
   Example preimage hex (provided for testing):
   010000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000055eb4087a819b4025925c2579ad2c0d32fc40a7bc7a7f960372d6a9c84c28afa00000000d10063036f726451126170706c69636174696f6e2f6273762d3230004c777b2270223a226273762d3230222c226f70223a227472616e73666572222c226964223a22616535396633623839386563363161636264623663633761323435666162656465643063303934626630343666333532303661336165633630656638383132375f30222c22616d74223a22313030303030227d6876a914c1f91f5d381fdfda766de28b760f5627d1768da488ad21020a177d6a5e6f3a8689acd2e313bd1cf0dcf5a243d1cc67b7218602aee9e04b2fac0100000000000000ffffffff9ca748ade9f40fef35f1499e8d024f3d5a80bcac2bff794115d02c396d54adc500000000c1000000
   ```

   **Preimage Components Structure:**
   - nVersion: 01000000 (4 bytes)
   - hashPrevouts: 0000000000000000... (32 bytes)
   - hashSequence: 0000000000000000... (32 bytes)
   - outpoint: 55eb4087a819b402... (36 bytes)
   - scriptCode: d10063036f726451... (variable length)
   - amount: 0100000000000000 (8 bytes little-endian)
   - nSequence: ffffffff (4 bytes)
   - hashOutputs: 9ca748ade9f40fef... (32 bytes)
   - nLocktime: 00000000 (4 bytes)
   - sighashType: c1000000 (4 bytes)

2. **Parse and Analyze**: Click "Inspect Preimage" to decode the structure

3. **View Component Breakdown**:
   - Individual component values and interpretations
   - Script code analysis and ASM conversion
   - Hash verification and validation
   - Signature type identification

#### Script Code to Assembly Conversion

1. **Input Script Code**: Paste raw script code hex from the preimage
2. **Convert to ASM**: View human-readable Assembly representation
3. **Analyze Operations**: Understand script execution flow and opcodes

#### Debugging Workflow

1. **Compare Preimages**: Side-by-side comparison for debugging discrepancies
2. **Identify Issues**: Highlight differences in script code or hash values
3. **Validate Components**: Verify each preimage component for correctness
4. **Export Analysis**: Save detailed breakdown for further investigation

## Testing & Validation

### Test Coverage
- **Unit Tests**: Preimage parsing logic validation
- **Integration Tests**: sCrypt contract integration testing
- **Manual Testing**: Real-world debugging scenarios

### Development Goals
- [ ] Implement transaction preimage parsing functionality
- [ ] Create user interface for preimage input and component display
- [ ] Add script code to Assembly conversion
- [ ] Integrate BSV SDK for proper preimage handling
- [ ] Add debugging workflows for common issues
- [ ] Handle various preimage formats and edge cases
- [ ] Ensure client-side processing for privacy

## Performance & Scalability

### Current Metrics
- **Parse Time**: < 500ms for complex preimages
- **Memory Usage**: Efficient handling of large script codes
- **Client-side Processing**: No server dependencies for enhanced security

### Scalability Considerations
- Client-side architecture supports unlimited concurrent users
- Optimized parsing algorithms handle complex script structures
- No backend infrastructure reduces operational complexity

## Maintenance & Support

### Monitoring
- **Logs**: Client-side error logging for debugging
- **Metrics**: Usage analytics for feature optimization
- **Alerts**: Error boundary implementation for graceful failure handling

### Troubleshooting
| Issue | Cause | Solution |
|-------|-------|----------|
| Invalid preimage format | Malformed input data | Validate preimage structure and component sizes |
| Script parsing error | Unsupported opcodes | Check script version and opcode compatibility |
| Hash mismatch | Incorrect component ordering | Verify little-endian/big-endian format consistency |
| checkSig failure debug | Script code discrepancy | Compare expected vs actual script code in preimage |

## Advanced Features

### Preimage Comparison Tool
- Side-by-side analysis of multiple preimages
- Automated difference highlighting
- Component-by-component validation

### Script Analysis Engine
- Complete opcode interpretation
- Stack simulation for script execution
- Execution path visualization

### sCrypt Integration
- Direct import from sCrypt debugging sessions
- Integration with sCrypt IDE tools
- Automated issue detection for common problems

### Export Capabilities
- JSON export of parsed preimage data
- CSV export for analysis tools
- Assembly code export for documentation

## Debugging Best Practices

### Common Issues and Solutions

1. **checkSig Exceptions**:
   - Verify script code matches expected locking script
   - Check signature encoding and hash type
   - Validate public key and signature format

2. **checkPreimage Failures**:
   - Compare MD5 values of script code components
   - Ensure correct input ordering and amounts
   - Verify hash computations for all components

3. **Script Code Inconsistencies**:
   - Remove executed opcodes before OP_CHECKSIG
   - Handle conditional script execution paths
   - Account for script version differences

## Resources

- **Repository**: [GitHub - Preimage](https://github.com/bsv-blockchain-demos/preimage)
- **Development Platform**: [Lovable Project](https://lovable.dev/projects/15e64fbb-0d3d-4aa6-b406-1032ae493e8f)
- **Business Documentation**: [Business Preimage Documentation](./business-preimage.md)
- **sCrypt Documentation**: [sCrypt Developer Guide](https://scrypt.io/docs/)
- **BSV Script Reference**: [BSV Script Documentation](https://docs.bsvblockchain.org/important-concepts/details/script)
- **Support Contact**: BSV Blockchain Demos Team

---
*For business context and ROI details, see: [Business Preimage Documentation](./business-preimage.md)*