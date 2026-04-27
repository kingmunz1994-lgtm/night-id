<div align="center">

# Night ID — ZK Identity on Midnight

</div>

> *Prove what you built. Share nothing else.*

---

🌑 **This project is built on the Midnight Network.**
🔗 **This project integrates with the Midnight Network.**
🛠 **This project extends the Midnight Network with on-chain identity primitives.**

[![Built On Midnight](https://img.shields.io/badge/⬛_BUILT_ON-MIDNIGHT_NETWORK-7c3aed?style=for-the-badge&labelColor=090714)](https://midnight.network)
[![ZK Proofs](https://img.shields.io/badge/🔒_ZK_PROOFS-ENABLED-00d68f?style=for-the-badge&labelColor=090714)](https://midnight.network/developers)
[![NIGHT Token](https://img.shields.io/badge/🌙_$NIGHT-POWERED-b97dff?style=for-the-badge&labelColor=090714)](#night-score--zk-builder-credential)
[![Live Demo](https://img.shields.io/badge/🌐_LIVE-DEMO-38bdf8?style=for-the-badge&labelColor=090714)](https://kingmunz1994-lgtm.github.io/night-id)
[![License MIT](https://img.shields.io/badge/LICENSE-MIT-475569?style=for-the-badge&labelColor=090714)](./LICENSE)

---

## What is Night ID?

Night ID is a zero-knowledge identity layer for the Midnight Network. Register a `.night` name tied to your wallet, then issue and share verifiable credentials about your on-chain activity — without exposing your address, balance, or any personal data.

Every credential is backed by real on-chain data queried live from the Midnight Preprod indexer. You prove. The network confirms. Nothing else leaves your device.

**[→ Live Demo](https://kingmunz1994-lgtm.github.io/night-id)**

---

## Midnight Network Integration

Night ID is Midnight-native from wallet connection to credential issuance.

**Built on Midnight** — Identity anchoring, DID generation, and ZK credential signing all operate on the Midnight Network. The Night Markets Escrow contract (`7473b82b398f6b8665541862a1165c6c5da379355f9c32dace36ed234b7cc711`) on Midnight Preprod is the on-chain source of truth for builder credentials.

**Integrates with Midnight** — Wallet detection uses the Midnight DApp Connector API. Lace wallet (`window.midnight.mnLace`) and 1AM wallet (`midnight#ready` UUID injection) are both supported natively. Any future wallet implementing the spec connects automatically.

**Extends Midnight** — Night ID ships the Brick Towers `identity-api` pattern to the ecosystem: HTTP credential issuance, W3C DID anchoring (`did:midnight:preprod:`), and ZK-signed builder scores that travel with a user across every Midnight application.

---

## Features

**🪪 .night Name Registration** — Claim a `.night` name on the Midnight Network, linked to your wallet address. Your name is your on-chain identity — portable, private, yours.

**🏗️ Night Score — ZK Builder Credential** — Night Score is your proof of work on Midnight. It is calculated from real on-chain activity queried directly from the Midnight Preprod GraphQL indexer: contracts deployed, ZK circuit calls made, and escrow flows completed. The score is deterministic and cannot be purchased or gamed.

| Score Range | Level |
|---|---|
| 0–99 | ⬜ Contributor |
| 100–299 | 🟣 Builder |
| 300–599 | 🔵 Maker |
| 600–999 | 🟢 Founder |
| 1000+ | 🌟 Architect |

**🔒 ZK Credentials** — Prove statements about yourself without revealing the underlying data. Example credentials include proving you have built on Midnight, completed a full escrow flow (`createListing → fundEscrow → releaseEscrow`), or earned above a threshold of `tNIGHT` — all without disclosing your balance or address.

**🔑 Real Wallet Detection** — The DApp Connector API is polled on page load and on `midnight#ready` events, detecting:
- **Lace** — via `window.midnight.mnLace` (static key)
- **1AM** — via UUID injection on `midnight#ready`
- Any wallet implementing the Midnight DApp Connector spec

When a wallet is found, the address is auto-filled and verification runs immediately. Manual address entry is also supported for wallets not yet injected.

**⛓️ Live Contract Activity Feed** — The bottom of the page shows a live feed of `ContractDeploy`, `ContractCall`, and `ContractUpdate` events for the Night Markets contract, polled from the real Midnight Preprod indexer every 30 seconds. No mock data, no simulation.

**🌐 Share to X / Discord** — Export your credential as a formatted block and post directly to X (with pre-filled tweet text) or copy it as a Discord code block for pasting into any server.

**📡 Fully Client-Side** — Night ID has no backend. All indexer queries run directly from the browser to the Midnight Preprod GraphQL endpoint (`https://indexer.preprod.midnight.network/api/v3/graphql`). Nothing is proxied, logged, or stored.

---

## Privacy Model

Night ID is built on one rule: the credential proves, it does not reveal.

When you verify your address, the indexer returns on-chain facts — transaction counts, contract interactions, timestamps. Night ID converts those facts into a score and a W3C DID. Your raw address is never included in the credential. Your balance is never queried. No cookies, no analytics, no server logs.

The ZK proof is the credential. The credential is the identity.

---

## Credential Issuance

Night ID implements the **Brick Towers `identity-api`** credential issuance pattern.

| Field | Value |
|---|---|
| **Issuer** | Night Markets Protocol |
| **Standard** | W3C DID · Midnight ZK |
| **DID format** | `did:midnight:preprod:<suffix>` |
| **Signing key** | EC P-256 · ZK verified |
| **Privacy** | Zero personal data |

Supported credential types:

- **Age proof** (Brick Towers) — prove you meet an age threshold without revealing your date of birth
- **Midnames DID anchoring** — your `.night` name is anchored to your DID on-chain
- **KYC Midnight epoch v1** — the first epoch of the Midnight identity standard

---

## On-Chain Data

Night ID reads from the following on Midnight Preprod:

| Source | Details |
|---|---|
| **Night Markets Escrow** | `7473b82b398f6b8665541862a1165c6c5da379355f9c32dace36ed234b7cc711` |
| **GraphQL indexer** | `https://indexer.preprod.midnight.network/api/v3/graphql` |
| **Network** | Midnight Preprod |
| **Deploy block** | 127,350 |

Queries used:
- `unshieldedTransactions(address: $addr)` — transaction count and first-activity date
- `contractActions(contractAddress: $addr)` — deploys, circuit calls, contract updates

---

## Score Breakdown

| Action | Points |
|---|---|
| Night Markets contract deployed | +200 |
| Any smart contract deployed | +100 per contract |
| ZK circuit call on-chain | +15 per transaction |
| Full escrow flow completed | +75 bonus |

NIGHT token rewards are derived from the score at a rate of 1 NIGHT per 10 points and displayed on the issued credential.

---

## Getting Started

```bash
# Clone the repo
git clone https://github.com/kingmunz1994-lgtm/night-id.git
cd night-id

# Run locally — no build step required
npm run dev
# → http://localhost:3003
```

Or open `public/index.html` directly in a browser — it works without a server.

**To connect a wallet:**

1. Install [Lace](https://midnight.network/lace), [1AM](https://1am.xyz), or any Midnight DApp Connector wallet
2. Open Night ID — the wallet is detected automatically
3. Approve the connection request in your wallet
4. Your address is auto-filled and your Night Score loads immediately

**To verify manually:**

1. Paste any `mn_addr_preprod1...` address into the input field
2. Press Enter or click **Verify on Midnight Preprod**
3. Your credential is issued from live indexer data

---

## Deployment

Night ID is deployed as a static site via GitHub Pages — no server, no backend, no infrastructure to manage.

```bash
# Deploy: push public/ to gh-pages branch
# Or configure GitHub Pages to serve from /public on main
```

The live demo is served at: `https://kingmunz1994-lgtm.github.io/night-id`

---

## Part of the Night Ecosystem

Night ID is one component of the Night protocol suite:

| Project | Description |
|---|---|
| [Night Markets](https://kingmunz1994-lgtm.github.io/night-markets) | ZK-private global marketplace on Midnight |
| **Night ID** | ZK identity and builder credentials |
| [Night Fun](https://kingmunz1994-lgtm.github.io/night-fun) | Social layer on Midnight |

---

## License

MIT © Night ID Contributors — *Built on the Midnight Network.*

---

<div align="center">

*"Your proof of work. Nothing more, nothing less."*

[🌐 Live Demo](https://kingmunz1994-lgtm.github.io/night-id) · [🌑 Midnight Network](https://midnight.network) · [⛓️ Midnight Explorer](https://explorer.preprod.midnight.network/contracts/7473b82b398f6b8665541862a1165c6c5da379355f9c32dace36ed234b7cc711)

</div>
