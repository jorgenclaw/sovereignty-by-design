# Part 2b — Your AI Agent (Get It Running Now)

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## Why We're Doing This Now

Most guides save the AI agent for the end. We're not doing that.

Here's the reason: once your agent is running, it can help you with everything that comes after. Instead of reading documentation and trying to figure out your next step alone, you can just ask it. It will walk you through Parts 3 through 7 in plain language, at your pace, in conversation.

Setting up the agent early turns the rest of this guide from a solo reading exercise into a collaborative project between you and a tool that's genuinely on your side.

This section is a quick-start. Part 7 covers the full AI layer in depth — how it works, what it can do, how to customize it. For now, we just want it running.

*Updated October 2026 for NanoClaw v2.*

---

## What You'll Need

Before you start, make sure you have:

- ✅ Your Linux computer running (from Part 2) — you need a terminal. macOS works too; Windows needs WSL2.
- ✅ An AI subscription in your own name. NanoClaw is most cost-efficient on a flat monthly plan: a *Claude* plan (the default) or a *ChatGPT* plan. Plans start around $20/month. An API key also works, but you pay per use.
- ✅ *Claude Code*, Anthropic's command-line assistant — NanoClaw uses it for setup, fixes and customization. Install it from Anthropic's official instructions (check you're on claude.com or docs.claude.com).
- ✅ A messaging app to talk to your agent from your phone: Signal (most private — see 2b.2), Telegram, WhatsApp or Discord.

You don't need to have finished GrapheneOS yet. You don't need to have set up your VPN. Those come later — and your agent will help you get there.

---

## 2b.1 — Install NanoClaw

Open a terminal and run:

```bash
git clone https://github.com/nanocoai/nanoclaw.git
cd nanoclaw
bash nanoclaw.sh
```

*Optional but recommended:* before running the installer, check out the latest release tag shown on the project's GitHub releases page (`git checkout <tag>`), so you know exactly which version you're running.

**What the installer does, in order:**
1. Installs Node.js and pnpm if they're missing.
2. Checks for Docker and installs it if needed (you may need to log out and back in once).
3. Builds the agent's container — the sandbox your agent lives in. Takes a few minutes.
4. Signs in to your AI account. For Claude, a browser opens and you sign in to your own account; the resulting credential is stored in a local vault (OneCLI). Your agent never sees the raw key.
5. Connects your first messaging app and asks you to name your agent.

If a step fails, the installer calls Claude Code to diagnose the problem and pick up where it left off. Logs are in `logs/setup.log`.

**What you end up with:**
- A background service that keeps your agent connected
- A workspace on your computer where your agent stores its memory and files
- A messaging connection so you can text your agent like you'd text a person

---

## 2b.2 — Talk to It Over Signal

Scott's strong recommendation: *communicate with your NanoClaw over Signal.* Signal is end-to-end encrypted — even Signal can't read your conversations. This matters because what you send your agent is often personal.

NanoClaw connects to Signal as a *linked device* (the same way Signal Desktop does). Your agent sends and receives as whichever Signal account links it. To give your agent its own identity, separate from yours, it needs **its own phone number**.

**The two-number setup:**

1. Your personal number stays in your normal Signal app.
2. Get a second number for your agent. The easiest way without a second SIM card is a **VoIP number** — a phone number that lives on the internet.
3. On your phone, install **Molly** (a Signal app that can run alongside regular Signal) and register the agent's number there.

**Scott's recommendation for the second number: [JMP.chat](https://jmp.chat)** — open source, privacy-respecting, about $4.99/month. You can sign up without giving them your real name or existing phone number.

*How to sign up for JMP with privacy:*
1. Use a Proton email address (not your personal email) when signing up
2. Use a VPN or Tor during signup
3. Pay with Bitcoin if possible

**What about Google Voice?** Only consider it if you have a number you've had for 20 years that every government agency and company already knows — in that case, porting it to Voice might make sense. For a fresh second number, don't give that to Google.

**Linking your NanoClaw to Signal:**

1. Open Claude Code in your NanoClaw folder and run `/add-signal`.
2. A QR code appears. On your phone, open **Molly** (the agent's number) → Settings → Linked Devices → Link New Device → scan it. The code expires after about 3 minutes; run the step again if needed.
3. From your *personal* Signal, send your agent's number a message:

```
Hello
```

Your agent should respond within a few seconds. If it does, you're in.

> If you link from your *personal* Signal instead, the agent will speak as you. Use the second number to keep you and your agent separate.

**Get started now with what you have:** If you already use Telegram or WhatsApp, you can start there and move to Signal once your second number is sorted. In Claude Code, run `/add-telegram` or `/add-whatsapp`. Just know what you're working with: WhatsApp is owned by Meta and collects metadata even when messages are encrypted. Telegram's standard chats are not end-to-end encrypted (only "Secret Chats" are). Signal is the destination.

---

## 2b.3 — First Conversation

Once you see a response, try a few things:

- **"What parts of my sovereignty setup are still left to do?"**
- **"Explain what GrapheneOS is and why I'd want it"** — it will explain in plain language, matching your background.
- **"Remind me in 10 minutes to stretch."** — checks that scheduled messages work.

From this point forward, your agent is your co-pilot. Use it.

---

## 2b.4 — What Your Agent Can (and Can't) See

**Your agent can:**
- Talk to you over your messaging app
- Store notes and memory in files on your computer
- Browse the web and research things for you
- Help you configure software step-by-step

**Your agent cannot:**
- See files outside the folders you've given it — it runs in a sandboxed container
- Hold your raw API keys — credentials stay in a local vault and are added to requests for it
- Read your other conversations — only messages sent to it

**Important:** With a cloud AI service, your conversations with the agent go to that company (Anthropic for Claude, OpenAI for ChatGPT) to generate responses — along with anything the agent reads in order to answer you. This is the same trade-off as using Claude.ai or ChatGPT directly. What's different is that your *infrastructure* — the agent's memory, your files, your identity — stays on your hardware. For more private processing, NanoClaw can switch to a privacy-focused service like Venice, or to a model running on your own computer through Ollama. Part 7 covers this. For now: it's a meaningful level of sovereignty, but not fully private AI yet.

---

## 2b.5 — Optional: Get on Nostr

Nostr is a decentralized social network where your identity is a cryptographic key — not an account controlled by a company.

**What you'll do:**
1. Download Primal on your phone ([iOS](https://apps.apple.com/app/primal/id1673134503) / [Android](https://play.google.com/store/apps/details?id=net.primal.android)) and create an account — it generates your keypair (your npub = your public name, your nsec = your private key)
2. Back up your nsec offline, the same way you'd back up a wallet's recovery words. Optionally keep it in Amber on Android for signing.
3. Follow Jorgenclaw, Scott's agent: search for `jorgenclaw@jorgenclaw.ai`

**Why bother?** Your Nostr identity can't be taken down or deplatformed. Sovereignty Badges (sovereignty.jorgenclaw.ai/app) are issued to your Nostr key — they're yours permanently, not stored in any database we control.

> Giving your *agent* its own Nostr identity is an advanced project — see Part 7 for how Jorgenclaw does it without ever letting the agent see its private key.

---

## What's Next

With your agent running, continue to:

- **Part 3** — Communications (Signal beyond the basics, White Noise, encrypted email)
- **Part 4** — Network (VPN, DNS, home network)

Or just ask your agent: *"What should I work on next?"*

---

## Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| Agent doesn't respond | The service may have stopped, or the link didn't finish | Ask Claude Code in your NanoClaw folder: "my agent isn't responding — check it" (or run `/debug`). Logs: `logs/nanoclaw.error.log` |
| AI sign-in failed or expired | The AI credential needs refreshing | Ask Claude Code to re-register your AI credential |
| QR code expired | The Signal link timed out (about 3 minutes) | Run `/add-signal` again for a fresh code |
| Setup stopped partway | One install step failed | Run `bash nanoclaw.sh` again — it picks up where it left off, and calls Claude Code to diagnose |

---

*Continue to [Part 3 — Communications →](part-3-communications.md)*
