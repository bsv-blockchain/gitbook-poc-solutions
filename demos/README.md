# Demo Catalog

Comprehensive index of all Demo solutions with quick access to key information.

> **Note**: This catalog represents a subset of BSVA's Demos portfolio. Many Demos are still in development and not all are documented in this GitBook yet. BSVA is continuously incorporating new additions to expand the catalog.

> **Open Source Access**: All demos are freely accessible and can be used freely as they are open source. You can explore, fork, and build upon these implementations for your own projects.

## Active Demos

| Use Case | Demo Name | Description | Links | Last Updated |
|----------|-----------|-------------|-------|--------------|
| **Data Integrity & Hashing** | Truth Machine | This application demonstrates secure data integrity and timestamping on the BSV Blockchain. Upload a file, and its cryptographic hash is recorded on the blockchain, creating an immutable proof of existence at that exact time. Files are stored in a regular database and can be retrieved later with verifiable evidence of their creation date and integrity. The Treasury section enables token creation to fund transaction fees for the service. | [Business](demo-catalog/truth-machine/business-truth-machine.md) [Technical](demo-catalog/truth-machine/technical-truth-machine.md) | Aug 2025 |
| **Supply Chain Tracking** | Natural Chain | A natural gas supply chain proof of concept system leveraging the BSV blockchain that creates an incentivized data-provenance tracking for methane emissions across different supply chain stages. The demo shows how each party can view history, confirm/acknowledge receipt of items, triggering new transaction states with unique transaction IDs (txid). | [Business](demo-catalog/natural-chain/business-natural-chain.md) [Technical](demo-catalog/natural-chain/technical-natural-chain.md) | Aug 2025 |
| **Healthcare Tokenization** | Prescription Tokens | A prescription token proof-of-concept using BSV blockchain that creates secure, transferable prescription ownership. This demo serves as a precursor to identity solutions, built entirely with BSV stack components and demonstrating remote rootKey-based WalletClient over JSON RPC. | [Business](demo-catalog/prescription-tokens/business-prescription-tokens.md) [Technical](demo-catalog/prescription-tokens/technical-prescription-tokens.md) | Aug 2025 |
| **Medical Data Exchange** | P2P Medical | Peer-to-peer medical data exchange system demonstrating secure patient data sharing and interoperability between healthcare providers using BSV blockchain for data integrity and access control. | [Business](demo-catalog/p2p-medical/business-p2p-medical.md) [Technical](demo-catalog/p2p-medical/technical-p2p-medical.md) | Aug 2025 |
| **Digital Identity** | Register | Digital identity management system showcasing decentralized identity creation, verification, and management capabilities on BSV blockchain for secure user authentication and credential management. | [Business](demo-catalog/register/business-register.md) [Technical](demo-catalog/register/technical-register.md) | Aug 2025 |
| **Digital Receipts** | Digital Reciepts | This proof of concept tackles the environmental and economic waste of paper receipts by creating a BSV blockchain-powered digital receipt platform. Addressing the massive environmental impact—10 million trees and 21 billion gallons of water annually—plus billions in economic losses, the PoC demonstrates how blockchain can enable secure, immutable digital receipts. The platform connects payment systems with retailers to eliminate paper waste while creating new economic value through trusted collaboration and data integrity, positioning BSV as a scalable solution for everyday commerce transformation. | [Business](demo-catalog/digital-reciept/business-digital-reciept.md) [Technical](demo-catalog/digital-reciept/technical-digital-reciept.md)| Sep 2025 |
| **Community Knowledge** | Slack Threads | This proof of concept connects BSVA's internal Slack workspace with a public Q&A website using BSV micropayments. Valuable discussions from organizational Slack channels can be shared publicly, extending knowledge beyond internal teams. Users can reward helpful answers with BSV tips, demonstrating how blockchain micropayments create economic incentives for quality knowledge sharing while connecting private organizational discussions with broader developer communities. | [Business](demo-catalog/slack-threads/business-slack-threads.md) [Technical](demo-catalog/slack-threads/technical-slack-threads.md) | Sep 2025 |
| **Digital Identity Privacy** | Digital Trust Card  | This proof of concept addresses online safety challenges following the UK's new Online Safety legislation by creating a blockchain-powered age verification tool. The Digital Trust Card enables privacy-first age verification without revealing personal identity, empowering families to protect children online while maintaining control. By partnering with child safety organizations, privacy advocates, and educational institutions, this PoC demonstrates how blockchain can provide practical solutions to real-world regulatory compliance issues while positioning BSV as a thoughtful innovator in digital identity and privacy protection. | [Business](demo-catalog/digital-trust-card/business-digital-trust-card.md) | Sep 2025 |
| **Sovereign Data Control** | Data Ownership | This proof of concept integrates SOLID pods with BSV blockchain to create a decentralized personal knowledge management system. SOLID provides personal data vaults with user ownership, while BSV enables public timestamping, notarization, and micropayments. The demo shows users maintaining data control while leveraging BSV for proof of existence, governance, and immutable audit trails. Built as a "second brain" application using NextJS and W3C standards, it demonstrates pay-per-use storage with micropayments, content sharing capabilities, and seamless AI integration while following decentralized identity management protocols. | [Business](demo-catalog/data-ownership/business-data-ownership.md) | Sep 2025 |
| **File Integrity & Tracking**| Time Anchor | Put checksums on the blockchain to verify integrity of applications, but expanded to include Overlay Services to register the files and keep track with UHRP / BRC-26 and StorageUploader and use WalletClient from @bsv/sdk to make use of BSV Desktop for making transactions | [Business](demo-catalog/time-anchor/business-time-anchor.md) | Sep 2025 |
| **Sustainable Fashion** | Circular Economy    | This proof of concept demonstrates how BSV blockchain can revolutionize circular fashion by creating complete traceability from plastic waste to finished garments. The application tracks every step of the recycling process - from bottle collection in London through transformation into high-fashion clothing. By recording each stage on the immutable blockchain, it provides consumers with verifiable proof of sustainability claims, enabling them to make informed purchasing decisions. This transparency-driven approach could transform the fashion industry's environmental impact while building consumer trust through data-backed circular economy practices. | [Business](demo-catalog/circular-economy/business-circular-economy.md) | Sep 2025 |
| **Token Economy** | Token Transfer | This proof of concept demonstrates token-based file sharing on BSV blockchain. Users earn tokens by uploading files and spend tokens to access others' content. Built with BSV Desktop wallet integration and PushDrop protocol, it showcases practical token minting, transfer mechanisms, and value exchange systems within a decentralized file-sharing platform. | [Business](demo-catalog/token-transfer/business-token-transfer.md)  | Sep 2025 |
| **Payment Solutions** | Pay-QuickR | React TypeScript demonstration of BSV blockchain wallet integration using @bsv/sdk. Showcases wallet authentication, public key retrieval, and QR code generation for developers learning BSV blockchain development. Live demo available with documented limitations for educational purposes. | [Business](demo-catalog/pay-quickr/business-pay-quickr.md) [Technical](demo-catalog/pay-quickr/technical-pay-quickr.md) | Sep 2025 |
| **Debugging Tools** | Beef Inspector | Early-stage web application designed to parse and visualize BEEF (Background Evaluation Extended Format) hex payloads for BSV blockchain transactions. Built using modern web technologies to provide developers with tools to inspect BEEF transaction structures. | [Business](demo-catalog/beef-inspector/business-beef-inspector.md) [Technical](demo-catalog/beef-inspector/technical-beef-inspector.md) | Sep 2025 |
| **Transaction Analysis** | Preimage | Early-stage web application designed to parse and visualize BSV transaction preimages. Aims to help developers inspect preimage components and understand BSV transaction signature mechanisms for debugging sCrypt smart contracts. | [Business](demo-catalog/preimage/business-preimage.md) [Technical](demo-catalog/preimage/technical-preimage.md) | Sep 2025 |



## Quick Filters

### By Industry
- **Healthcare:** P2P Medical, Prescription Tokens
- **Supply Chain:** Natural Chain, Circular Economy
- **Data Management:** Truth Machine, Data Ownership (SOLID + BSV)
- **Identity:** Register, Digital Trust Card
- **Sustainable Fashion:** Circular Economy
- **Content Sharing / Token Economy:** Token Transfer
- **Developer Tools:** Pay-QuickR, Beef Inspector, Preimage

### By Use Case Category
- **Data Integrity:** Truth Machine, Time Anchor
- **Tokenization:** Prescription Tokens, Token Transfer
- **Supply Chain Tracking:** Natural Chain, Circular Economy
- **Healthcare Systems:** P2P Medical, Prescription Tokens
- **Identity Management:** Register, Digital Trust Card
- **Age Verification & Compliance:** Digital Trust Card
- **Decentralized Knowledge Management:** Data Ownership
- **File Integrity & Verification:** Time Anchor
- **Sustainable Product Transparency:** Circular Economy
- **Decentralized File Sharing:** Token Transfer
- **Wallet Integration & Payments:** Pay-QuickR
- **Transaction Debugging:** Beef Inspector, Preimage
- **Developer Education:** Pay-QuickR, Beef Inspector, Preimage

### By Implementation Status
- **Done (Live Demos Available):** Truth Machine, Natural Chain, Prescription Tokens, Digital Receipts, Pay-QuickR, Beef Inspector, Preimage
- **In Progress:** P2P Medical, Register, Slack Threads, Digital Trust Card, Data Ownership, Time Anchor, Token Transfer
- **Not Started:** Circular Economy

### By Development Stage
- **Fully Functional:** Truth Machine, Natural Chain, Prescription Tokens, Digital Receipts, Slack Threads, Pay-QuickR, Beef Inspector, Preimage
- **Core Features Complete:** P2P Medical, Register
- **Pilot Phase:** Digital Trust Card, Data Ownership, Time Anchor, Token Transfer
- **Planning Phase:** Circular Economy


## Demo Access Links

### Truth Machine - Data Integrity & Hashing
- **Live Demo:** [https://truth-machine.bsvb.tech/](https://truth-machine.bsvb.tech/)
- **Repository:** [https://github.com/bsv-blockchain-demos/truth-machine](https://github.com/bsv-blockchain-demos/truth-machine)

### Natural Chain - Supply Chain Natural Gas
- **Live Demo:** [natural-chain.vercel.app](https://natural-chain.vercel.app) *(new bsvb.tech URL coming soon)*
- **Repository:** [https://github.com/bsv-blockchain-demos/natural-chain](https://github.com/bsv-blockchain-demos/natural-chain)


### Prescription Tokens - Tokenization of Medicine
- **Live Demo:** [prescription-tokens.vercel.app](https://prescription-tokens.vercel.app) *(new bsvb.tech URL coming soon)*
- **Repository:** [https://github.com/bsv-blockchain-demos/prescription-tokens](https://github.com/bsv-blockchain-demos/prescription-tokens)

### Pay-QuickR - BSV Wallet Integration Demo
- **Live Demo:** [pay-quickr.vercel.app](https://pay-quickr.vercel.app)
- **Repository:** [https://github.com/bsv-blockchain-demos/Pay-QuickR](https://github.com/bsv-blockchain-demos/Pay-QuickR)

### Beef Inspector - BEEF Payload Debugging Tool
- **Repository:** [https://github.com/bsv-blockchain-demos/beef-inspector](https://github.com/bsv-blockchain-demos/beef-inspector)
- **Status:** Complete demo - ready for use and contribution

### Preimage - Transaction Preimage Analysis Tool
- **Repository:** [https://github.com/bsv-blockchain-demos/preimage](https://github.com/bsv-blockchain-demos/preimage)
- **Status:** Complete demo - ready for use and contribution

## Navigation Structure

Each use case includes:
- **Business Documentation**: Business value, use cases, ROI, and implementation considerations
- **Technical Documentation**: Architecture, APIs, deployment guides, and developer resources

---
