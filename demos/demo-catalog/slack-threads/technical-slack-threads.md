# BSVA Slack Threads - Technical Documentation

**Demo ID**: `demo-2025-007`
**Version**: `[1.0.0]`
**Last Updated**: `2025-09-05`

## Architecture Overview

### System Architecture
The system integrates a Slack bot application within BSVA’s internal Slack workspace and a Next.js-based public Q&A website.
- The Slack bot app detects emoji reactions on threaded messages to archive full threads into MongoDB.
- The public website displays shared threads and enables users to tip contributors with BSV micropayments.
- Blockchain micropayments provide economic incentives for quality participation and knowledge sharing.

### Technology Stack
- **Frontend**: Next.js 13 (React) for the shared Q&A website
- **Backend**: Node.js Express server powering the Slack-bot with MongoDB for thread storage
- **Database**: MongoDB for persistent storage of thread metadata, messages, replies, and reactions
- **Infrastructure**: Hosted on cloud services with Slack API integration and BSV payment network connectivity

### Key Components
1. **Slack Bot App**: Reacts to `:inbox_tray:` emoji to save and update Slack threads in MongoDB
2. **Slack API Handler**: Listens for real-time message updates, edits, deletions, and emoji reactions
3. **Public Website**: Displays threads, manages BSV micropayment tipping, and interfaces with blockchain services

## Integration & APIs

### External Dependencies
| Service       | Purpose                          | Version | Documentation                                   |
|---------------|---------------------------------|---------|------------------------------------------------|
| Slack API     | Interaction with Slack workspace| Latest  | [Slack API Docs](https://api.slack.com/)       |
| MongoDB       | Thread storage and indexing     | Latest  | [MongoDB Docs](https://www.mongodb.com/docs/)  |
| BSV Network   | Micropayments and blockchain    | Latest  | [BSV Explorer & SDK Docs](https://bsv.dev/)    |

### API Endpoints
```bash
GET  /api/v1/threads
POST /api/v1/tips
GET  /api/v1/threads/:threadId
```

## Implementation Guide

### Prerequisites
- Node.js (v18 or later), npm/yarn
- MongoDB instance (cloud or local)
- Slack workspace admin access for bot installation and permissions
- BSV blockchain node or provider for micropayments

### Setup Instructions

Clone repositories:

Slack Threads Webapp:
```bash
git clone https://github.com/bsv-blockchain-demos/slack-threads-webapp
cd slack-threads-webapp
npm install
npm run dev
```

Slack Bot App:
```bash
git clone https://github.com/bsv-blockchain-demos/slack-bot-app
cd slack-bot-app
npm install
node app.js
```

### Configuration

Key configuration parameters:

- `SLACK_BOT_TOKEN`: Slack bot OAuth token
- `MONGO_URI`: MongoDB connection string
- `BSV_NODE_URL`: BSV blockchain node endpoint

## Testing & Validation

### Test Coverage
- **Unit Tests**: Core Slack bot event handlers and database operations covered
- **Integration Tests**: Verified end-to-end thread saving, updating, and retrieval
- **Performance Tests**: Thread saving latency < 500ms; tipping transaction time < 2 sec

### Validation Criteria
- [ ] Slack bot successfully archives threads on `:inbox_tray:` reaction
- [ ] Public site displays archived threads with up-to-date content
- [ ] BSV micropayment tips processed and confirmed on blockchain

## Performance & Scalability

### Current Metrics
- **Response Time**: API average 300ms
- **Throughput**: 50 thread saves per minute in pilot
- **Resource Usage**: Moderate CPU and memory usage on Node.js server

### Scalability Considerations
- Potential for horizontal scaling of bot app instances
- Load balancing of API backend
- Batch processing of blockchain transactions to reduce fees and latency

## Maintenance & Support

### Monitoring
- **Logs**: Centralized logging of bot events and API requests
- **Metrics**: Dashboard with thread counts, tip volumes, API performance
- **Alerts**: Notifications for failures in thread archiving or transaction failures

### Troubleshooting
| Issue               | Cause                        | Solution                         |
|---------------------|------------------------------|---------------------------------|
| Slack API rate limit | Excessive requests           | Implement exponential backoff   |
| Payment failure     | Blockchain node unavailable  | Retry transactions or fallback  |

## Resources

- **Repository**: [Slack Threads Webapp](https://github.com/bsv-blockchain-demos/slack-threads-webapp), [Slack Bot App](https://github.com/bsv-blockchain-demos/slack-bot-app)
- **Business Documentation**: [BSVA Slack Threads - Business Overview](./business-slack-threads.md)
- **Demo Environment**: [Live Demo](http://slack-threads-us-01.bsvb.tech/)
- **Support Contact**: [Lead Blockchain Engineer]

---
*For business context and ROI details, see: [BSVA Slack Threads - Business Overview](./business-slack-threads.md)*

