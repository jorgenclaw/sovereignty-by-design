# Part 1 — Foundation

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## 1.1 — What Sovereignty Actually Means

### The One-Sentence Version

Sovereignty means you control your own tools — not just that you use them.

### The Longer Version

Most people use technology by renting access to it. Your email lives on Google's or Microsoft's servers. Your messages pass through Apple's or Meta's infrastructure. Your money sits at a bank that can freeze your account. Your photos are stored on iCloud, which can be subpoenaed. Your AI assistant remembers your conversations in a data center you've never seen.

None of this is secret. It's just the default. And for most people in most circumstances, the default works fine.

But defaults have costs you don't see on a regular day:

• *Your data is a product.* Companies read your email to serve you ads. They analyze your behavior to build profiles they sell to advertisers, insurers, and data brokers.
• *You're one court order away from exposure.* Subpoenas and government requests to tech companies are routine. You don't have to be a criminal for your data to become evidence in someone else's case.
• *Platforms can disappear or change.* Services shut down, get acquired, change their terms. If your life is built on infrastructure you don't control, someone else can pull the rug.
• *Data breaches are permanent.* Once a company is breached and your data is sold, it cannot be unsold. It circulates forever.

Sovereignty is the practice of moving your critical tools — communications, money, identity, data — onto infrastructure you control. Not because you have something to hide, but because control over your own tools is a reasonable thing to want.

### Privacy and Sovereignty Are the Same Thing

Privacy is sovereignty applied to information. Sovereignty is privacy applied to infrastructure.

They're not separate disciplines. Keeping your messages private requires controlling the pipes they travel through. Controlling your money requires keys that only you hold. Controlling your identity requires a network where no single company can silence or impersonate you.

This guide treats them as one project.

### What This Is Not

This guide is not about:

• *Hiding illegal activity* — the tools here are the same ones journalists, activists, abuse survivors, and ordinary privacy-conscious people use. The goal is control, not concealment.
• *Achieving perfect anonymity* — total anonymity is practically impossible. The goal is meaningful privacy: reducing exposure to reasonable levels, not eliminating it entirely.
• *Financial privacy tricks* — we won't teach you how to hide assets from creditors, move money across borders illegally, or avoid taxes. Those aren't sovereignty topics; they're legal problems.

### The Threat Model Question

Every privacy decision starts with a threat model — a clear picture of what you're protecting and who you're protecting it from.

Useful questions to ask yourself:

• Who would benefit from knowing things about me that I haven't chosen to share?
• What would the consequences be if my messages, location, or finances were exposed?
• Am I protecting against casual data harvesting, targeted surveillance, or something specific?

Most people are not targets of sophisticated state-level surveillance. Most people are protecting themselves from data brokers, corporate data harvesting, and the aggregated consequences of living in a world where your information is constantly being collected and sold. That's a real problem worth solving, and the tools in this guide address it.

If your threat model is more serious — journalism in a hostile country, whistleblowing, domestic safety situations — this guide is still relevant, but you'll want to go deeper on operational security practices. Michael Bazzell's *Extreme Privacy* covers that layer; it's listed in the Further Reading appendix.

---

## 1.2 — How to Use This Guide

### Start Here If You Have One Hour

Don't try to do everything at once. The sovereignty stack is a project, not a one-day fix. Here's where to start:

*Week 1 — Communications:*
• Get Signal with a VoIP number (Section 3.1)
• Install Molly so you can have two Signal accounts if needed (Section 3.1)
• Enable disappearing messages on all Signal conversations

*Week 2 — Network:*
• Set up a VPN on your phone and computer (Section 4.2)
• Change your DNS settings to a privacy-respecting provider (Section 4.1)

*Week 3 — Money:*
• Buy a small amount of Bitcoin (Section 5.2 or 5.3)
• Move it to a wallet you control (Section 5.4)

After those three weeks, you've done more than 90% of people. Everything else in this guide is refinement, depth, and specific use cases.

### How the Sections Are Organized

The guide is structured in layers, each building on the previous:

| Part | Topic | What it addresses |
|------|-------|-------------------|
| Part 1 | Foundation | Why this matters, how to use this guide |
| Part 2 | Device & OS | Your phone and computer as the hardware layer |
| Part 3 | Communications | How your messages travel and who can read them |
| Part 4 | Network | What your internet traffic reveals |
| Part 5 | Money | Bitcoin and financial sovereignty |
| Part 6 | Your Own Node | Verifying your own transactions |
| Part 7 | AI Agent Layer | A sovereign AI assistant inside the stack |

You can read it linearly or jump to the section that matters most right now.

### A Note on Progress Over Perfection

You don't need to do everything in this guide to benefit from any of it. Using Signal for your most sensitive conversations — even while still using iMessage for everything else — is meaningfully better than not using Signal at all. Having Bitcoin in a wallet you control — even a small amount — teaches you something you can't learn by reading.

Start somewhere. Improve from there.

---

## 1.3 — The Sovereignty Stack (Overview)

### The Full Picture

A complete sovereignty stack looks like this:

```
┌─────────────────────────────────────────────────────┐
│                   AI AGENT LAYER                     │
│         NanoClaw — your sovereign assistant          │
├─────────────────────────────────────────────────────┤
│                    MONEY LAYER                       │
│   Bitcoin (Lightning + on-chain) · Monero · Cashu   │
├─────────────────────────────────────────────────────┤
│                 COMMUNICATIONS LAYER                  │
│         Signal/Molly · White Noise · Nostr           │
├─────────────────────────────────────────────────────┤
│                   NETWORK LAYER                      │
│          VPN (Mullvad) · DNS (Quad9/NextDNS)         │
├─────────────────────────────────────────────────────┤
│              DEVICE & OPERATING SYSTEM               │
│         GrapheneOS (phone) · Linux (desktop)         │
└─────────────────────────────────────────────────────┘
```

Each layer protects the one above it. A secure device makes secure communications more effective. A private network protects everything passing through it. Sovereign money means your economic life doesn't depend on a bank's goodwill.

You don't have to build all of this at once. Each layer stands on its own. A person using Signal on a stock Android phone has done something valuable — even without GrapheneOS underneath. Build the stack in the order that makes sense for where you are.

### What Each Layer Does

*Device & OS:* Your phone and computer are the foundation. A compromised device undermines everything else. GrapheneOS on a Pixel phone is the strongest available mobile option for most people. Linux is the equivalent for desktop.

*Network:* Your internet connection and DNS queries tell your ISP exactly which services you use. A VPN shifts that visibility away from your ISP. Encrypted DNS prevents your queries from being logged at the network level.

*Communications:* The most immediate win for most people. Switching from SMS and email to end-to-end encrypted channels (Signal, White Noise) closes the most common surveillance vectors.

*Money:* Bitcoin is self-custodied money: no bank required, no account to freeze, no permission needed to transact. Lightning makes small payments fast and cheap. Monero and Litecoin with MWEB add optional privacy.

*AI Agent:* The layer this guide adds that others don't cover. Running an AI assistant inside your sovereignty stack gives you a capable, persistent assistant without feeding your daily life into a tech company's data warehouse.

### What It Costs

The full stack has real costs:

• *Time:* Setting everything up takes time. Maintaining it takes less, but it's ongoing.
• *Money:* A Mullvad VPN subscription runs about $5/month. A ProtonMail account (free tier works for most use cases). A Pixel phone if you're switching to GrapheneOS. Bitcoin — start with whatever you can afford, even $20.
• *Convenience:* Some things are harder with a sovereignty stack. Some apps don't work on GrapheneOS without Google services. Some merchants don't accept Bitcoin. The tradeoffs are real.

None of these costs are prohibitive. The sovereignty stack is accessible to anyone with a modest budget and willingness to invest a few weekends.

---

*Next: Part 2 — Device & Operating System*
