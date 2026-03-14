# Section 5.3 — Getting Bitcoin With KYC (and When That's Okay)

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Disclosure: The River link in this section is a referral link. You may receive a sign-up bonus, and Scott receives a small benefit. River is recommended because it is genuinely the best KYC on-ramp option for most US-based beginners — not because of the referral. If you'd prefer to use River without the referral, go directly to river.com.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## What You're Setting Up

KYC stands for Know Your Customer — the identity verification process (name, address, government ID, sometimes a selfie) that most exchanges require before you can buy Bitcoin. When you go through KYC, your identity is permanently linked to that Bitcoin purchase in the exchange's records.

This isn't automatically a reason to avoid it. It depends on your situation and goals.

This section covers *River.com*, a reputable Bitcoin-only exchange for when you want to get started quickly and are comfortable with the tradeoffs.

Think of River as the shallows — the place where people learn to swim before heading into deeper water. Most of the world isn't ready to jump straight to no-KYC peer-to-peer trading. River is where they start.

---

## Should You Use KYC or No-KYC?

There's no universal right answer. Here's a simple way to think about it:

| Your situation | Recommendation |
|----------------|----------------|
| You want to start today with no technical setup | KYC (River) to get started now; learn no-KYC tools later |
| You have time and want full privacy from the start | No-KYC (Bisq or RoboSats, Section 5.2) |
| You're buying a large amount and want regulatory clarity | KYC — some people prefer a clean paper trail for tax purposes |
| You're already public about owning Bitcoin | KYC is probably fine for you |
| Your threat model includes government surveillance | No-KYC only |

The main point: *getting started imperfectly is better than not getting started.* You can always acquire more Bitcoin through no-KYC methods later. The Bitcoin you buy through KYC doesn't contaminate the rest of your stack — they're separate.

---

## Why River

River (river.com) is a Bitcoin-only exchange based in the United States. "Bitcoin-only" matters: they don't offer altcoins, which keeps them focused on doing one thing well. They've built a reputation for clean self-custody practices and don't pressure you to leave your Bitcoin on their platform.

What makes River worth recommending over other KYC options:

• *Bitcoin only* — no distractions, no temptation to speculate on other assets
• *Built-in Lightning support* — you can receive Bitcoin directly over Lightning, not just on-chain
• *Strong self-custody guidance* — they actively encourage you to withdraw to your own wallet
• *Recurring buys* — set it and forget it if you want to accumulate gradually
• *US-based* — simpler regulatory picture for US customers

---

## The Critical Step: Moving Bitcoin Off the Exchange

The most important thing you can do after buying Bitcoin on any exchange is *move it to a wallet you control*.

When Bitcoin sits on an exchange, the exchange holds the keys. You hold an IOU. Exchanges have been hacked (Mt. Gox, 2014: 850,000 BTC gone), frozen accounts during market volatility, gone bankrupt (FTX, 2022), and been forced to freeze withdrawals by regulators.

The phrase used in the Bitcoin community is: *"Not your keys, not your coins."*

*Move your Bitcoin off River to your own wallet as soon as you buy it.* See Section 5.4 for wallet options.

---

## Getting Started on River

*→ Use the referral link to get a sign-up bonus: [river.com/invite?r=6AI22CQ7BM](https://river.com/invite?r=6AI22CQ7BM)*
*(Or go directly to river.com if you prefer no referral.)*

1. Go to river.com and create an account.
2. Complete identity verification (name, address, government ID). This is the KYC step.
3. Link a bank account or debit card to fund purchases.
4. Buy Bitcoin.
5. *Immediately withdraw to your own wallet.* Don't leave it sitting on the exchange.

*Success looks like:* Bitcoin arrives in your own wallet (not held by River) within minutes of purchasing.

---

## Setting Up Recurring Purchases (Dollar-Cost Averaging)

One of the most effective ways to accumulate Bitcoin without trying to time the market is to buy a fixed dollar amount on a regular schedule — weekly, bi-weekly, or monthly. This is called dollar-cost averaging (DCA), and it smooths out price volatility over time.

River supports automatic recurring purchases. Set up a recurring buy, configure automatic withdrawal to your own wallet address, and let it run.

*Important:* Make sure the auto-withdrawal is configured. If Bitcoin accumulates on the exchange between withdrawals, you're back to the "not your keys" problem.

---

## After You're Comfortable: Mixing KYC and No-KYC

Over time, many people build a stack that includes both KYC and no-KYC Bitcoin. These are separate acquisitions and don't need to interact with each other. Your privacy-oriented purchases (via Bisq or RoboSats) and your convenience purchases (via River) can live in separate wallets with no connection between them.

This is a reasonable long-term approach: River for ease and recurring buys, Bisq/RoboSats when you want to add to your stack privately.

---

## Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| Verification rejected | ID unclear, address mismatch, or restriction on your account type | Try re-submitting with a clearer photo; contact River support |
| Bank transfer taking too long | ACH transfers typically take 1–3 business days | Plan ahead; use debit card for faster (but usually more expensive) funding |
| Withdrawal not arriving in my wallet | Network congestion or you used the wrong address | Check the transaction ID on a block explorer (mempool.space); confirm you used the correct wallet address |
| Forgot to set up auto-withdrawal | Bitcoin is sitting on River | Set it up now under account settings; withdraw manually in the meantime |

---

## A Honest Note on Privacy

KYC Bitcoin is permanently tied to your identity in River's database. If River is breached, subpoenaed, or required to report to regulators, that record exists. This is the real tradeoff.

For most people in most circumstances, this is an acceptable tradeoff for the convenience. For people with higher privacy requirements — journalists, activists, people in politically complicated situations, or anyone whose government might target Bitcoin holders — no-KYC from the start is worth the extra effort.

Only you can decide which applies to you. This guide tries to give you both options.

---

*See also:*
- *Section 5.2* — No-KYC options (Bisq, RoboSats)
- *Section 5.4* — Wallets: where to keep what you own
- *Section 5.5* — Lightning payments
