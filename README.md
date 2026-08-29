# NoWalletConnect

A simple **USDT receive screen**. You open it. The sender pays from their own wallet. No WalletConnect popup, no browser extension on your side.

**Live:** [https://solacediamond.github.io/nowalletconnect/](https://solacediamond.github.io/nowalletconnect/)

---

## What it does

1. You save your Polygon and/or Solana receive address in Settings.
2. You pick the network and type the exact USDT amount.
3. You show the QR (raw address) or copy the address.
4. The sender sends **USDT on that same network**, exact amount.
5. The page asks a Cloudflare Worker every 5 seconds. When a matching incoming transfer is found, you get **Payment received** and an explorer link.

The sender never uses this website.

## Networks

| Network | Token | Notes |
| --- | --- | --- |
| Polygon | USDT (ERC-20) | Cheap, usually seconds to ~30s |
| Solana | USDT (SPL) | Cheap, usually seconds |

Wrong network or native coin (POL / SOL) is not detected and can be lost.

## How verification works

The static page calls:

`https://nowalletconnect-api.lightunuovo.workers.dev/verify`

A payment is treated as paid only if:

- it went to the address in Settings
- it is USDT on the selected network
- the amount matches within **0.01 USDT**
- it is inside the look-back window (about **30 minutes**)

Checkout watches for **15 minutes**. **Check again** starts a new window. API keys live in Cloudflare Secrets Store, not in this repo.

## Quick start (receiver)

1. Open the live URL above (not a random `file://` copy, unless you allowed that origin on the Worker).
2. Settings (bottom-right) → paste addresses → Save.
3. Choose Polygon or Solana → Continue.
4. Type the amount they should send.
5. Show QR / copy address. Stay on that screen.

Test with a tiny amount to yourself on each network before taking real payments.

## What to tell the sender

> Scan or copy → **USDT only** → same network as the screen → **exact amount**.

If their wallet cannot scan the QR, they paste the address and pick the token themselves.

## Limits (read this)

- This is a live detector, not a cash register. No invoices, refunds, or payment history.
- The QR is the address only. It does not lock USDT or the amount.
- Two people sending the **same amount** to the same wallet in the same window can look like one payment. Use a different amount for the next sale.
- Addresses are stored in **this browser** (`localStorage`). Another device or a cleared cache means you paste them again.
- The green screen is not legal proof by itself. Open the explorer link.

## Repo layout

Typical Pages site:

```text
index.html          # the app
polygon.png
solana.png
nwc.jpg             # favicon
README.md
```

The Worker is a separate Cloudflare Worker (`nowalletconnect-api`), not this static folder.

## Worker

`GET /verify?network=polygon|solana&address=...&amount=...&since=...`

Returns JSON:

```json
{ "paid": true, "tx": "0x...", "explorer": "https://..." }
```

or `{ "paid": false }`.

Secrets Store bindings:

- `ETHERSCAN` → Etherscan API key (Polygon via chainid 137)
- `HELIUS` → Helius API key (Solana)

CORS should allow at least:

- `https://solacediamond.github.io`
- optional local origins such as `http://127.0.0.1:5500` for Live Server

## Local preview

Serve `index.html` over `http://localhost` or `http://127.0.0.1` (not as a raw file) and keep that origin on the Worker allow-list. GitHub Pages is the supported setup.

## License

Use and modify for your own receive flow. You are responsible for telling senders the correct network and token.
