# Privacy & Sovereignty: A Practical Guide

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> Privacy and sovereignty are the same thing. Sovereignty means you control your infrastructure, your communications, and your money. Privacy is what you get when that control holds.

---

## Quick Start

Don't try to do everything at once. Start here:

1. *Communications:* Install Signal + Molly. Switch email to ProtonMail → Part 3
2. *Network:* Add Proton VPN (free tier works). Change your DNS → Part 4
3. *Passwords:* Switch to Proton Pass or Bitwarden (both free, both excellent)
4. *Money:* Get some Bitcoin into a wallet you control → Parts 5.1–5.4

That's enough for week one. The Proton suite handles steps 1–3 under one account if you want simplicity — or mix and match with the FOSS alternatives listed throughout the guide. Everything else is refinement.

---

## Table of Contents

### Part 1: Foundation
- [Part 1: Foundation](part-1-foundation.md)
  - 1.1 — What Sovereignty Actually Means
  - 1.2 — How to Use This Guide
  - 1.3 — The Sovereignty Stack (Overview)

### Part 2: Device & Operating System
- [Part 2: Device & Operating System](part-2-device-os.md)
  - 2.1 — Mobile: GrapheneOS on Pixel
  - 2.2 — Desktop: Linux
  - 2.3 — Getting Apps Without Google (F-Droid, Obtainium, Aurora Store)
  - 2.4 — Browser Choice *(LibreWolf · Brave · Tor Browser)*

### Part 3: Communications
- [Part 3: Communications](part-3-communications.md)
  - 3.1 — Signal and Molly
  - 3.2 — White Noise
  - 3.3 — Nostr: Your Public Identity
  - 3.4 — VoIP Numbers
  - 3.5 — Email (ProtonMail + Aliases)

### Part 4: Network
- [Part 4: Network](part-4-network.md)
  - 4.1 — DNS: The First Thing to Fix
  - 4.2 — VPN: What It Does and What It Doesn't *(Proton VPN + Mullvad)*
  - 4.3 — Home Network Hardening

### Part 5: Money
- [5.1 — Why Bitcoin](5-1-why-bitcoin.md)
- [5.2 — Getting Bitcoin Without KYC](5-2-bitcoin-no-kyc.md) *(Bisq + RoboSats)*
- [5.3 — Getting Bitcoin With KYC](5-3-bitcoin-with-kyc.md) *(River.com)*
- [5.4 — Wallets: Storing What You Own](5-4-wallets.md) *(Cake Wallet, Zeus, Cashu)*
- [5.5 — Lightning Payments](5-5-lightning.md)
- [5.6 — Other PoW Privacy Coins](5-6-privacy-coins.md) *(Monero + Litecoin MWEB)*

### Part 6: Passwords & Authentication
- [Part 6: Passwords & Authentication](part-6-passwords-authentication.md)
  - 6.1 — Password Managers *(Proton Pass · Bitwarden · KeePassXC)*
  - 6.2 — Two-Factor Authentication *(Aegis · TOTP · FIDO2)*
  - 6.3 — Email Aliases *(SimpleLogin)*
  - 6.4 — Account Security Checklist

### Part 7: The AI Agent Layer
- [Part 7: The AI Agent Layer](part-7-ai-agent-layer.md)
  - 7.1 — What NanoClaw Is
  - 7.2 — Setup: Signal + Molly + NanoClaw
  - 7.3 — Setup: White Noise + NanoClaw
  - 7.4 — How the Keys Work (Signing Daemon Architecture)
  - 7.5 — What Kind of Agent Is This?
  - 7.6 — What You Can Ask Your Agent

### Workshop Curriculum
- [Come As You Are — Privacy Workshop](workshop-come-as-you-are.md)
  - Session structure (90 min)
  - Handout: Six Things to Do This Week
  - Facilitator notes
  - Recommended opening video: "Privacy 101" by Naomi Brockwell

### Appendices
- [Appendix A — Threat Modeling Worksheet](#appendix-a) *(below)*
- [Appendix B — Minimum Viable Setup Checklist](#appendix-b) *(below)*
- [Appendix C — Glossary](#appendix-c) *(below)*
- [Appendix D — Further Reading](#appendix-d) *(below)*

---

## Appendix A — Threat Modeling Worksheet

Answer these questions before deciding which sections apply to you. There are no right or wrong answers — just honest ones.

**1. Who is most likely to seek information about you without your permission?**
- Data brokers and marketing companies (applies to almost everyone)
- An employer, landlord, or institution
- A specific individual (ex-partner, someone with a grievance)
- Law enforcement or government agencies
- Sophisticated state-level actors

**2. What information would be most harmful if exposed?**
- Your home address
- Your financial situation or habits
- Your medical or health information
- Your communications with specific people
- Your location patterns

**3. What level of effort are you willing to invest?**
- 1 hour (do the Quick Start above)
- A weekend (work through Parts 3 and 4)
- An ongoing project (work through the full guide over several months)

**4. What are you willing to trade for privacy?**
- Some convenience (can't use some mainstream apps, certain websites block VPNs)
- Some money (VPN subscription, privacy-respecting services often cost more than "free" ones that sell your data)
- Some time (initial setup is real; maintenance is minimal after)

---

## Appendix B — Minimum Viable Setup Checklist

The minimum you can do in a few hours that will meaningfully improve your privacy:

**Communications:**
- [ ] Installed Signal with privacy settings hardened
- [ ] Enabled disappearing messages on all conversations
- [ ] Have at least one contact on Signal you actually message there
- [ ] Switched primary email to ProtonMail (proton.me — free tier is fine to start)

**Network:**
- [ ] Have a VPN installed and know how to enable it
  - *Easiest start:* Proton VPN — free tier available, no credit card needed (protonvpn.com)
  - *Maximum anonymity:* Mullvad — no account required, accepts cash by mail (mullvad.net)
- [ ] Changed DNS to Quad9 (9.9.9.9) or Proton DNS (automatic when Proton VPN is active)

**Passwords & 2FA:**
- [ ] Using a password manager — Proton Pass or Bitwarden (both free, open-source)
- [ ] Two-factor authentication on your most important accounts (email, financial)
  - *Recommended 2FA app:* Aegis Authenticator (Android/GrapheneOS, FOSS, local backup)

**Money:**
- [ ] Have any amount of Bitcoin in a self-custodial wallet (not on an exchange)
- [ ] Have written down your seed phrase on paper and stored it somewhere safe

**Devices:**
- [ ] Auto-lock enabled on phone (under 1 minute)
- [ ] Full disk encryption enabled (Android: on by default; iOS: on by default; check your computer)

---

## Appendix C — Glossary

*2FA (Two-Factor Authentication):* A login system that requires two separate proofs of identity — typically your password plus a code from an app or physical key. Much harder to compromise than a password alone.

*Bitcoin:* A decentralized digital currency secured by cryptography and a distributed network. No company or government controls it.

*Cashu:* A privacy protocol for Lightning Bitcoin payments using blind signatures. Makes payments private even from the infrastructure that processes them.

*DNS (Domain Name System):* The internet's phone book. Translates domain names (like `signal.org`) into IP addresses. Your DNS queries reveal which services you use.

*E2E Encryption (End-to-End Encryption):* Messages are encrypted on the sender's device and only decryptable by the recipient. The service provider cannot read them.

*GrapheneOS:* A privacy-focused version of Android for Pixel phones. Removes Google's data collection while keeping full Android functionality.

*KYC (Know Your Customer):* Identity verification requirements (name, address, government ID) imposed by financial services and exchanges.

*Lightning Network:* A payment layer built on Bitcoin enabling instant, near-free transactions.

*Metadata:* Data about your data. Who you communicated with, when, and how often — not the content, but the pattern.

*Monero (XMR):* A privacy-focused cryptocurrency where all transactions are confidential by default.

*MWEB (MimbleWimble Extension Blocks):* Litecoin's optional privacy upgrade, enabling confidential transactions.

*Nostr:* A decentralized protocol for publishing content and messages. Your identity is a cryptographic keypair; no company controls it.

*npub:* A Nostr public key in human-readable format. Your Nostr identity's public address.

*nsec:* A Nostr private key. Never share this. Never expose it. Whoever has it can impersonate you.

*Relay:* In Nostr, a server that stores and forwards messages. Anyone can run one; no single relay is authoritative.

*Seed Phrase:* A list of 12 or 24 words that regenerates your cryptocurrency wallet. The master backup. Store it offline, never digitally.

*Self-Custody:* Holding your own Bitcoin keys rather than trusting an exchange. The foundation of Bitcoin sovereignty.

*SIM Swapping:* An attack where someone convinces your carrier to transfer your phone number to a SIM they control, enabling account takeovers on anything that uses SMS for 2FA.

*Tor:* A network that routes traffic through multiple encrypted relays, hiding the origin of requests. Used by journalists, activists, and privacy tools.

*VoIP (Voice over IP):* Phone service over the internet rather than through a cellular carrier. Can be obtained with minimal identity requirements.

*VPN (Virtual Private Network):* Encrypted tunnel between your device and a VPN server. Hides your activity from your ISP; shifts trust to the VPN provider.

*White Noise:* An encrypted messaging app using the Nostr relay network and MLS protocol. No central server.

---

## Appendix D — Further Reading

**For the physical and financial privacy layers this guide deliberately doesn't cover:**
Michael Bazzell — *Extreme Privacy* (5th Edition, 2024) — inteltechniques.com/books.html
The definitive guide to physical anonymity, alias names, private homes, and high-stakes privacy situations.

**For practical video tutorials on all topics in this guide:**
Naomi Brockwell (NBTV) — youtube.com/@NaomiBrockwellTV
Clear, well-researched video explanations of privacy tools and threats. Especially good for:
- "Encrypt Your DNS (STOP Your ISP SNOOPING!)"
- "The DARK side of VPNs"
- "How To DE-GOOGLE Your Phone! (2025)"
- "The MOST PRIVATE Messaging Apps"
- "I Have Nothing to Hide – The Dangerous Myth About Privacy"

**The Proton suite — integrated privacy services:**
proton.me — Mail, VPN, Drive, Docs, Passwords, Calendar under one account. Based in Switzerland. Free tier available across all products. Recommended starting point for most people.

*→ Proton referral link (may include bonus storage or trial): [pr.tn/ref/EY1X01YF](https://pr.tn/ref/EY1X01YF)*
*Disclosure: This is a referral link. Scott receives a small benefit. Proton earns no recommendation from us because of this — it's here because it's the best integrated privacy suite for non-technical users.*

**For ongoing privacy news and tool updates:**
Privacy Guides — privacyguides.org
RestorePrivacy — restoreprivacy.com

**For Nostr/Clawstr:**
njump.me — Nostr profile and note viewer
clawstr.com — Nostr with community channels

**For Bitcoin:**
bitcoin.org — The original Bitcoin site
learn.robosats.org — RoboSats documentation
bisq.wiki — Bisq documentation
getmonero.org — Monero documentation
litecoin.com/mweb — Litecoin MWEB documentation

wasabiwallet.io — CoinJoin for on-chain Bitcoin privacy (advanced users)

*→ River (KYC on-ramp, US-based, Bitcoin-only): [river.com/invite?r=6AI22CQ7BM](https://river.com/invite?r=6AI22CQ7BM)*
*Disclosure: Referral link. Scott receives a small benefit. River is recommended regardless — it's the best KYC starting point for most US beginners. Use river.com directly if you prefer.*

---

*This guide is a living document. If you find something outdated, unclear, or missing, report it to the maintainers.*
