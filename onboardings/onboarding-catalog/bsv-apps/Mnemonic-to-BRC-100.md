### Mnemonic-to-BRC-100.md

***
**Status:** Production
**Last Updated:** October 2025
**Contact/Support:** GitHub Project

***
# Mnemonic to BRC-100 Onboarding Guide

A utility for deriving BRC-100 wallet addresses from mnemonic phrases.

## Key Features
- Supports Centbee, Rock Wallet, Electrum SV
- Adjustable gap/offset for unused addresses

## Getting Started

1. Go to [https://mnemonic-brc100.vercel.app/](https://mnemonic-brc100.vercel.app/)
2. Enter mnemonic, PIN (optional), and derivation path
3. Select wallet type

| WALLET        | DERIVATION PATH     |
|---------------|--------------------|
| Centbee       | `m/44'/0'/0`       |
| Rock Wallet   | `m/0'/0`           |
| Electrum SV   | `m/44'/236'/0'`    |
| Common        | `m/44'/0'/0'`      |

4. Generate addresses and manage coins

## Screenshots
- Main form: (SKIP_SCREENSHOT_TAG)
- Derivation result: (SKIP_SCREENSHOT_TAG)

## Tips
- Refresh after spending to avoid accidental double spend
- Run code locally for best security

## Troubleshooting
- Always verify results on-chain

## Next Steps
- Apply derived addresses with other wallets/apps

***
