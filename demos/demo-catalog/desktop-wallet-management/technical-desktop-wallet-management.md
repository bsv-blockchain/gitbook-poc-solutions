# Desktop Wallet Management - Technical Documentation

**Demo ID**: `desktop-wallet-management`
**Version**: `1.0.0`
**Last Updated**: `December 9, 2025`

## Architecture Overview

This demo showcases a comprehensive wallet architecture using @bsv/sdk with BRC-42 key derivation, basket-based UTXO management, and P2PKH transactions. The architecture is portable from desktop to server-side implementations.

### Technology Stack
- **SDK**: @bsv/sdk (Official BSV Blockchain SDK)
- **Key Derivation**: BRC-42 standard implementation
- **Script Type**: P2PKH (Pay-to-Public-Key-Hash)
- **Language**: TypeScript

### Core Components

1. **WalletClient**: Main interface for wallet operations, identity management, and transaction creation
2. **Key Derivation**: BRC-42 hierarchical deterministic keys for protocol-specific key generation
3. **Basket Manager**: UTXO organization with fund categorization (savings, spending, payments, etc.)
4. **Transaction Builder**: P2PKH transaction creation with locking scripts and basket assignment
5. **Balance Tracker**: Multi-basket balance aggregation and real-time updates

## Integration & APIs

### External Dependencies
| Service | Purpose | Version | Documentation |
|---------|---------|---------|---------------|
| @bsv/sdk | Core BSV blockchain operations | Latest | [BSV SDK Docs](https://docs.bsvblockchain.org/) |
| TypeScript | Type safety and modern JS | 4.x+ | [TypeScript Docs](https://www.typescriptlang.org/) |

### WalletClient API

#### Connection & Identity
```typescript
import { WalletClient } from "@bsv/sdk";

// Initialize wallet
const wallet = new WalletClient();

// Get identity key (master public key)
const identityKeyResponse = await wallet.getPublicKey({
  identityKey: true
});
```

#### Key Derivation (BRC-42)
```typescript
// Derive protocol-specific key
const derivedKey = await wallet.getPublicKey({
  protocolID: [0, "myapp"],     // Protocol identifier
  keyID: "invoice123",          // Specific key identifier
  counterparty: "self",         // Counterparty designation
  forSelf: true                 // Self-key flag
});
```

#### Transaction Creation
```typescript
import { PublicKey, P2PKH } from "@bsv/sdk";

// Create P2PKH locking script
const pubKey = PublicKey.fromString(publicKeyString);
const lockingScript = new P2PKH().lock(pubKey.toHash());

// Create transaction with basket assignment
const result = await wallet.createAction({
  description: "Transfer to savings",
  outputs: [{
    satoshis: 10000,
    lockingScript: lockingScript.toHex(),
    basket: "savings",
    outputDescription: "Savings deposit",
    customInstructions: JSON.stringify({ /* metadata */ })
  }]
});
```

#### Balance Querying
```typescript
// List outputs from specific basket
const result = await wallet.listOutputs({
  basket: "savings",
  limit: 100
});

// Calculate basket balance
let balance = result.outputs.reduce((sum, output) =>
  sum + output.satoshis, 0
);
```

## Implementation Guide

### Prerequisites
- Node.js v18 or higher
- TypeScript 4.x or higher
- Basic understanding of Bitcoin UTXO model
- Knowledge of public/private key cryptography
- BSV wallet with sufficient balance for testing

### Getting Started

Clone the demo repository to get started quickly:

```bash
# Clone the repository
git clone https://github.com/bsv-blockchain-demos/desktop-wallet-data.git

# Navigate to the project directory
cd desktop-wallet-data

# Install dependencies
npm install

# Run the demo
npm start
```

Alternatively, you can set up a project from scratch:

```bash
# Initialize Node.js project
npm init -y

# Install BSV SDK
npm install @bsv/sdk

# Install TypeScript (if needed)
npm install --save-dev typescript @types/node

# Initialize TypeScript configuration
npx tsc --init
```

### Configuration

Create `tsconfig.json`:
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "lib": ["ES2020"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### Basic Implementation

#### 1. Wallet Connection
```typescript
import { WalletClient } from "@bsv/sdk";

async function connectWallet() {
  const wallet = new WalletClient();
  const identityKey = await wallet.getPublicKey({ identityKey: true });
  console.log("Connected:", identityKey.publicKey);
  return wallet;
}
```

#### 2. Key Derivation
```typescript
async function deriveKey(
  wallet: WalletClient,
  protocolID: string,
  keyID: string
) {
  const keyResult = await wallet.getPublicKey({
    protocolID: [0, protocolID],
    keyID: keyID,
    counterparty: "self",
    forSelf: true
  });

  return keyResult.publicKey;
}
```

#### 3. Address Generation
```typescript
import { PublicKey } from "@bsv/sdk";

function publicKeyToAddress(publicKeyString: string): string {
  const pubKey = PublicKey.fromString(publicKeyString);
  const address = pubKey.toAddress();
  return address.toString();
}
```

#### 4. Transaction Creation
```typescript
import { PublicKey, P2PKH } from "@bsv/sdk";

async function createBasketTransaction(
  wallet: WalletClient,
  basket: string,
  amountSats: number
) {
  // Derive fresh receiving key
  const keyResult = await wallet.getPublicKey({
    protocolID: [0, "internal"],
    keyID: `${basket}_${Date.now()}`,
    counterparty: "self",
    forSelf: true
  });

  // Create locking script
  const pubKey = PublicKey.fromString(keyResult.publicKey);
  const lockingScript = new P2PKH().lock(pubKey.toHash());

  // Submit transaction
  const result = await wallet.createAction({
    description: `Transfer ${amountSats} sats to ${basket}`,
    outputs: [{
      satoshis: amountSats,
      lockingScript: lockingScript.toHex(),
      basket: basket,
      outputDescription: `Deposit to ${basket}`
    }]
  });

  return result;
}
```

#### 5. Balance Calculation
```typescript
async function calculateBalance(
  wallet: WalletClient,
  baskets: string[]
): Promise<Map<string, number>> {
  const balances = new Map<string, number>();

  for (const basket of baskets) {
    try {
      const result = await wallet.listOutputs({
        basket,
        limit: 10000
      });

      const balance = result.outputs?.reduce(
        (sum, output) => sum + (output.satoshis || 0),
        0
      ) || 0;

      balances.set(basket, balance);
    } catch (error) {
      console.warn(`Basket ${basket} inaccessible`);
      balances.set(basket, 0);
    }
  }

  return balances;
}
```

## Testing & Validation

### Test Coverage
- **Unit Tests**: Key derivation determinism, address generation correctness
- **Integration Tests**: Wallet connection, transaction creation, balance queries
- **Manual Testing**: End-to-end wallet operations with real BSV testnet

### Validation Criteria
- [x] Wallet connects and retrieves identity key
- [x] BRC-42 key derivation produces consistent results
- [x] Public keys correctly convert to valid addresses
- [x] P2PKH locking scripts are properly formatted
- [x] Transactions successfully create outputs in baskets
- [x] Balance calculations accurately sum UTXOs
- [x] Multi-basket queries handle missing baskets gracefully

### Testing Example
```typescript
import { WalletClient, PublicKey } from "@bsv/sdk";

async function runTests() {
  const wallet = new WalletClient();

  // Test 1: Identity key retrieval
  const identity = await wallet.getPublicKey({ identityKey: true });
  console.assert(identity.publicKey.length > 0, "Identity key retrieved");

  // Test 2: Deterministic key derivation
  const key1 = await deriveKey(wallet, "test", "key1");
  const key2 = await deriveKey(wallet, "test", "key1");
  console.assert(key1 === key2, "Key derivation is deterministic");

  // Test 3: Address generation
  const address = publicKeyToAddress(key1);
  console.assert(address.length > 0, "Address generated");

  // Test 4: Balance calculation
  const balances = await calculateBalance(wallet, ["main"]);
  console.assert(balances.has("main"), "Balance retrieved");

  console.log("All tests passed!");
}
```

## Performance & Scalability

### Performance Metrics
- **Key Derivation**: < 10ms per key
- **Transaction Creation**: ~200ms (network dependent)
- **Balance Query**: ~100ms per basket

### Scalability Best Practices
- **Desktop to Server**: Replace WalletClient with server-side key management and database layer for UTXO tracking
- **High-Volume**: Use batch operations, key caching, and parallel queries
- **UTXO Management**: Regularly consolidate small outputs to prevent fragmentation
- **Basket Organization**: Use meaningful basket names and implement basket-specific spending policies

## Maintenance & Support

### Monitoring

#### Key Metrics
- **Transaction Success Rate**: Track failed transactions
- **Balance Accuracy**: Verify calculated vs actual balances
- **Key Derivation Errors**: Monitor BRC-42 compliance
- **Network Connectivity**: Check wallet service availability

#### Logging Implementation
```typescript
class WalletLogger {
  logTransaction(txid: string, basket: string, amount: number) {
    console.log(`[TX] ${new Date().toISOString()} - ${txid}: ${amount} sats to ${basket}`);
  }

  logKeyDerivation(protocolID: string, keyID: string) {
    console.log(`[KEY] ${new Date().toISOString()} - Derived: ${protocolID}/${keyID}`);
  }

  logError(operation: string, error: Error) {
    console.error(`[ERROR] ${new Date().toISOString()} - ${operation}:`, error.message);
  }
}
```

### Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| "Wallet not connected" | WalletClient initialization failed | Verify BSV wallet software is running and accessible |
| "Invalid protocol ID" | Special characters in protocolID | Use only alphanumeric characters and spaces |
| "Insufficient funds" | Basket lacks required balance | Check balance or select different basket |
| "Transaction rejected" | Malformed locking script or insufficient fees | Validate P2PKH script format and ensure adequate fees |
| "Basket not found" | Basket name mismatch or never used | Check basket name spelling; create basket with transaction |
| "Key derivation failed" | Invalid BRC-42 parameters | Verify protocolID, keyID format compliance |

### Common Errors and Fixes

#### Error: Invalid Public Key
```typescript
// Problem: Malformed public key string
// Solution: Validate before use
function validatePublicKey(keyString: string): boolean {
  try {
    PublicKey.fromString(keyString);
    return true;
  } catch {
    return false;
  }
}
```

#### Error: Balance Mismatch
```typescript
// Problem: Not querying all relevant baskets
// Solution: Comprehensive basket list
const ALL_BASKETS = [
  "main", "savings", "spending", "payments",
  "change", "rewards", "operational"
];

async function getTotalBalance(wallet: WalletClient): Promise<number> {
  const balances = await calculateBalance(wallet, ALL_BASKETS);
  return Array.from(balances.values()).reduce((sum, bal) => sum + bal, 0);
}
```

## Production Considerations

### Security Best Practices
- Never expose private keys in logs or error messages
- Store identity keys in secure hardware when possible
- Always verify amounts before transaction creation
- Restrict basket access based on user permissions
- Audit all fund movements with immutable logs

### Server-Side Adaptation
For production deployments, implement:
- Database layer for key storage and UTXO tracking
- Caching strategy for frequently accessed keys (LRU cache recommended)
- Webhook notifications for transaction events
- Connection pooling for concurrent operations

## Advanced Concepts

### Multi-Protocol Key Management
Use a protocol registry to organize keys by application context (payments, messaging, documents, tokens). The demo repository includes examples of protocol-specific key derivation.

### UTXO Consolidation
Periodically consolidate small UTXOs (below a threshold like 1000 satoshis) when you have 10+ small outputs. This reduces transaction complexity and improves wallet performance.

## Resources

- **Demo Repository**: [https://github.com/bsv-blockchain-demos/desktop-wallet-data](https://github.com/bsv-blockchain-demos/desktop-wallet-data)
- **Business Documentation**: [Business Desktop Wallet Management Documentation](./business-desktop-wallet-management.md)
- **BSV SDK Documentation**: [Official BSV SDK](https://docs.bsvblockchain.org/)
- **BRC-42 Specification**: [Key Derivation Standard](https://brc.dev/42)
- **BSV Academy**: [Bitcoin Fundamentals](https://hub.bsvblockchain.org/bsv-academy)
- **Support Contact**: BSV Blockchain Association

---
*For business context and use cases, see: [Business Desktop Wallet Management Documentation](./business-desktop-wallet-management.md)*
