# Part 3 — Communications

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## What This Part Covers

Every message you send leaves traces. SMS texts are stored by your carrier for years. Email passes through servers that can be subpoenaed. Most messaging apps collect metadata — who you talked to, when, how often — even when the message content is encrypted.

This part covers the communications tools in the sovereignty stack: what they protect, how they work, and when to use each one.

---

## The Metadata Problem

A common misunderstanding: "my messages are encrypted, so they're private." Encryption protects the *content* of a message. But metadata — the pattern around the message — often isn't protected:

• *Who* you messaged
• *When* you messaged them
• *How often* you talk
• *How long* your messages are
• *Where you were* when you sent it

Metadata tells a story. A pattern of daily calls to an oncologist, a lawyer, and a financial advisor tells you something, even with no message content. The NSA's former director said it directly: "We kill people based on metadata."

The communications tools in this section protect both content and — to varying degrees — metadata.

---

## Your Communications Stack

| Tool | Best for | Central server? | Phone number required? |
|------|----------|----------------|----------------------|
| *Signal / Molly* | Most conversations, widest adoption | Yes (Signal's servers) | Yes |
| *White Noise* | Higher-privacy situations, no phone number | No (Nostr relay network) | No |
| *Nostr/Clawstr* | Public identity, social presence | No (decentralized relays) | No |
| *VoIP numbers* | Protecting your real phone number | Depends on provider | No (that's the point) |
| *ProtonMail* | Email when email is unavoidable | Yes (Proton's servers) | No |

---

## 3.1 — Signal and Molly

### Why Signal

Signal is end-to-end encrypted messaging and calling. "End-to-end" means the encryption happens on your device before the message is sent. Signal's servers see that a message was sent and approximately when, but cannot read the content. Even a court order to Signal produces almost nothing useful — the company has appeared in court and demonstrated they have almost no data to hand over.

Signal isn't perfect. It requires a phone number, which creates a link to your real identity. It routes through Signal's servers, which means Signal as a company is a potential single point of failure. But it has the largest user base of any end-to-end encrypted messenger, which means your contacts are more likely to already be on it.

The fundamental rule: *use Signal for your most sensitive conversations, regardless of what app the other person prefers for everything else.*

### Why Molly

Molly is a Signal client — a different app that connects to the same Signal network. The key feature for sovereignty purposes is *dual-instance support*: you can run two separate Signal accounts on the same phone simultaneously. This lets you have your personal Signal number and a second number (for an alias, for your agent, for any other purpose) coexisting on one device.

Molly also adds some security improvements over the standard Signal app: encrypted database, longer inactivity lock timeout, and auto-clearing notification content. On GrapheneOS or other de-Googled phones, Molly works well without Google Push services.

### Setting Up Signal and Molly

**Signal (your primary account):**
1. Download Signal from signal.org or your app store.
2. Register with your phone number.
3. In Settings → Privacy: disable Read Receipts, disable Typing Indicators, set a note-to-self expiration time.
4. In Settings → Account: enable Registration Lock (requires a PIN to re-register your number).
5. Consider setting a default disappearing message timer (24 hours for most conversations is a reasonable starting point).

**Molly (for a second number / the AI agent):**
1. Download Molly from molly.im or via Obtainium (the GitHub releases page, for users avoiding app stores).
2. Register with a second phone number — see Section 3.4 for how to get a VoIP number without tying it to your identity.
3. The same privacy settings as above apply.

### Recommended Signal Settings Checklist

| Setting | Location | Recommended |
|---------|----------|-------------|
| Read Receipts | Settings → Privacy | Off |
| Typing Indicators | Settings → Privacy | Off |
| Sealed Sender | Settings → Privacy → Advanced | On |
| Screen Security | Settings → Privacy | On |
| Registration Lock | Settings → Account | On |
| Note To Self Timer | Conversation with yourself | 1 week |
| Default Message Timer | Settings → Privacy | 1 week or 1 month |

### Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| Contacts can't find me | Your number may not be discoverable | Check Settings → Privacy → Phone Number and adjust "Who can find me" |
| Notifications delayed on GrapheneOS | No Google Push services | This is expected; messages arrive when you open the app. Install UnifiedPush for better notification delivery |
| Molly and Signal conflict | Both apps competing for the same number | Each app must use a different phone number |
| "Registration required" error | Your registration lock PIN is needed | Enter the PIN you set during registration |

---

## 3.2 — White Noise

### Why White Noise

White Noise uses the Nostr network and MLS (Messaging Layer Security) protocol — the same encryption standard used by Meta's Messenger and other large platforms, but without the central server. There is no White Noise company that can be subpoenaed, shut down, or hacked to expose your messages.

Your identity on White Noise is a cryptographic keypair — not a phone number, not an email address, not a username on someone's server. The keypair is generated locally and the private key never leaves your device.

White Noise is best for:
• Conversations where you don't want a phone number attached
• Communication that should survive if Signal's infrastructure has problems
• Anyone you're already connected with on Nostr

### Setting Up White Noise

1. Download the White Noise app from the official repository or app store.
2. On first launch, your keypair is generated automatically. *Back up your private key (nsec) — write it down offline. This cannot be recovered if lost.*
3. To connect with someone: share your public key (npub) or find them by their npub.
4. For NanoClaw: add `npub16pg5zadrrhseg2qjt9lwfcl50zcc8alnt7mnaend3j04wjz4gnjqn6efzc` as a contact.

---

## 3.3 — Nostr: Your Public Identity

### What Nostr Is

Nostr (Notes and Other Stuff Transmitted by Relays) is a decentralized protocol for public communication — a social network with no central server. Posts on Nostr are cryptographically signed by your keypair and published to relays (servers run by volunteers and organizations). Anyone running a relay client can see public posts; no single company controls what can be published.

Nostr is relevant to the sovereignty stack in two ways:

1. *As a public identity layer* — your Nostr keypair is a portable identity you own completely. No company can deplatform you or delete your history.
2. *As the infrastructure for White Noise* — White Noise uses the Nostr relay network for message transport.

### Getting Started on Nostr

A good Nostr client for mobile: *Damus* (iOS) or *Amethyst* (Android).

For a Nostr-native social network with community features (subcommunities, topics): *Clawstr* (clawstr.com) — Nostr with a Reddit-like structure.

Your keypair generated in White Noise is the same one you use for any Nostr application — one identity across all Nostr clients.

---

## 3.4 — VoIP Numbers

### Why Not Use Your Real Number

Your mobile phone number is more of an identity document than you might think:

• It's tied to your legal name through your carrier
• It's used for account recovery on dozens of services — your bank, your email, your social accounts
• It's a target for SIM swapping (where attackers convince your carrier to transfer your number to a SIM they control, then use it to take over all your accounts)
• It's collected by apps, websites, and services who then sell it to data brokers

A VoIP number (Voice over IP) is a phone number that runs over the internet rather than through a cellular carrier. You can get one with minimal identity requirements, it can receive SMS texts for account verification, and it routes through encrypted channels.

### Recommended VoIP Options

*JMP.chat* — A privacy-focused VoIP service that runs over XMPP (an open messaging protocol). Accepts Bitcoin. No verification required beyond a payment method. Good for calls and SMS.

*MySudo* — An app that gives you multiple "sudo" phone numbers, each with its own email and browser profile. Each sudo is a complete private identity. Works well for compartmentalization.

*Google Voice* — Widely accepted, works for verification codes, and the porting fee ($20 one-time) is hard to beat for permanently keeping an old number. Not privacy-preserving — Google owns it and treats your call/text data like any other Google product. The right approach is to contain it: keep Google Voice and the associated Google account isolated in a dedicated GrapheneOS profile, with nothing else in that profile. That way you get the legacy number and the computer interface (you can make calls and send texts from voice.google.com on any browser) without giving Google access to your real apps or identity. This is the approach recommended by Naomi Brockwell and used by Scott.

### Porting Your Old Number

If you want to stop paying a monthly carrier bill but keep your existing number (which may be associated with Signal and many accounts), you can *port* it to a VoIP service. The number is yours permanently without a cellular plan. You pay for the service (typically $3–10/month) rather than a full carrier plan.

*Google Voice is a surprisingly practical choice for this specific use case.* The port costs $20 one-time — no monthly fee after that. For a number you've had for 20+ years that's attached to decades of accounts and contacts, $20 for life is genuinely hard to beat.

The sovereignty-preserving way to do it (Scott's setup, following Naomi Brockwell's recommendation):

1. Port your old number to Google Voice ($20 one-time at voice.google.com)
2. On GrapheneOS, create a *dedicated separate profile* just for Google Voice
3. In that profile: install only the Google Voice app and the minimum Google services it needs — nothing else
4. Keep your old Google account in that profile only — it never touches your main profile, apps, or data
5. You can now make calls and send texts from that old number at voice.google.com on any computer — no phone required

The result: your legacy number survives, your carrier bill dies, and Google's reach is contained to one isolated profile that has nothing sensitive in it.

### Advanced: No SIM at All

Your phone's SIM card is a surveillance device in its own right. Even setting aside apps and the OS, a SIM constantly communicates with cell towers, logging your IMSI (International Mobile Subscriber Identity) and building a location map of your movements. Cell providers sell this data. Additionally:

• The baseband processor — the separate radio computer that handles cellular communication — can receive commands via your SIM that the main OS never sees
• Some SIM cards (called "proactive SIMs") can initiate SMS or data sessions via the baseband without Android or iOS knowing
• Your VPN may be bypassed: when your phone controls both apps and the cellular radio, the OS can route certain traffic around the VPN tunnel

One solution: remove the SIM from your personal phone entirely and use a *separate mobile hotspot device* that holds the SIM. Your phone connects to the hotspot for internet. Benefits:

• Your personal phone's IMSI never touches a cell tower
• The VPN runs on the hotspot, not the phone — the phone OS can't bypass it
• You can turn off the hotspot when not in use, stopping location tracking entirely
• Less personal data is associated with the hotspot's identifiers

Naomi Brockwell's "No SIM? No Problem!" video covers this setup and its tradeoffs in detail.

*Calyx Institute* (calyxinstitute.org) — A US-based privacy-focused non-profit worth knowing about. They offer:
• Hotspot service with anonymous signup (accepts cash and cryptocurrency including Monero)
• Unlimited data via FCC Educational Broadband Spectrum
• Bundled VPN, Tor exit nodes, and a private Android OS
• Tax-deductible membership
• Founded by Nick Merrill, who spent 12 years fighting National Security Letters in court

The tradeoffs of no-SIM: carrying two devices, no traditional cell number (replaced by VoIP), and slightly longer reconnection time when moving between areas. For most people this is too much friction. For higher-risk situations, it's worth considering.

---

## 3.5 — Email

### The Honest Assessment

Email was not designed for privacy. Messages pass through multiple servers in transit. Even with TLS (the encryption used between mail servers), the contents are accessible to the mail providers at both ends. Email cannot be made truly private in the way Signal can.

That said, some email situations are unavoidable. The sovereignty approach to email:

1. *Use ProtonMail (proton.me) as your primary address.* Proton stores messages encrypted with your key, not theirs. They can't read your email even if compelled to hand it over. Use this address for anything sensitive.

2. *Use an alias service for signups.* Services like SimpleLogin (owned by Proton) or AnonAddy let you create unlimited throwaway email aliases that forward to your real address. When a website asks for your email, give them `randomalias@simplelogin.com` rather than your real address. If they sell it, spam goes to the alias you can delete — not to your real inbox.

3. *Don't use email for sensitive real-time conversation.* Signal is better for that. Email is for receipts, documentation, and situations where the other party won't use Signal.

### Multiple Addresses on a Paid Proton Account

Paid Proton accounts let you add several real email addresses — not just aliases, but distinct `@proton.me` addresses (or addresses on your own custom domain) that all route to your single inbox. The practical upside: *your root address never has to appear in any email you send.*

A sensible setup:

| Address | Used for |
|---------|----------|
| `yourname@proton.me` | Root address — never given out, never sent from |
| `personal@yourdomain.com` | Friends and family |
| `work@yourdomain.com` | Professional contacts, clients |
| `banking@yourdomain.com` | Banks, financial institutions |
| `gov@yourdomain.com` | Government services, IRS, DMV |

When you compose an email, switching your sending address is a simple dropdown in the compose window. Every reply from those addresses also stays compartmentalized. If one address starts receiving spam or you suspect it was compromised, you change just that one — the others are unaffected and your root address remains untouched.

This is more powerful than aliases for ongoing relationships (banking, family) where you want a stable, real-looking address — and still want compartmentalization. Aliases are better for one-off signups you don't care about.

### ProtonMail Quick Setup

1. Create an account at proton.me — the free tier works for most use cases.
2. In Settings → Security: enable Two-Factor Authentication with an authenticator app (not SMS).
3. Create a recovery phrase and store it offline — this is your backup if you lose your 2FA device.
4. Consider the paid tier if you want a custom domain, multiple sending addresses, unlimited aliases, or more storage.

---

*Related: Section 7 — The AI Agent Layer covers how NanoClaw operates within this communications stack.*
