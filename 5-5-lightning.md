# Section 5.5 — Lightning Payments

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## What You're Setting Up

The Lightning Network is a payment layer built on top of Bitcoin. While a regular Bitcoin transaction takes 10 minutes to an hour and costs variable fees that can reach dollars during busy periods, Lightning transactions settle in under a second and cost fractions of a cent.

Lightning is what makes Bitcoin practical as everyday money — and it's also Bitcoin's primary privacy layer.

This second point is underappreciated. On-chain Bitcoin is fully public: every transaction, every amount, every address is visible on the blockchain forever. Lightning payments don't go on the blockchain at all. They route through a network of encrypted channels. No one monitoring the blockchain can see that a payment happened, how much it was, or who the parties were. The payment exists only between the two participants and the routing nodes — none of whom see the full picture.

For the vast majority of people doing everyday transactions, Lightning isn't just more convenient than on-chain Bitcoin — it's meaningfully more private. It doesn't require understanding coinjoin, running a full node, or holding Monero. Lightning with Cashu on top (see Section 5.4) gives you practical financial privacy without specialized knowledge.

Think of it this way:
- *On-chain Bitcoin:* your transactions are public ledger entries, permanent and traceable. Fine for large settlements and savings.
- *Lightning:* payments are off-chain, fast, cheap, and not visible to blockchain observers. The right tool for everyday spending.
- *Cashu on Lightning:* fully private digital cash. Even the Lightning routing nodes don't see the full picture.

---

## How Lightning Works (Plain English)

The Bitcoin blockchain is like sending money by certified mail — secure, permanent, traceable, but not fast. Lightning is like having a running tab with someone you trust.

When you open a Lightning channel with another person or service, you both lock some Bitcoin into a shared account on the blockchain (one transaction). From that point, you can send payments back and forth instantly, as many times as you want, without touching the blockchain. When you're done, you close the channel and the final balances settle on-chain (one more transaction).

Across a network of millions of these channels, payments can route from your wallet to anyone else's through a chain of connected channels — automatically, in milliseconds — without either party needing to have a direct channel to each other.

You don't need to understand the technical details to use it. From a user perspective: you scan a QR code, confirm the amount, and the payment arrives.

---

## Lightning Addresses

A Lightning address looks like an email address: `you@wallet.domain`

This is much more user-friendly than a traditional Bitcoin address (a long string of letters and numbers that changes every time). Lightning addresses are permanent, easy to share, and work across any Lightning-compatible wallet.

Examples of what they look like:
- `scott@zeusln.app`
- `jorgenclaw@npub.cash`

If you're setting up Zeus with your own node, you can get a Lightning address through services like Olympus by Zeus. Cake Wallet provides Lightning addresses in the format `you@cake.cash`.

---

## What You Can Pay For With Lightning

Lightning is accepted by a growing number of merchants and services:

• *Podcasts* — Podcast apps like Fountain stream small payments ("streaming sats") to podcasters while you listen
• *Nostr tips* — Zapping a Nostr post sends Lightning directly to the creator
• *VPN services* — Mullvad accepts Lightning payments (no account needed, maximum privacy)
• *Software and services* — Many open-source projects accept Lightning tips
• *Food and retail* — Growing number of Bitcoin-accepting merchants use Lightning for the speed
• *AI services* — Some AI tools accept Lightning via the L402 micropayment standard

The ecosystem is still growing. Bitcoin with Lightning won't replace your credit card tomorrow. But for digital services and online payments, it's increasingly practical.

---

## Making Your First Lightning Payment

### Option A: Scan a QR Code

Most Lightning payments work like this:

1. The person or service you're paying shows you a Lightning invoice (usually a QR code).
2. Open your Lightning wallet (Zeus or Cake Wallet).
3. Tap "Send" and scan the QR code.
4. Confirm the amount and tap Send.
5. The payment arrives in under a second.

### Option B: Send to a Lightning Address

If you know someone's Lightning address:

1. Open your wallet and tap "Send."
2. Type or paste the Lightning address (e.g., `creator@wallet.domain`).
3. Enter the amount in sats (satoshis — the smallest unit of Bitcoin, 100 million sats = 1 BTC).
4. Confirm and send.

### Receiving Lightning Payments

1. Open your wallet and tap "Receive."
2. Set an amount (optional — you can leave it blank for the sender to choose).
3. Share the QR code or Lightning address with whoever is paying you.
4. Funds arrive in seconds.

---

## Satoshis: The Unit of Lightning

Lightning payments are typically denominated in *satoshis* (often shortened to "sats") — the smallest unit of Bitcoin.

```
1 Bitcoin = 100,000,000 satoshis
1,000 sats ≈ $1 (at $100,000/BTC)
100 sats ≈ $0.10
1 sat ≈ $0.001
```

Getting comfortable thinking in sats rather than Bitcoin makes Lightning easier to use. Sending 500 sats as a tip is clearer in practice than saying "0.000005 BTC."

---

## Cashu: Private Lightning Payments

If you want Lightning payments that are private — where even the Lightning routing nodes can't reconstruct your payment history — Cashu eCash is the answer (see Section 5.4 for setup).

With Cashu:
- You convert Bitcoin into Cashu tokens at a mint
- Tokens can be sent peer-to-peer with no on-chain trace
- The mint uses blind signatures — it can't link tokens to your identity
- Tokens are redeemable back to Bitcoin at any time

Think of Cashu as digital cash: you take money from your wallet (Bitcoin), get bills from an ATM (Cashu mint), and spend those bills privately. The bills work; the ATM doesn't know where you spent them.

---

## Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| "Payment failed — no route found" | There's no path of channels between you and the recipient | Try again (routing is dynamic); if persistent, the recipient may have a liquidity issue |
| Payment stuck / pending for a long time | Rare network issue | Lightning payments either succeed quickly or fail; if stuck over 60 seconds, it will likely refund automatically |
| "Insufficient balance" but I have funds | Your channel doesn't have enough *outbound* liquidity | You need to receive some payments first to open up outbound capacity, or open a new channel |
| Lightning address not working | Address may be offline or the format is wrong | Confirm the address, check that the recipient's wallet is online |
| I want to receive but no one can pay me | You don't have inbound liquidity | This requires opening a channel from the other direction; services like Olympus by Zeus can provide initial inbound liquidity |

---

*See also:*
- *Section 5.4* — Wallets (Zeus for Lightning, Cake Wallet as alternative)
- *Section 6* — Running your own node (connects Zeus to sovereign Lightning infrastructure)
