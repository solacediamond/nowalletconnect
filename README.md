# NoWalletConnect

Accept USDT payments without a wallet-connect popup.

The customer picks an amount and network, scans a QR or copies an address, and sends USDT. NoWalletConnect watches the chain and marks the payment received.

## How it works

```
Merchant share link
        ↓
Customer opens checkout.html?merchant=ID
        ↓
Customer enters amount + Polygon or Solana
        ↓
Payment page shows receive address + QR
        ↓
Customer sends exact USDT amount
        ↓
Verifier checks the chain
        ↓
Success screen + explorer link
```

1. Each merchant has an ID, email, and payout wallets stored in the database.
2. The public checkout link only contains the merchant ID.
3. The customer never connects a wallet to the site.
4. Funds go directly to the merchant’s Polygon or Solana address.

## Networks

| Network | Token |
|---|---|
| Polygon | USDT (ERC-20) |
| Solana | USDT (SPL) |

Send USDT on the selected network only. The wrong network can mean a lost payment.

## Pages

| File | Role |
|---|---|
| `checkout.html` | Customer enters amount and network. Requires `?merchant=YOUR_ID`. |
| Payment page (`index` / checkout target) | QR, receive address, live wait, success. |
| `merchant.html` | Merchant dashboard. Sign in with **merchant ID + email**. |
| Integration guide | Copy-paste Pay Now button for a website. |

## Merchant checkout link

```
https://solacediamond.github.io/nowalletconnect/checkout.html?merchant=YOUR_ID
```

Replace `YOUR_ID` with the ID in the database (example: `NWC`).

Customers then choose:

- amount in USDT
- Polygon or Solana

That continues to the payment page with:

```
?merchant=YOUR_ID&amount=50&network=polygon
```

## Embed on a website

```html
<div style="display:inline-block;text-align:center;">
  <a href="https://solacediamond.github.io/nowalletconnect/checkout.html?merchant=MERCHANT_ID" style="text-decoration:none;">
    <button style="padding:12px 24px;background:linear-gradient(135deg,#00d4ff,#7c3aed);color:#0f1419;border:none;border-radius:8px;font-weight:700;cursor:pointer;font-size:16px;">
      Pay Now
    </button>
  </a>
  <div style="margin-top:8px;font-size:11px;font-family:sans-serif;">
    <a href="https://solacediamond.github.io/nowalletconnect/" style="color:#94a3b8;text-decoration:none;">Powered by NoWalletConnect</a>
  </div>
</div>
```

Change the button text, colors, and size if you want. Keep the checkout `href` and replace `MERCHANT_ID`. Keep the Powered by line.

## Merchant dashboard

Sign in with:

- merchant ID
- email on file

No password. Both values must match the same database row.

The dashboard shows:

- checkout link
- wallets on file
- volume, payment count, average, largest payment
- Polygon vs Solana split
- last 7 days
- searchable history and CSV export

## Database fields

**Merchants**

| Field | Used for |
|---|---|
| Merchant ID | Public checkout handle and login |
| Business name | Dashboard label |
| Polygon wallet | Receive address |
| Solana wallet | Receive address |
| Email | Login (must match ID) |

**Payments**

| Field | Used for |
|---|---|
| Date | History and 7-day trend |
| Merchant ID | Filter dashboard rows |
| Amount | Analytics |
| Transaction hash | Explorer link |
| Network | Polygon / Solana split |

## Customer payment flow

1. Open the merchant checkout link.
2. Enter the exact USDT amount.
3. Choose Polygon or Solana.
4. Send **only USDT** on that network to the shown address.
5. Wait for confirmation. Use **Check again** if needed.
6. Payment window expires after 15 minutes if nothing arrives.

## Pricing

- $1 one-time setup
- $1 per month
- No per-transaction fee from NoWalletConnect
- Merchant keeps 100% of received USDT (network gas is paid by the sender)

## What this is not

- Not MetaMask / Phantom WalletConnect
- Not a custodian — payouts go to the merchant wallet
- Not a place to send the wrong token or the wrong chain

## Support

- Telegram: [t.me/noWalletConnect](https://t.me/noWalletConnect)
- Docs: [NoWalletConnect-Docs.pdf](https://github.com/solacediamond/nowalletconnect/blob/main/NoWalletConnect-Docs.pdf)

## License

Use with a valid merchant ID issued by NoWalletConnect.
