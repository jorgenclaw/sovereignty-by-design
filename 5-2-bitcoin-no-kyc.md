# Section 5.2 — Getting Bitcoin Without KYC

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## What You're Setting Up

Most ways to buy Bitcoin require you to hand over your name, address, and government ID before you can make a single purchase. This is called KYC — Know Your Customer — and it creates a permanent record connecting your real identity to your Bitcoin.

This section covers two tools that let you buy Bitcoin without any of that: *Bisq* and *RoboSats*. Both connect you directly with another person who wants to sell. No company in the middle. No ID. No account.

The tradeoff: these tools take a little more patience and setup than a regular exchange. You're exchanging directly with another human, which means coordinating payment methods and waiting for confirmations. It's worth it.

---

## The Two Tools

| Tool | Best for | Speed | Fees | Setup needed |
|------|----------|-------|------|--------------|
| *Bisq* | Larger amounts, cash-by-mail, maximum privacy | Minutes to 8 days depending on payment method | ~1.3% | Download app, small BTC deposit to start |
| *RoboSats* | Smaller amounts, fast trades, simplest start | Average 8 minutes | ~0.2% | Just a browser or Android app |

If you're buying for the first time and want the simplest path: *start with RoboSats*.

If you want maximum privacy (including cash-by-mail or postal money orders) and are willing to invest in a fuller setup: *use Bisq*.

---

## Option A: RoboSats (Recommended Starting Point)

### What It Is

RoboSats is a Lightning-based peer-to-peer exchange. Lightning (explained in Section 5.5) is a faster payment layer built on top of Bitcoin. Trades on RoboSats settle in about 8 minutes on average, and fees are low — about 0.2% of the trade amount.

You don't create an account. Each time you use RoboSats, you get a "robot" — an anonymous identity generated from a secret token you save. Use it once and discard it, or reuse it. No one knows who you are.

### What You Need Before Starting

• A Lightning-compatible Bitcoin wallet (Phoenix, Zeus, or Mutiny are good choices — see Section 5.4 on wallets). You'll need a small amount of Bitcoin already in the wallet to place a "fidelity bond" (a tiny deposit that proves you're serious — you get it back after the trade).
• A way to send fiat to the seller: Zelle, Revolut, Cash App, or another payment method both you and the seller agree on.

### How to Access It

RoboSats is designed to be used privately. These access methods are listed from most to least private:

1. *Tor Browser (most private):* Download from torproject.org and visit `robosats.org` — it automatically routes to the private version.
2. *Android app:* Download the APK directly from the official GitHub page (github.com/RoboSats/robosats/releases). The app handles privacy internally.
3. *Clearnet (not recommended):* There is a clearnet address but it offers no privacy guarantees. Don't use it for real trades.

### Making Your First Trade (Step by Step)

1. Open RoboSats via Tor or the Android app.
2. A robot avatar is generated for you automatically. *Save your robot token* — this is your identity for this trade. If you lose it, you lose access to any active trades.
3. Browse the order book. You'll see a list of sellers with their price premium (how much above or below market rate they're asking), the payment method they accept, and the trade limits.
4. Choose an offer that matches your payment method and tap "Take Order."
5. Both you and the seller lock a small fidelity bond via a Lightning hold invoice (think of it as a handshake deposit — it's locked but not spent unless someone behaves badly).
6. The seller also locks the full Bitcoin amount in escrow.
7. Send the fiat payment to the seller using the agreed method (Zelle, Revolut, etc.), then tap "Confirm Fiat Sent."
8. The seller confirms they received the fiat. The Bitcoin is released to your wallet instantly over Lightning.
9. Both fidelity bonds are returned. Trade complete.

*Success looks like:* Bitcoin arrives in your Lightning wallet, usually within 10 minutes of starting.

*Trade limits:* Minimum ~20,000 sats (~$20). Maximum ~4,000,000 sats (~$4,000 at current prices). For larger amounts, use Bisq instead.

### Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| No offers matching your payment method | The order book is thin right now | Check back later or create your own offer (you become the "maker" and wait for a taker) |
| Trade window expired | You or the seller didn't confirm in time | The fidelity bond may be partially forfeited; open a dispute if the seller is at fault |
| Lightning payment fails | Your wallet doesn't have a route or sufficient balance | Try a different Lightning wallet or increase your wallet's inbound liquidity |
| Robot token lost | You can't access your active trade | Always back up the token immediately — write it down when you start |

### Official Documentation

learn.robosats.org — full guides, fee structure, and access instructions.

---

## Option B: Bisq (For Larger Amounts or Cash)

### What It Is

Bisq (pronounced "bisk") is a desktop application that runs entirely on your own computer. There are no company servers. When you trade, your Bitcoin is locked in a multi-signature escrow (a security system that requires both parties to unlock — Bisq itself cannot touch your funds). All traffic is routed through Tor automatically.

Bisq supports cash-by-mail and postal money orders, which are among the highest-privacy payment methods available — no digital trail whatsoever.

There are two versions currently active:

• *Bisq Easy (Bisq 2):* Simpler for small-to-medium amounts. No security deposit required for buyers. Sellers charge a higher premium (10–15% above market) in exchange for the lower barrier. Good starting point.
• *Bisq v1 (original):* Better for larger amounts. Uses a security deposit (a small amount of Bitcoin you put up as collateral). Stronger protections, lower premiums, but requires you to already have a small amount of BTC.

### Installing Bisq

1. Download the installer from bisq.network — choose the version for your operating system (Windows, Mac, or Linux).
2. Install and launch. The app will connect to the Bisq P2P network over Tor automatically. First sync takes a few minutes.
3. No account creation. You're connected.

### If You're Using Bisq v1: The Security Deposit

Bisq v1 requires you to deposit a small amount of Bitcoin before trading (roughly 15% of the trade value as a security deposit). This is returned after a successful trade. If you don't have any Bitcoin yet, start with Bisq Easy or RoboSats first, then use Bisq v1 for larger trades later.

### Payment Methods by Privacy Level

| Method | Privacy | Region | Notes |
|--------|---------|--------|-------|
| Cash in person (face-to-face) | Highest | Anywhere | Meet locally, exchange cash directly |
| Cash by mail | Very high | Global | Mail physical cash; use a pseudonym |
| US Postal Money Order | Very high | USA | Purchased with cash at any post office |
| Cash deposit (bank) | High | Global | Deposit cash at a branch |
| Zelle | Moderate | USA | Fast (minutes), but ties to bank account |
| Revolut | Moderate | Global/EUR | Same-day, works across borders |

*For maximum privacy:* Use cash by mail or postal money orders. You can use a pseudonym in the Bisq payment account — your real name is never required. For a return address, use a PO Box or a secondary address.

### Making a Trade on Bisq

1. Open Bisq and go to the "Buy BTC" tab.
2. Browse offers or post your own.
3. Choose an offer matching your preferred payment method and amount.
4. Confirm the trade — both sides lock their deposits into escrow.
5. Send the fiat payment to the seller using the agreed method.
6. Confirm in Bisq that you've sent the payment.
7. The seller confirms receipt. The escrow releases to your wallet.
8. Both security deposits are returned.

*Success looks like:* Bitcoin arrives in your Bisq wallet, and security deposits are returned. The whole process takes minutes (Zelle) to days (cash by mail).

### Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| App won't connect / sync stuck | Tor is taking time or your connection is slow | Give it 5–10 minutes; check that Tor isn't blocked by your network |
| Account not signed (v1) | New accounts have buy limits for your first 60 days | Start with smaller amounts; limits increase automatically after successful trades |
| Seller not responding | They may be offline or slow | Use the in-app dispute resolution; mediators can review the trade |
| Trade window expired | Time limit passed without both sides confirming | Open a dispute immediately within the app |
| High premiums | Sellers on Bisq Easy charge more for the reduced friction | This is expected — you're paying a privacy premium. Use Bisq v1 for better rates if you have BTC for the deposit |

### Official Documentation

bisq.network — main site
bisq.wiki — detailed guides on every payment method, including cash by mail

---

## Bisq vs. RoboSats: Quick Reference

| Question | Answer |
|----------|--------|
| I want to start today with no Bitcoin at all | RoboSats (Bisq Easy also works) |
| I want to buy $500+ in one trade | Bisq v1 |
| I want to use cash or postal money orders | Bisq |
| I want it done in under 15 minutes | RoboSats |
| I want the lowest fees | RoboSats (0.2% vs 1.3%) |
| I want to never touch a website or app server | Bisq (fully local desktop app) |

---

## A Note on Premium Pricing

Both tools involve trading at a slight premium over the "spot" market price you'd see on a regular exchange. This is normal and expected — the seller is providing a privacy service. Think of it as what you pay to keep your name off a list. The premium is typically 1–5% on RoboSats and varies on Bisq depending on the payment method and current demand.

---

*Next: Section 5.3 — Getting Bitcoin With KYC (and why you might)*
