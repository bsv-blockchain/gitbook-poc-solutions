# MarsCast Onboarding Guide

**Status:** Production
**Last Updated:** October 2025
**Contact/Support:** [Babbage Systems](https://marscast.babbage.systems/)

---

## 1. What Is MarsCast?

MarsCast is a blockchain-secured API service that delivers real-time Mars weather data and planetary information via pay-per-request micropayments on the BSV blockchain. It demonstrates how API access can be monetized with BSV micropayments while requiring identity verification through CoolCert certificates.

**Key Features:**
- **Mars Weather Data** - Access latest Mars planetary and atmospheric data
- **Pay-Per-Request Model** - Micropayment pricing at $0.001 per API request
- **CoolCert Integration** - Identity verification required for API access
- **Real-Time Data** - Current Mars weather and planetary conditions
- **Blockchain Payments** - Transparent, instant BSV micropayment transactions
- **API Demonstration** - Showcases monetized API access patterns

**Target Users:** Developers learning API monetization with micropayments, educators demonstrating blockchain applications, space enthusiasts accessing Mars data, and anyone interested in pay-per-use API models.

---

## 2. Before You Begin

### Prerequisites
- **BSV Wallet:** BRC-100 compatible wallet (Metanet Desktop recommended)
- **CoolCert Identity:** Valid CoolCert identity certificate (required for authentication)
- **Web Browser:** Modern browser with JavaScript enabled
- **BSV Balance:** Small amount of BSV for API requests ($0.001 per request)

### Supported Platforms
- **Web-based:** Access via https://marscast.babbage.systems/
- **Cross-platform:** Works on desktop and mobile browsers
- **No Installation Required:** Pure web application
- **API Access:** RESTful API for programmatic use

### Setting Up Prerequisites
1. **Get a BSV Wallet:** Follow the [BSV Desktop Onboarding Guide](../metanet-desktop-mainnet.md)
2. **Obtain CoolCert:** Visit [CoolCert](./CoolCert.md) to issue your identity certificate
3. **Fund Wallet:** Ensure you have BSV for API micropayments

---

## 3. Getting Started: Step-by-Step

### Step 1: Access MarsCast
1. Navigate to **https://marscast.babbage.systems/** in your web browser
2. Ensure your BSV Desktop wallet is running in the background
3. The main page displays the Mars weather dashboard

![MarsCast weather dashboard interface](../../../assets/onboardings/bsv-apps/marscast-app/weather-dashboard.png)

### Step 2: Authenticate with CoolCert
1. MarsCast requires a valid CoolCert identity certificate
2. If you don't have one:
   - Visit [https://coolcert.babbage.systems/](https://coolcert.babbage.systems/)
   - Issue your CoolCert certificate following the [CoolCert guide](./CoolCert.md)
   - Return to MarsCast with your certificate
3. MarsCast will automatically detect your CoolCert identity
4. Grant permission for MarsCast to verify your certificate

![Certificate verification and authentication flow](../../../assets/onboardings/bsv-apps/marscast-app/auth-flow.png)

### Step 3: Request Mars Weather Data
1. Once authenticated, click **"Fetch Mars Weather"**
2. Review the request cost: **$0.001 (approximately 1,000-2,000 satoshis)**
3. Confirm the micropayment in your BSV wallet
4. API processes your request immediately

### Step 4: View Mars Data
After payment confirmation, you'll receive:
- **Temperature:** Current Mars surface temperature
- **Atmospheric Pressure:** Mars atmospheric conditions
- **Wind Speed:** Mars wind data
- **Sol (Mars Day):** Current Martian day number
- **Season:** Current Martian season
- **Additional Metrics:** Various planetary data points

### Step 5: Review Transaction History
1. View your API request history in MarsCast dashboard
2. Check payment confirmations in BSV Desktop wallet
3. Each request is recorded on the blockchain for transparency
4. Monitor your spending on API calls

---

## 4. Advanced Features

### API Integration for Developers
MarsCast provides RESTful API access:
- **Programmatic Requests:** Integrate Mars data into your applications
- **Identity Authentication:** Use CoolCert for automated identity verification
- **Payment Automation:** Implement BSV micropayment workflows
- **Data Parsing:** Process JSON responses for Mars weather data

### Educational Applications
- **Demonstrate Micropayments:** Show how APIs can be monetized per-request
- **Teach Blockchain Identity:** Explain CoolCert authentication integration
- **Space Education:** Use real Mars data for astronomy education
- **API Monetization:** Study pay-per-use business models

### Pay-Per-Request Benefits
- **No Subscriptions:** Pay only for what you use
- **Transparent Pricing:** Fixed $0.001 per request
- **Instant Access:** No signup or monthly fees
- **Blockchain Proof:** Every request recorded immutably
- **Micro-Scale Economics:** Enable fractional cent transactions

### Integration with Other Apps
- Combine Mars data with analytics dashboards
- Build educational applications using MarsCast API
- Integrate with IoT devices for space-themed projects
- Create student projects showcasing blockchain APIs

---

## 5. Troubleshooting & FAQs

### Common Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| Missing certificate error | No CoolCert identity | Visit CoolCert and issue your identity certificate |
| API request fails | Insufficient BSV or connectivity | Check wallet balance; verify internet connection |
| Authentication fails | CoolCert verification issue | Reissue CoolCert certificate; ensure wallet is connected |
| Micropayment status unknown | Blockchain confirmation pending | Wait a few seconds for transaction confirmation |
| Data not loading | Network or API issue | Refresh page; check API status; retry request |

### Important Tips
- **Keep CoolCert Active:** Ensure your certificate is valid before requesting data
- **Monitor BSV Balance:** Each request costs ~$0.001; plan accordingly
- **Request Responsibly:** API calls cost money; avoid unnecessary requests
- **Save Data:** Copy or export Mars weather data if needed for later reference
- **Use for Learning:** MarsCast is an excellent demonstration of blockchain API patterns

### Getting Help
- **Website:** [https://marscast.babbage.systems/](https://marscast.babbage.systems/)
- **CoolCert Issues:** Visit [CoolCert guide](./CoolCert.md) for identity troubleshooting
- **BSV Community:** Forums for technical support
- **Babbage Systems:** Documentation and developer resources

---

## 6. Learn More / Next Steps

### For Students and Educators
- Use MarsCast to demonstrate blockchain-based API monetization
- Teach concepts of micropayments and identity verification
- Integrate Mars data into astronomy or computer science curricula
- Build projects combining space data with blockchain technology

### For Developers
- **Integrate MarsCast API:** Use Mars data in analytics or educational dashboards
- **Study Implementation:** Learn how identity and payments integrate
- **Build Similar APIs:** Use MarsCast as template for your own pay-per-use services
- **Demo API Monetization:** Show clients how micropayments enable new business models

### Use Cases
- **Educational Dashboards:** Display real-time Mars data for classrooms
- **Space-Themed Apps:** Integrate Mars weather into astronomy applications
- **API Development Learning:** Study pay-per-request implementation patterns
- **Micropayment Demonstration:** Showcase fractional payment capabilities to stakeholders

### Related Apps and Resources
- **Prerequisites:** [CoolCert](./CoolCert.md) for identity, [BSV Desktop](../metanet-desktop-mainnet.md) for wallet
- **Similar Concepts:** [ToolBSV](./Tool.md) for AI API micropayments
- **BSV Ecosystem:** Explore more apps at [Metanet Apps](https://metanetapps.com/)

### Explore Further
- Learn about Mars from NASA and space agencies
- Study blockchain API monetization patterns
- Explore identity-gated application architectures
- Build your own micropayment-enabled APIs

---

**Quick Links:**
[CoolCert Guide](./CoolCert.md) | [BSV Desktop Guide](../metanet-desktop-mainnet.md) | [BSV Getting Started](../README.md)

---

*MarsCast - Real-time Mars weather data via blockchain-secured, pay-per-request API with BSV micropayments.*
