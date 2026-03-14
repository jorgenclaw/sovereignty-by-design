# Section 5.4 — Wallets: Storing What You Own

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## What You're Setting Up

A Bitcoin wallet is software that manages your cryptographic keys — the passwords that prove you own your Bitcoin. Without a wallet you control, your Bitcoin isn't really yours. This section covers the wallets we recommend and why.

The core principle: *self-custodial, open-source wallets only.* Self-custodial means you hold the keys. Open-source means anyone can inspect the code — there are no hidden backdoors.

---

## The Wallet Landscape

Not all wallets work the same way. Here's the key distinction:

| Type | How it works | Analogy |
|------|-------------|---------|
| *Custodial* | Exchange or company holds your keys | Bank account |
| *Self-custodial (hot)* | You hold your keys, always connected to the internet | Physical wallet in your pocket |
| *Self-custodial (air-gapped)* | Keys on a permanently offline device; transactions passed via QR | Cash in an envelope at home |
| *Hardware wallet* | Keys stored on a dedicated secure chip, never internet-connected | Safe |

This guide only recommends self-custodial and hardware wallets. Custodial wallets defeat the purpose of Bitcoin.

---

## What We Recommend

### Cake Wallet — Best All-in-One for Most People

*What it is:* An open-source mobile wallet (iOS and Android) that supports Bitcoin, Litecoin with MWEB, and Monero in one app. Free to use.

*Why we recommend it:*
• Supports all three coins in this guide in one place
• Litecoin with MWEB — one of the only wallets with full MWEB support built in
• Monero support is first-class, not an afterthought
• Built-in swap between coins (no KYC swap, no extra app)
• Tor support for network privacy
• Open source — code is publicly auditable

*Best for:* Someone who wants Bitcoin, Litecoin, and Monero accessible from one app, with good defaults.

*Website:* cakewallet.com
*Lightning support:* Introduced in Cake Wallet v6 — still maturing; for serious Lightning use, see Zeus below.

---

### Zeus — Best for Lightning and Node Control

*What it is:* An open-source mobile wallet (iOS and Android) that connects to your own Bitcoin Lightning node or to a hosted node provider. Best-in-class for Lightning users who want full control.

*Why we recommend it:*
• You can connect to your own node — no one else can see your transaction history
• Native Cashu eCash integration (see below)
• Tor support for all connections
• AGPL v3 open source license — the strongest open-source licensing available
• Full node control means no third party can freeze your Lightning channels

*Best for:* People who are running (or plan to run) their own Bitcoin node (Section 6), or who use Lightning heavily and want maximum privacy.

*Setup path:* Zeus connects to Parmanode (Section 6) for a fully sovereign setup. This takes more time to configure but gives you the cleanest stack.

*Website:* zeusln.com

---

### Cashu eCash — Private Lightning Payments

*What it is:* Not a wallet you install separately — Cashu is a privacy protocol that some wallets (including Zeus) integrate. It uses blind signatures (a cryptographic technique borrowed from digital cash systems) to make Lightning payments private.

*Why it matters:* Even Lightning transactions leave some metadata. Cashu tokens are like digital cash — the mint that issues them can't see your balance, can't see who you're paying, and can't connect your identity to your transactions. Think of it as the Monero of the Lightning world.

*Best for:* Situations where you want Lightning speed with Monero-level privacy — receiving tips, paying for services where you don't want a record.

*Website:* cashu.space

---

### Cupcake — Free Air-Gapped Wallet (Your Old Phone as a Hardware Wallet)

*What it is:* An open-source app by Cake Labs that turns any old smartphone into an air-gapped signing device. Cupcake holds your private keys on a phone that is completely offline — it has zero network permissions, meaning the OS physically cannot connect it to the internet. It pairs with Cake Wallet on your daily phone: Cake Wallet watches your balance and builds transactions, then passes them to Cupcake via animated QR codes for signing. The signed transaction goes back to Cake Wallet via QR, which broadcasts it.

Your private keys never touch the internet. Ever.

*Why this matters:*
• *Hardware wallet security at zero cost* — An old phone that cost nothing is now doing what a $150 Coldcard does, minus a few advanced features
• *No shipping address* — Buying a hardware wallet means giving a company your home address, which is a privacy and physical security concern. Cupcake has no shipping
• *No import restrictions* — Hardware wallets get seized at customs in some countries. Not a problem with an app
• *Open source* — Code is publicly auditable at github.com/cake-tech/cupcake

*Best for:*
• Anyone with an old Android or iPhone sitting in a drawer — put it to work
• People who want cold storage security without the hardware wallet price tag
• A stepping stone to a dedicated hardware device while building your stack

*What it doesn't do (yet):* Lightning is not supported — Cupcake is on-chain only (Bitcoin and Monero). For Lightning, use Zeus on your daily phone. Litecoin with MWEB support is on the roadmap.

*How to set it up:*
1. Find an old phone — any Android or iPhone you no longer use daily
2. Do a factory reset on it (clean slate)
3. Install Cupcake from cupcakewallet.com (or F-Droid / GitHub for Android)
4. *Put the phone in airplane mode. Never connect it to Wi-Fi or cellular again.*
5. Create a new wallet in Cupcake and write down your seed phrase on paper
6. On your daily phone, open Cake Wallet and add a "watch-only" wallet by scanning the QR code Cupcake shows you
7. To send Bitcoin: Cake Wallet builds the transaction → shows a QR → Cupcake scans it, signs it, shows a QR back → Cake Wallet scans and broadcasts

*Success looks like:* Your Cupcake phone never connects to the internet, your Cake Wallet shows your balance, and you can receive Bitcoin immediately. Sending requires the two-phone QR handoff.

*Website:* cupcakewallet.com

---

### Dedicated Hardware Wallets — For Significant Amounts

*What it is:* A purpose-built physical device that stores your private keys offline. Compared to Cupcake, dedicated hardware wallets offer a dedicated secure element chip, tamper-evident packaging, and a longer support lifecycle.

*When to upgrade from Cupcake to dedicated hardware:* When the amount of Bitcoin in cold storage would meaningfully hurt you if lost. There's no fixed number — use your own judgment.

*Reputable options:*
• *Foundation Passport* (and the newer *Passport Prime*) — Bitcoin-only, open source hardware and firmware, airgapped. Built in the US. Designed for usability alongside sovereignty. The option most aligned with this guide's philosophy.
• *Coldcard* — Bitcoin-only, maximum security, open source. Recommended by Bitcoin security experts. Higher learning curve.
• *Trezor Model T* — Multi-coin, widely used, long track record. Less opinionated than the options above.

*Note:* Buy hardware wallets only from the manufacturer's official website. Never from third-party sellers on Amazon or eBay — tampered devices have been documented in the wild.

---

## Setting Up Your First Wallet

### If You're Starting with Cake Wallet:

1. Download Cake Wallet from the official website (cakewallet.com) or your app store.
2. Choose "Create New Wallet."
3. *Write down your seed phrase — 12 or 24 words — on paper. Store it somewhere safe and offline.* This is the only backup of your wallet. If your phone breaks or is stolen and you don't have this written down, your funds cannot be recovered.
4. Confirm the seed phrase by entering the words in order.
5. Your wallet is ready. Your receiving address is on the main screen — share this address to receive Bitcoin.

*Success looks like:* You can see a receiving address. When you send a small test amount from an exchange and it arrives, you've successfully set up self-custody.

### The Seed Phrase — The Most Important Thing

Your seed phrase (also called a recovery phrase) is everything. It's a list of 12 or 24 common English words that, in the right order, can regenerate your entire wallet on any device. Keep it:

• *Written on paper* — not photographed, not in a notes app, not in email
• *Stored somewhere physically secure* — a fireproof safe is good; a drawer is okay; your phone is not
• *Not shared with anyone* — anyone who has these words has your Bitcoin

No legitimate wallet app, exchange, or support team will ever ask for your seed phrase. If someone asks for it, they're trying to steal your funds.

---

## Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| Sent Bitcoin to the wrong address | Transactions are irreversible on the blockchain | Unfortunately, Bitcoin transactions cannot be reversed; double-check addresses before sending |
| Wallet won't sync / balance not showing | The app is syncing with the network | Give it a few minutes; ensure you have internet connectivity |
| Lost my seed phrase | Without it, you cannot recover your wallet if you lose your device | If you still have the device and wallet access, move funds to a new wallet immediately and write down the new seed phrase |
| App says "transaction failed" | Usually a fee issue or connectivity problem | Increase the fee setting slightly and try again |
| Someone asking for my seed phrase | This is always a scam | Never share it. Block and report. |

---

*See also:*
- *Section 5.5* — Lightning payments (how to use Zeus for fast cheap transactions)
- *Section 6* — Running your own node (connecting Zeus to Parmanode)
