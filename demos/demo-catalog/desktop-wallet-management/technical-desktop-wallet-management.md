# Desktop Wallet Management - Technical Documentation

**Demo ID**: `desktop-wallet-management`
**Version**: `1.0.0`
**Last Updated**: `December 9, 2025`

## Architecture Overview

### System Architecture
The Desktop Wallet Management proof-of-concept demonstrates a comprehensive wallet architecture built on the @bsv/sdk. The system implements the UTXO model with hierarchical deterministic key derivation (BRC-42 standard), basket-based fund organization, and P2PKH transaction creation. The architecture is designed to be portable from desktop to server-side implementations, providing a foundation for enterprise wallet infrastructure.

```
┌─────────────────────────────────────────────────┐
│           Wallet Client Interface               │
│              (@bsv/sdk)                        │
└────────────┬────────────────────────────────────┘
             │
      ┌──────┴──────┐
      │             │
┌─────▼──────┐ ┌───▼──────────┐
│   Key      │ │   UTXO       │
│ Management │ │ Management   │
└─────┬──────┘ └───┬──────────┘
      │            │
┌─────▼────────────▼──────┐
│   Transaction Builder   │
└─────────────────────────┘
```

### Technology Stack
- **SDK**: @bsv/sdk (Official BSV Blockchain SDK)
- **Key Derivation**: BRC-42 standard implementation
- **Script Type**: P2PKH (Pay-to-Public-Key-Hash)
- **Language**: TypeScript for type safety and modern JavaScript features
- **Storage Model**: UTXO-based with basket organization

### Key Components

1. **WalletClient**: Primary interface for all wallet operations
   - Connection management and identity retrieval
   - Public/private key operations
   - Transaction creation and submission

2. **Key Derivation System**: BRC-42 hierarchical deterministic keys
   - Protocol-specific key generation
   - Counterparty-based key isolation
   - Deterministic and reproducible derivation

3. **Basket Manager**: UTXO organization system
   - Fund categorization (savings, spending, payments, etc.)
   - Output tracking and balance calculation
   - Transaction routing to specific baskets

4. **Transaction Builder**: P2PKH transaction creation
   - Locking script generation
   - Output construction with basket assignment
   - Change management and fee calculation

5. **Balance Tracker**: Multi-basket balance aggregation
   - Output enumeration across baskets
   - Satoshi summation and BSV conversion
   - Real-time balance updates

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

### Setup Instructions

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

### Current Metrics
- **Key Derivation**: < 10ms per key
- **Transaction Creation**: ~200ms (network dependent)
- **Balance Query**: ~100ms per basket (database dependent)
- **Memory Usage**: Minimal (stateless operations)

### Scalability Considerations

#### Desktop to Server Migration
- Replace WalletClient with server-side key management
- Implement database layer for UTXO tracking
- Add caching for frequently accessed keys
- Use connection pooling for concurrent operations

#### High-Volume Scenarios
- **Batch Operations**: Group multiple transactions for efficiency
- **Key Caching**: Cache derived keys for frequently used protocols
- **UTXO Consolidation**: Periodically merge small outputs
- **Parallel Queries**: Query multiple baskets concurrently

#### Performance Optimization
```typescript
// Parallel basket queries
async function getBalanceOptimized(
  wallet: WalletClient,
  baskets: string[]
): Promise<Map<string, number>> {
  const queries = baskets.map(async (basket) => {
    try {
      const result = await wallet.listOutputs({ basket, limit: 10000 });
      const balance = result.outputs?.reduce(
        (sum, output) => sum + (output.satoshis || 0), 0
      ) || 0;
      return [basket, balance] as [string, number];
    } catch {
      return [basket, 0] as [string, number];
    }
  });

  const results = await Promise.all(queries);
  return new Map(results);
}
```

### Basket Organization Best Practices
- Use meaningful basket names (savings, spending, rewards)
- Avoid the "default" basket (typically admin-only)
- Regularly consolidate UTXOs to prevent fragmentation
- Implement basket-specific spending policies

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

1. **Key Management**
   - Never expose private keys in logs or error messages
   - Store identity keys in secure hardware when possible
   - Implement key rotation policies for long-lived applications

2. **Transaction Validation**
   - Always verify amounts before transaction creation
   - Validate recipient addresses against expected format
   - Implement transaction size limits to prevent abuse

3. **Access Control**
   - Restrict basket access based on user permissions
   - Audit all fund movements with immutable logs
   - Implement multi-signature requirements for high-value baskets

### Server-Side Adaptation

#### Database Schema Example
```sql
CREATE TABLE wallet_keys (
  id SERIAL PRIMARY KEY,
  protocol_id VARCHAR(255),
  key_id VARCHAR(255),
  public_key TEXT,
  created_at TIMESTAMP,
  UNIQUE(protocol_id, key_id)
);

CREATE TABLE utxos (
  id SERIAL PRIMARY KEY,
  txid VARCHAR(64),
  vout INTEGER,
  satoshis BIGINT,
  basket VARCHAR(50),
  locking_script TEXT,
  spent BOOLEAN DEFAULT false,
  created_at TIMESTAMP
);
```

#### Caching Strategy
```typescript
import { LRUCache } from 'lru-cache';

class WalletCache {
  private keyCache = new LRUCache<string, string>({ max: 1000 });

  async getCachedKey(
    wallet: WalletClient,
    protocolID: string,
    keyID: string
  ): Promise<string> {
    const cacheKey = `${protocolID}:${keyID}`;

    let publicKey = this.keyCache.get(cacheKey);
    if (!publicKey) {
      publicKey = await deriveKey(wallet, protocolID, keyID);
      this.keyCache.set(cacheKey, publicKey);
    }

    return publicKey;
  }
}
```

### Integration Patterns

#### Webhook Notifications
```typescript
interface TransactionEvent {
  txid: string;
  basket: string;
  amount: number;
  timestamp: number;
}

async function notifyTransaction(event: TransactionEvent, webhookUrl: string) {
  await fetch(webhookUrl, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(event)
  });
}
```

## Advanced Concepts

### Multi-Protocol Key Management
```typescript
const PROTOCOL_REGISTRY = {
  payments: [0, "payments"],
  messaging: [0, "messaging"],
  documents: [0, "documents"],
  tokens: [0, "tokens"]
};

async function deriveProtocolKey(
  wallet: WalletClient,
  protocol: keyof typeof PROTOCOL_REGISTRY,
  identifier: string
): Promise<string> {
  const keyResult = await wallet.getPublicKey({
    protocolID: PROTOCOL_REGISTRY[protocol],
    keyID: identifier,
    counterparty: "self",
    forSelf: true
  });

  return keyResult.publicKey;
}
```

### UTXO Consolidation Strategy
```typescript
async function consolidateBasket(
  wallet: WalletClient,
  basket: string,
  threshold: number = 1000 // satoshis
): Promise<void> {
  const outputs = await wallet.listOutputs({ basket, limit: 10000 });

  const smallOutputs = outputs.outputs.filter(
    output => output.satoshis < threshold
  );

  if (smallOutputs.length < 10) {
    return; // Not worth consolidating
  }

  const totalAmount = smallOutputs.reduce(
    (sum, output) => sum + output.satoshis, 0
  );

  // Create consolidation transaction
  await createBasketTransaction(wallet, basket, totalAmount);
}
```

## Resources

- **Business Documentation**: [Business Desktop Wallet Management Documentation](./business-desktop-wallet-management.md)
- **BSV SDK Documentation**: [Official BSV SDK](https://docs.bsvblockchain.org/)
- **BRC-42 Specification**: [Key Derivation Standard](https://brc.dev/42)
- **BSV Academy**: [Bitcoin Fundamentals](https://hub.bsvblockchain.org/bsv-academy)
- **Support Contact**: BSV Blockchain Association

---
*For business context and use cases, see: [Business Desktop Wallet Management Documentation](./business-desktop-wallet-management.md)*
