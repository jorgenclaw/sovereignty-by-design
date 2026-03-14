# Section 5.6 — Other Proof-of-Work Privacy Coins: Monero and Litecoin with MWEB

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## What This Section Covers

Bitcoin is the foundation of a sovereign money stack. But Bitcoin's on-chain transaction history is public — every transaction is visible on the blockchain forever. For most purposes this is fine. For some, it's a problem.

Before reaching for another coin, it's worth understanding what Bitcoin already has for privacy — and where its limits are.

---

## Bitcoin's Own Privacy Tools

### Lightning Network (The Middle Ground)

The first and most practical privacy upgrade for Bitcoin isn't a different coin — it's Lightning (Section 5.5). Lightning payments don't appear on the blockchain at all. They're off-chain, encrypted, and invisible to anyone not directly involved in routing. For everyday spending, Lightning provides meaningful privacy without any specialized knowledge or tooling.

*If you're using Lightning for daily transactions, you're already doing more for your Bitcoin privacy than most on-chain techniques provide.*

### CoinJoin — Bitcoin On-Chain Privacy (The Deep End)

For on-chain Bitcoin that needs more privacy, *CoinJoin* is the established technique. CoinJoin works by combining multiple people's Bitcoin transactions into one large transaction, making it difficult to determine which inputs correspond to which outputs. An outside observer sees one big transaction — not which coins went where.

*Wasabi Wallet* (wasabiwallet.io) is the most mature CoinJoin implementation for Bitcoin. It automates the coordination of CoinJoin rounds with other users and includes coin control so you can manage which outputs you spend together.

A few things worth knowing:
• CoinJoin is legal. It's not money laundering — it's mixing your own coins with other people's coins for privacy.
• Some exchanges flag coins that have been through CoinJoin and may ask questions. This is annoying but not illegal.
• CoinJoin requires more sophistication than Lightning. You need to understand UTXO management and spending discipline to use it effectively.
• *As Bitcoin scales toward billions of users, CoinJoin may become less necessary* — the sheer volume of transactions makes individual tracking harder, and second-layer privacy (Lightning, Cashu) handles most use cases. For now it's a meaningful tool, but it's the deep end of the pool.

### PayJoin — Collaborative Transactions

*PayJoin* (also called Pay-to-EndPoint or P2EP) is a privacy technique where sender and receiver collaborate to build a transaction that looks like a simple payment but actually breaks chain analysis assumptions. Unlike CoinJoin, which combines transactions from strangers, PayJoin happens between the two parties of an actual payment.

PayJoin is less common today — it requires both the sender's and receiver's wallet to support it. But as support grows, it's a elegant privacy improvement that doesn't require any third parties or coin mixing rounds.

---

### The Privacy Depth Chart

| Layer | Tool | Privacy level | Who it's for |
|-------|------|--------------|--------------|
| Lightning | Zeus / Cake Wallet | Good — off-chain, not publicly visible | Everyone doing everyday payments |
| Lightning + Cashu | Zeus with Cashu | Strong — blind signatures, no link to identity | Anyone wanting cash-like privacy |
| On-chain CoinJoin | Wasabi Wallet | Strong — breaks chain analysis | Advanced users with on-chain privacy needs |
| PayJoin | Wallet-dependent | Moderate — breaks basic chain analysis | Users whose wallets support it |
| Monero | Cake Wallet / Feather | Maximum — private by default at protocol level | When Bitcoin privacy tools aren't enough |

Bitcoin is the ocean — the base layer, the reserve, the thing everything else swims in. On-chain transparent Bitcoin is the shallows. Lightning is the middle. CoinJoin is the deep end. Monero is a different pool — opaque walls, no windows — for when you need to disappear entirely. But you always come back to the ocean.

---

This section covers two proof-of-work (PoW) coins that add privacy at the protocol level for when Bitcoin's own tools aren't sufficient:

• *Monero (XMR)* — privacy by default. Every transaction is confidential. No one can see how much you sent or who you sent it to.
• *Litecoin with MWEB (LTC)* — privacy by choice. Litecoin is Bitcoin's closest cousin, with optional confidential transactions through a feature called MWEB.

Both are proof-of-work coins, which matters: like Bitcoin, they're secured by real computational work (mining), not by a vote or a committee. That's a meaningful distinction from proof-of-stake coins in terms of credible neutrality and security model.

These aren't replacements for Bitcoin. They're tools for specific jobs.

---

## Monero (XMR)

### What Makes It Different

On the Bitcoin blockchain, every transaction is permanently public. You can look up any address and see its full history. This is by design — Bitcoin's transparency is part of what makes it auditable and trustworthy as a ledger.

Monero is designed the opposite way. Privacy is the default, not an option. Every Monero transaction hides:

• *Who sent it* — using "ring signatures" that blend your transaction with others, making the sender indistinguishable from a group
• *How much was sent* — using "confidential transactions" that mathematically verify the amount is valid without revealing the number
• *Who received it* — using "stealth addresses" that generate a one-time address for every transaction, so the recipient's real address never appears on the blockchain

The result: Monero's blockchain is functionally unreadable to outside observers. Even with full access to the chain, you can't reconstruct who paid whom or for how much.

### When to Use Monero

Monero isn't a better version of Bitcoin — it serves a different role. Consider it for:

• *Paying for services where you don't want a permanent record* — subscriptions, digital purchases, donations
• *Receiving payment for services without revealing your running total* — useful if you receive tips or payments publicly but don't want the amounts aggregated
• *Converting out of Bitcoin* — if you have Bitcoin you'd like to make private, converting to Monero and back (through a non-KYC swap service) breaks the chain of traceability

### Getting Monero

The easiest non-KYC path: use a swap service like Cake Wallet's built-in swap or a dedicated swap tool. You send Bitcoin, you receive Monero, no account required. See Section 5.4 on Cake Wallet.

For Monero purchased with fiat: Bisq supports Monero trading (not just Bitcoin), and the same no-KYC approaches from Section 5.2 apply.

### Storing Monero

*Cake Wallet* (Section 5.4) supports Monero natively alongside Bitcoin and Litecoin. It's the simplest path. For higher security, a dedicated Monero wallet (Feather Wallet on desktop, or Monerujo on Android) gives you more control.

### A Note on the Debate

Bitcoin maximalists will tell you Monero is unnecessary — that privacy improvements coming to Bitcoin will eventually close the gap. They may be right eventually. In the meantime, Monero is the only coin with privacy built in by default and a decade-long track record of that privacy holding up under scrutiny. It's a working tool right now.

---

## Litecoin with MWEB

### What Litecoin Is

Litecoin (LTC) launched in 2011 as a close technical sibling to Bitcoin. The key differences:

| Feature | Bitcoin | Litecoin |
|---------|---------|----------|
| Block time | 10 minutes | 2.5 minutes (4× faster) |
| Transaction fee | ~$0.49 average | ~$0.006 average (80× cheaper) |
| Supply cap | 21 million | 84 million |
| Mining algorithm | SHA-256 | Scrypt |

Litecoin doesn't aim to replace Bitcoin. Its advocates describe it as "digital silver" to Bitcoin's "digital gold" — a faster, cheaper option for everyday transactions while Bitcoin handles larger stores of value.

### What MWEB Is

MWEB stands for *MimbleWimble Extension Blocks* — a privacy upgrade that activated on Litecoin in May 2022. It's Litecoin's biggest protocol change to date.

MWEB adds optional confidential transactions to Litecoin:

• *Amounts are hidden* — when you move LTC into the MWEB layer, transaction amounts are no longer visible on the public blockchain
• *Native transaction mixing* — MWEB includes built-in CoinJoin (a technique that blends multiple transactions together to obscure who paid whom)
• *Stealth addresses* — recipients can receive funds to a one-time address, so their main address doesn't appear on the chain

The key word is *optional*. You choose when to move funds into the MWEB layer for privacy, and when to move back out to the regular transparent chain. MWEB transactions stay in "extension blocks" alongside (but separate from) the main Litecoin chain.

### How MWEB Compares to Monero

| Aspect | Monero | Litecoin MWEB |
|--------|--------|---------------|
| Privacy by default | Yes — always on | No — you opt in |
| Privacy strength | Very strong | Strong when used |
| Transaction speed | ~2 minutes | ~2.5 minutes |
| Exchange support | Limited (many exchanges avoid it) | Growing, but some exchanges warn against MWEB use |
| Ecosystem maturity | 10+ years of privacy-focused development | MWEB activated 2022, still maturing |
| Use case | When you need strong default privacy | When you want cheap fast transactions with optional privacy |

### When to Use Litecoin with MWEB

• *Small, frequent transactions* where Bitcoin's fees would eat into the value — sending $5 worth of value on-chain in Bitcoin costs ~$0.49 in fees; on Litecoin it costs ~$0.006.
• *On-chain payments without Lightning complexity* — Litecoin's low fees make on-chain transactions practical for everyday amounts, without the channel management that Lightning requires.
• *Privacy for specific transactions* — move funds into MWEB when you need confidentiality, then back out when you want interoperability.

### Exchange Caution

Some exchanges (including Binance) have issued warnings about MWEB transactions due to regulatory pressure around privacy features. This is worth knowing: if you ever want to move MWEB Litecoin to an exchange for sale, verify they support it first. Coinbase and Bitget have supported MWEB; others haven't.

This isn't unique to Litecoin — Monero faces the same friction with exchanges. It's a sign that these privacy features are working, not that they're flawed.

### Getting and Storing Litecoin

*Cake Wallet* supports Litecoin with MWEB natively — the simplest option. You can receive, send, and opt into MWEB from within the same wallet you use for Bitcoin and Monero.

To get Litecoin without KYC: Bisq supports LTC trading. The same P2P marketplace approach from Section 5.2 applies — search for LTC offers rather than BTC offers.

---

## Side-by-Side Summary

| Question | Best choice |
|----------|-------------|
| I need strong privacy with no configuration | Monero |
| I want cheap fast payments with optional privacy | Litecoin with MWEB |
| I want to obfuscate Bitcoin holdings | Convert to Monero, then back to Bitcoin via non-KYC swap |
| I'm new to all of this | Start with Bitcoin; revisit this section when the basics are solid |

---

## The Honest Picture

None of these coins are Bitcoin. Bitcoin's security, network effects, and proven track record are unmatched. Monero and Litecoin serve specific roles where Bitcoin's design isn't optimal.

For most people, the right progression looks like this:

1. *Get Bitcoin.* Self-custody it. (Sections 5.2–5.4)
2. *Use Lightning for everyday payments.* This handles most privacy needs without any extra tooling. (Section 5.5)
3. *Add Cashu for cash-like privacy.* If Lightning isn't private enough for a specific transaction. (Section 5.4)
4. *Reach for Monero when you need maximum protocol-level privacy.* When Bitcoin's tools aren't enough.
5. *Learn CoinJoin (Wasabi) if you have specific on-chain privacy requirements.* This is the deep end — powerful, but requires discipline.

You don't need to hold all of these coins. You don't need to do all of these things. Privacy is a spectrum and a practice — you do what fits your threat model and your current knowledge.

The goal isn't to collect coins. It's to have the right tool for each job, and to know when you need it.

---

*See also:*
- *Section 5.1* — Why Bitcoin (the foundation)
- *Section 5.2* — Getting Bitcoin without KYC
- *Section 5.4* — Wallets (Cake Wallet supports Monero and Litecoin alongside Bitcoin)
- *Further Reading: Appendix D* — getmonero.org, litecoin.com/mweb
