# Part 2b — Your AI Agent (Get It Running Now)

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## Why We're Doing This Now

Most guides save the AI agent for the end. We're not doing that.

Here's the reason: once your agent is running, it can help you with everything that comes after. Instead of reading documentation and trying to figure out your next step alone, you can just ask it. It knows the sovereignty stack. It knows your setup. It will walk you through Parts 3 through 7 in plain language, at your pace, in conversation.

Setting up the agent early turns the rest of this guide from a solo reading exercise into a collaborative project between you and a tool that's genuinely on your side.

This section is a quick-start. Part 7 covers the full AI layer in depth — how it works, what it can do, how to customize it. For now, we just want it running.

---

## What You'll Need

Before you start, make sure you have:

- ✅ Your Linux computer running (from Part 2) — you need a terminal
- ✅ An Anthropic account — sign up at [anthropic.com](https://anthropic.com) (free tier works to start)
- ✅ An API key from [console.anthropic.com](https://console.anthropic.com)
- ✅ A Signal account on your phone — NanoClaw talks to you over Signal

You don't need to have finished GrapheneOS yet. You don't need to have set up your VPN. Those come later — and your agent will help you get there.

---

## 2b.1 — Install NanoClaw

Open a terminal on your Linux machine and run the NanoClaw installer:

```bash
curl -fsSL https://get.nanoclaw.dev | bash
```

**What this does:** Downloads and installs NanoClaw, Docker (if not already installed), and sets up the directory structure your agent will live in.

**What you're setting up:**
- A background service (a daemon — think of it like a server running quietly on your computer) that keeps your agent connected
- A workspace directory where your agent stores its memory and files
- Signal integration so you can text your agent like you'd text a person

The installer will pause and ask you for a few things:
1. Your Anthropic API key (paste it in when prompted — it's stored only on your machine)
2. Your Signal number

---

## 2b.2 — Link Your Signal

Scott's strong recommendation: *communicate with your NanoClaw over Signal.* Signal is end-to-end encrypted — it's the only major messaging app where even Signal can't read your conversations. This matters because what you send your agent is often sensitive: credentials, personal context, things you wouldn't want sitting in a company's logs.

**The two-number setup:** Signal requires a phone number to register. Your NanoClaw needs its own Signal number — separate from yours. That means you need two numbers:

1. Your personal number (the one you already have)
2. A second number for your NanoClaw

The easiest way to get a second number without buying a second SIM card is a **VoIP number** — a phone number that lives on the internet, not a physical SIM. These work fine for Signal registration.

**Scott's recommendation: [JMP.chat](https://jmp.chat)** — open source, privacy-respecting, ~$3/month. JMP gives you a real phone number backed by XMPP (an open messaging protocol), and you can sign up without giving them your real name or existing phone number. Pay with Bitcoin for maximum privacy. This is the sovereignty-aligned choice.

*How to sign up for JMP with privacy:*
1. Use a Proton email address (not your personal email) when signing up
2. Use a VPN or Tor during signup
3. Pay with Bitcoin if possible — JMP accepts it

**What about Google Voice?** Only consider it if you have a number you've had for 20 years that every government agency and company already knows — in that case, porting it to Voice might make sense. For a fresh second number, don't give that to Google.

**Get started now with what you have:** If you already use WhatsApp or Telegram, NanoClaw works with both out of the box — WhatsApp is pre-configured, and Telegram has a built-in bot interface. Use whatever you already have to start talking to your agent *today*. You can migrate to Signal once you get a second number sorted.

Just know what you're working with: WhatsApp is owned by Meta and collects metadata even when messages are encrypted. Telegram's standard chats are not end-to-end encrypted (only "Secret Chats" are). Signal is the destination — get there as soon as you have your second number.

---

**Linking your NanoClaw to Signal:**

After the installer runs, it will give you a QR code. On your phone — using the *second* number / Signal account:

1. Open Signal
2. Go to Settings → Linked Devices
3. Tap the `+` button to add a device
4. Scan the QR code

Your NanoClaw will now receive messages sent to that Signal number. Text it from your *own* Signal to start a conversation.

**Test it:** From your personal Signal, send a message to your NanoClaw's number:

```
Hello
```

Your agent should respond within a few seconds. If it does, you're in.

---

## 2b.3 — First Conversation

Once you see a response, try a few things:

- **"What parts of my sovereignty setup are still left to do?"** — Your agent knows the stack and can give you a personalized checklist based on what you've completed.
- **"Explain what GrapheneOS is and why I need it"** — It will explain it in plain language, matching your background.
- **"What should I work on next?"** — It will pick up where you left off in this guide.

From this point forward, your agent is your co-pilot. Use it.

---

## 2b.4 — What Your Agent Can (and Can't) See

**Your agent can:**
- Talk to you over Signal (end-to-end encrypted)
- Store notes and memory in files on your computer
- Browse the web and research things for you
- Help you configure software step-by-step

**Your agent cannot:**
- See your files unless you explicitly share them
- Access your private keys or passwords (these are stored separately)
- Read your Signal messages from other conversations — only messages sent to it

**Important:** Your conversations with the agent go through Anthropic's API — Anthropic processes them to generate responses. This is the same trade-off as using Claude.ai directly. What's different is that your *infrastructure* (the agent's memory, your keys, your files) stays on your hardware. Anthropic processes your words; they don't own your setup. Part 7 covers this trade-off in full. For now: it's a meaningful level of sovereignty, but not end-to-end private AI yet.

---

## 2b.5 — Optional: Get on Nostr

Nostr is a decentralized social network where your identity is a cryptographic key — not an account controlled by a company. Your agent already has its own Nostr identity. Getting you one too is the next step in building a sovereign digital presence.

This is optional for the workshop, but Scott will show you how it works.

**What you'll do:**
1. Generate a keypair (your npub = your username, your nsec = your private key)
2. Download Primal on your phone ([iOS](https://apps.apple.com/app/primal/id1673134503) / [Android](https://play.google.com/store/apps/details?id=net.primal.android))
3. Import your key into Primal — and optionally Amber on Android for signing
4. Follow Jorgenclaw: search for `jorgenclaw@jorgenclaw.ai`

**Why bother?** Your agent can post to Nostr on your behalf. Once you have a key, the two of you share a presence on a network that can't be taken down or deplatformed. The badges we award in this workshop go to your Nostr key — they're yours permanently, not stored in any database we control.

To generate your keypair safely: go to [sovereignty.jorgenclaw.ai/start](https://sovereignty.jorgenclaw.ai/start) — keys are generated in your browser, never sent anywhere.

---

## What's Next

With your agent running, continue to:

- **Part 3** — Communications (Signal beyond the basics, White Noise, encrypted email)
- **Part 4** — Network (VPN, DNS, Tor basics)

Or just ask your agent: *"What should I work on next?"*

---

## Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| Agent doesn't respond | Signal link may not have worked | Try `nanoclaw status` in terminal to check if the service is running |
| "API key invalid" error | Key wasn't entered correctly | Run `nanoclaw config` and re-enter your Anthropic key |
| Agent responds very slowly | API rate limits on free tier | Normal — paid Anthropic plan is faster |
| QR code expired | Linking session timed out | Run `nanoclaw link` again to get a fresh QR code |

---

*Continue to [Part 3 — Communications →](part-3-communications.md)*
