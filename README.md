# NoWalletConnect

A simple, standalone crypto payment receiver app. No wallet extensions required.

## Features

- 💰 Receive USDT on Polygon or Solana
- 📱 Works on desktop and mobile
- 🎯 QR code generation for easy payments
- ⚡ Automatic blockchain verification
- 🔗 View transactions on explorer
- 🎨 Beautiful, modern interface

## How It Works

1. **Configure** - Set your receiver wallet address in settings
2. **Select Network** - Choose Polygon or Solana
3. **Generate QR** - Enter amount, get a QR code
4. **Share** - Send QR code to payer
5. **Verify** - App auto-checks blockchain for payment
6. **Done** - See transaction hash and success screen

## Quick Start

Just open `crypto-payment-real-verification.html` in your browser. No installation needed.

### Folder Structure

```
crypto-payment-real-verification.html
nwc.jpg                              (favicon)
images/
  ├── polygon.png
  └── solana.png
```

## Configuration

Click the ⚙️ settings button (bottom-right) to configure:
- Your Polygon wallet address (starts with 0x)
- Your Solana wallet address
- Settings are saved locally in your browser

## Supported Networks

- **Polygon** - Fast, cheap transactions (5-30 sec verification)
- **Solana** - Instant transactions (instant verification)

## How Verification Works

When you click "I've Sent the Payment", the app:
- Scans the blockchain every 5 seconds for 75 seconds
- Looks for incoming USDT to your wallet
- Verifies amount matches (within 0.01 USDT)
- Confirms transaction is recent (within 45 minutes)
- Shows success with transaction hash

## Built With

- HTML5
- CSS3 (with modern animations)
- Vanilla JavaScript
- QRCode.js library
- Etherscan API (Polygon verification)
- Helius API (Solana verification)

## Browser Support

Works on all modern browsers:
- Chrome/Edge
- Firefox
- Safari
- Mobile browsers

## Privacy

- No backend server
- All data stored locally in browser
- No tracking or analytics
- Fully open source

## License

MIT License - Use freely

---

**Questions?** Check the [documentation](NoWalletConnect-Docs.pdf) or open an issue.
