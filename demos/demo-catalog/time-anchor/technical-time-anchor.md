# Time Anchor - Technical Documentation

**Demo ID**: `demo-2025-10`
**Version**: `0.1.0`
**Last Updated**: `May 2026`

---

## Overview

Time Anchor (a.k.a. Desktop File Integrity) is a cross-platform desktop application that anchors file integrity proofs on the BSV Blockchain. The SHA-256 hash of a file is recorded on-chain inside an `OP_FALSE OP_RETURN` output, and the file itself can optionally be encrypted client-side and pushed to UHRP storage for later recall. Once anchored, any holder of the file can independently verify that it existed in its exact form at the time of the transaction.

---

## Features

- **On-chain hash anchoring** — SHA-256 of file bytes committed via `OP_FALSE OP_RETURN`
- **Client-side encryption** — files encrypted using a wallet-derived key before leaving the machine
- **UHRP / BRC-26 storage** — encrypted bytes published through `StorageUploader` with a 30-day default retention window
- **Overlay broadcast** — transactions broadcast to the `tm_desktopintegrity` topic and looked up via the `ls_desktopintegrity` service
- **File recall** — decrypt and download any previously anchored file via local log or UHRP URL
- **Independent verification** — drop any file into the Verify view to confirm whether its hash exists on the overlay
- **Local audit logs** — every anchoring event writes a human-readable log file (filename, time, txID, UHRP URL, original timestamps, size)
- **Settings drawer** — runtime override of Overlay and UHRP endpoints

---

## System Architecture

### Desktop Shell (Electron 28)

- `electron/main.ts` — main process, window management, IPC handlers for file dialog, file read (≤ 25 MB), file stats, log read/write, and recalled-file save
- `electron/preload.ts` — bridges a typed `window.electronAPI` to the renderer with `contextIsolation: true`

### Renderer (React 19 + TypeScript + Vite)

- React Router routes: `/` (Homepage), `/recall`, `/verify`, `/logs`, `/help`
- Context providers:
  - `walletContext` — manages `WalletClient`, identity key, derived key under `protocolID [0, 'fileintegrity']`, and a per-session random `keyID` persisted in `localStorage`
  - `fileContext` — orchestrates file selection, transaction creation, UHRP upload, and log writing

### BSV Integration (@bsv/sdk ^1.6.11)

- `hooks/FileHash.ts` — builds the `OP_FALSE OP_RETURN <sha256>` locking script
- `hooks/transactions.ts` — `createTransaction()` calls `wallet.createAction()` with `acceptDelayedBroadcast: false`, then broadcasts the BEEF to the overlay via `TopicBroadcaster`; `getTransactionByFileHash()` queries `ls_desktopintegrity`
- `utils/UHRPManager.ts` — `uploadToUHRP()` publishes encrypted bytes via `StorageUploader`; `downloadFromUHRP()` resolves UHRP hosts through the overlay and fetches with SHA-256 verification against the URL's embedded hash

### Wallet

The renderer instantiates `new WalletClient('auto', 'localhost:3000')` and expects a running BSV wallet at that endpoint — either [BSV Desktop](https://github.com/bsv-blockchain/bsv-desktop) or [MetaNet Client](https://projectbabbage.com). All signing, key derivation, encryption, and broadcast happens through the wallet — the app never touches private keys.

---

## Service Endpoints

Defaults are defined in `config/serviceConfig.ts` and overridable through the Settings drawer (persisted in `localStorage`):

| Service | Default | Purpose |
|---|---|---|
| Overlay | `https://overlay-us-1.bsvb.tech` | SHIP/SLAP host for `tm_desktopintegrity` and `ls_desktopintegrity` |
| UHRP | `https://go-uhrp-us-1.bsvblockchain.tech` | Storage uploader endpoint |

---

## Transaction Format

Each anchored file produces a single transaction with one output:

```
satoshis:       1
lockingScript:  OP_FALSE OP_RETURN <sha256(fileBytes)>   // FileHash.lock()
description:    "File Integrity: <fileName>"
```

The transaction's BEEF is broadcast to the `tm_desktopintegrity` topic. Lookup queries against `ls_desktopintegrity` use `{ fileHash: <hex sha256> }` and return matching outputs as a BEEF list with `outputIndex`.

---

## Quick Start Guide

### Prerequisites

- Node.js 18 or higher
- npm
- A running BSV wallet on `localhost:3000` ([BSV Desktop](https://github.com/bsv-blockchain/bsv-desktop) or [MetaNet Client](https://projectbabbage.com))

### Setup

```bash
git clone https://github.com/bsv-blockchain-demos/desktop-integrity-app.git
cd desktop-integrity-app
npm install
```

### Run (production-style local launch)

```bash
npm run start
```

This compiles the Electron main/preload, builds the Vite renderer, runs `electron-builder --dir`, and launches the packaged binary for the current OS.

### Development (two terminals)

```bash
# Terminal 1 — Vite dev server
npm run dev

# Terminal 2 — Electron pointed at the Vite dev server
npm run electron
```

### Distributable Installers

```bash
npm run build:win     # NSIS installer + portable exe
npm run build:mac     # DMG
npm run build:linux   # AppImage + deb
npm run build:all     # All three at once
```

Output written to `release/`.

---

## Usage Guide

### Anchor a File

1. Open the app — the wallet is initialized automatically on startup
2. Select a file via the file picker or drag-and-drop (≤ 25 MB)
3. Optionally toggle the **UHRP** switch to also publish the encrypted file to storage
4. Click **Save to blockchain**
5. The renderer:
   - Reads bytes via the Electron IPC bridge
   - Calls `createTransaction()` → `wallet.createAction()` → broadcasts to the overlay
   - If UHRP is enabled, encrypts the bytes with the wallet's derived key and publishes via `StorageUploader`
   - Maps the resulting `uhrpURL → keyID` in the wallet's `LocalKVStore`
   - Writes a local log file with txID, UHRP URL, original timestamps, and size

### Recall a File

1. Navigate to **Recall**
2. Choose **From Logs** (pick a previously saved log entry) or **From UHRP URL** (paste directly)
3. The app resolves UHRP hosts via the overlay, downloads the encrypted bytes, verifies the content hash against the UHRP URL, decrypts using the wallet, and offers a save dialog

### Verify a File

1. Navigate to **Verify**
2. Select or drag-drop the file to verify
3. The app computes SHA-256 and queries `ls_desktopintegrity` — a non-empty `outputs` list confirms the file's hash exists on-chain

### Browse Logs

Navigate to **Logs** to view every locally recorded anchoring event with txID, UHRP URL, keyID, and original file metadata.

---

## Security Model

- **No private keys in the app** — all signing, encryption, and key derivation happens inside the connected BSV wallet
- **Per-session derived key** — derived via `protocolID: [0, 'fileintegrity']` with a random 8-byte `keyID` generated on first run and persisted in `localStorage`
- **Client-side encryption before storage** — file bytes are encrypted via `wallet.encrypt()` prior to UHRP upload; the storage host only sees ciphertext
- **Content-addressed integrity** — UHRP URLs embed the SHA-256 of the (encrypted) payload; `downloadFromUHRP()` rejects any host whose response does not match
- **Tamper-evident anchoring** — verifying any file requires only its bytes plus access to the overlay; no privileged keys or APIs

---

## File Size Limits

The Electron main process enforces a 25 MB cap on file reads (`MAX_FILE_BYTES` in `electron/main.ts`). Larger files should be hashed and anchored out-of-band.

---

## Project Structure

```
desktop-integrity-app/
├── electron/             # Main process + preload (TypeScript)
├── src/
│   ├── components/       # homepage, recall, verify, logs, sidebar, topbar, settingsDrawer, help
│   ├── css/
│   └── App.tsx           # React Router shell
├── context/              # walletContext, fileContext
├── hooks/                # transactions.ts, FileHash.ts, clearStorage.ts
├── utils/                # UHRPManager.ts, queueHandler.ts, clipboard.ts
├── config/               # serviceConfig.ts (overlay + UHRP endpoints)
├── types/                # Shared TS types + electron.d.ts
└── scripts/              # Build helpers
```

---

## Troubleshooting

| Issue | Cause | Solution |
|---|---|---|
| "Failed to connect wallet" toast on startup | No wallet running on `localhost:3000` | Launch BSV Desktop or MetaNet Client and unlock it |
| "No one currently hosts this file" on recall | UHRP retention (30 days) expired or host pruned | The on-chain hash anchor still proves existence; the file payload is unrecoverable |
| Verify returns empty outputs | File never anchored, or different overlay endpoint | Confirm overlay URL in Settings, re-check the file |
| Transaction creation fails | Wallet has insufficient sats or rejected the action | Top up the wallet and retry |
| File exceeds 25 MB limit | Renderer cap | Hash the file externally and anchor the hash, or raise `MAX_FILE_BYTES` |

---

## Resources

- **Repository**: https://github.com/bsv-blockchain-demos/desktop-integrity-app
- **Business Documentation**: [business-time-anchor.md](business-time-anchor.md)
- **BSV TypeScript SDK**: https://github.com/bsv-blockchain/ts-sdk
- **UHRP / BRC-26**: https://bsv.brc.dev/transactions/0026
- **BSV Desktop**: https://github.com/bsv-blockchain/bsv-desktop

---

*For business context, target users, and ROI, see: [Business Documentation](business-time-anchor.md)*
