# Part 7 — The AI Agent Layer

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## 7.0 — Do You Need a Paid Proton Account?

Short answer: **you don't — to get started.**

A paid Proton Unlimited account is what *Scott uses* to run Jorgenclaw's infrastructure (email, calendar, drive, pass). That's an operator cost — the cost of running the service.

If you're building your own personal assistant, you can start with a **free Proton account** and supplement it with open-source tools:

| Need | Free option |
|------|------------|
| Email | Proton Mail free tier (1GB, proton.me address) |
| Password manager | KeePassXC (local) or Bitwarden free tier |
| Calendar | Radicale (self-hosted, see Section 7.5) |
| Calendar on phone | DAVx5 + Etar (both free on F-Droid) |
| AI agent | NanoClaw (free, open source) — powered by an AI plan |

**Total recurring cost: the AI plan.** NanoClaw itself is free, but its thinking comes from an AI service: a Claude or ChatGPT subscription (from about $20/month), Venice, or a model running on your own graphics card (no monthly cost, but the hardware costs more). Everything else in this table is free.

When to upgrade to paid Proton:
- You want a custom email domain (e.g., you@yourdomain.com)
- You need more than 1GB of mail storage
- You want Proton Drive for full file sync
- You want Proton VPN on multiple devices

The upgrade path is clear — but you don't have to be on it on day one.

---

## 7.1 — What NanoClaw Is

### The Short Version

NanoClaw is an AI assistant that lives inside a sovereignty stack instead of on a tech company's server. You talk to it from your phone over a messaging app — Signal (end-to-end encrypted) is the recommended choice, and Telegram, WhatsApp, Discord and others are available. It has memory, schedules tasks and browses the web, and with add-ons it can do much more (Scott's agent, Jorgenclaw, posts to Nostr — see 7.9).

It's not ChatGPT. The difference isn't just technical — it's structural.

### What Makes It Different

When you ask ChatGPT or Google Gemini a question, three things happen that you may not think about:

1. Your message travels to a company's server. That company logs it.
2. The company can read your conversation. Courts can subpoena it. Employees can access it. Data breaches can expose it.
3. The AI runs on that company's infrastructure. If they shut down, change their terms, or decide your questions violate their policies, your assistant disappears.

NanoClaw runs differently. The AI model is still usually accessed over the internet (Claude by default; ChatGPT and the privacy-focused Venice are options), but the *system around it* — how messages travel, how memory is stored, how secrets are handled — is all under your control. You can also run the model on your own graphics card through Ollama, so nothing leaves your home.

Your conversations arrive over your messaging app (with Signal, end-to-end encrypted to your computer — then the AI service processes the content). The AI's memory files sit on your own hardware. The private keys used to sign the agent's Nostr posts never leave your machine. The agent is yours: you can move it, modify it, or shut it down.

This isn't about hiding things from Anthropic specifically. It's about not building your daily digital life on infrastructure you don't control.

### What NanoClaw Can Do

• *Answer questions and research topics* — web browsing, reading documents, summarizing content
• *Manage your schedule* — set reminders, recurring tasks, check-ins
• *Keep memory across conversations* — it remembers what you've told it: your preferences, ongoing projects, people in your life
• *Send and receive messages* — in direct chats and groups on the messaging apps you connect
• *Grow with add-ons* — for example, Scott's agent Jorgenclaw has its own Nostr identity and posts autonomously (see 7.9)
• *Help with writing and documents* — drafts, research, formatting, editing

### What It Can't Do (And Why That's Good)

NanoClaw operates inside a security model designed to limit its blast radius:

• It cannot send Bitcoin transactions or access wallets without explicit spending controls you set
• It cannot read private keys or sensitive credentials stored outside its workspace
• It runs in a container with limited filesystem access — it cannot reach into your broader system
• A cloud AI service sees the conversation and anything the agent reads in order to answer you — but not your keys, and not files the agent was never given

The agent is capable and useful. But the sensitive parts of your life — your keys, your money, your identity documents — stay in your hands.

---

## 7.2 — Setup: Signal + Molly + NanoClaw

### What You're Setting Up

Signal is your personal encrypted messaging app — the one connected to your real number, with your real contacts. *You use Signal.*

Molly is a separate app that connects to the *same Signal network* but runs a *completely separate account*. You register your agent's own phone number (typically a VoIP number — see Section 3.4) in Molly. NanoClaw itself runs on your computer and connects to that Molly account as a *linked device* (the same way Signal Desktop does), so the agent sends and receives as that number.

Both apps coexist on the same phone. This is Molly's key feature here: two Signal accounts on one device, each with its own number. (Scott uses Molly for business conversations and Signal for personal ones — both end-to-end encrypted.)

The flow looks like this:

```
Your phone (your Signal app) ─────┐
                                   ├─→ Signal network (E2E encrypted) ─→ NanoClaw ─→ Claude AI
Your phone (NanoClaw's Molly) ────┘
```

Your messages are encrypted all the way to your computer. NanoClaw decrypts them there, sends the text to the AI service to generate a reply, and responds through Signal's encryption.

### What You Need

• Your phone running both Signal (your account) and Molly (NanoClaw's account)
• Your agent's own Signal number (the second number you registered in Molly, linked with `/add-signal` — see Part 2b)
• A trigger word for group chats — your agent's name with an @, chosen during setup (Scott's is `@Jorgenclaw`); in a 1-on-1 conversation with the agent, no trigger is needed

### Starting a Conversation

1. Open your Signal app and start a conversation with the NanoClaw contact (or a group where it's added).
2. In a 1-on-1 chat: type your message normally. No trigger word needed.
3. In a group chat: start your message with your agent's trigger (for example `@Jorgenclaw`) to address it.
4. Send it. The agent typically responds within 15–30 seconds for simple questions; longer for tasks that require web browsing or file work.

### Opening NanoClaw to Friends and Family

The Molly/dual-instance setup makes it easy to add friends and family to a Signal group that includes your NanoClaw agent. Anyone in the group can interact with it.

One thing to be clear about before you do this: *you, as the NanoClaw operator, can read all messages in that group.* Not because Signal doesn't encrypt them — it does. But because your device is a group member, your Signal app receives and decrypts all messages automatically. That's how group chats work.

This isn't a gotcha — it's just something worth naming explicitly with your friends and family before they start chatting with the agent. A simple: "By the way, I can see everything in this group — it's my assistant, my server" is all it takes. People appreciate the transparency, and it's the right thing to do.

This is true of every encrypted group, on Signal or White Noise alike: each member's device — including the one running your agent — decrypts every message in the group.

### Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| No response after a minute | The agent may not have received your message, or the server is restarting | Wait 2 minutes and try again |
| Response doesn't remember previous conversations | Memory files may not have loaded | Ask "What do you remember about me?" — if blank, memory may need to be rebuilt |
| Agent responds to everyone in a group, not just you | Group configured without a trigger word | Check with the person managing the NanoClaw server |
| You're not sure if your message was received | Agent reacts with 👍 to confirm receipt on long tasks | If no reaction after 30 seconds, something may be wrong |

---

## 7.3 — Choosing the AI Behind Your Agent

NanoClaw is the agent — the memory, the scheduling, the messaging. The *thinking* comes from an AI model, and you choose which one. Every option below uses the same NanoClaw agent; only the "brain" changes.

| Option | What it costs | Who sees your conversations | Good for |
|--------|---------------|-----------------------------|----------|
| *Claude* (default) | A Claude subscription, from about $20/month | Anthropic | The most capable starting point; NanoClaw is built around it |
| *ChatGPT* | A ChatGPT subscription (installed with `/add-codex`) | OpenAI | People who already pay for ChatGPT |
| *Venice* | Pay per use through its API (its $18/month Pro plan includes only a small API allowance) | Venice, which says it doesn't store conversations | More private cloud processing without new hardware |
| *Local model via [Ollama](https://ollama.com)* | No monthly fee, but needs a graphics card with plenty of memory (24GB+ for a capable model) | No one — it runs on your computer | Full privacy |

NanoClaw runs most cost-efficiently on a flat monthly subscription rather than paying per request. A sensible path for a family that wants privacy without new hardware: keep a Claude plan for everyday work, add Venice for conversations you'd rather keep private, and switch between them.

**Switching is a plain-English request.** Open Claude Code (or OpenCode) in your NanoClaw folder and say, for example, *"switch my agent to Venice"* — then *"switch back to Claude"* when you want. Behind the scenes, Venice runs through NanoClaw's OpenCode provider (`/add-opencode`) and local models through `/add-ollama-provider`; Claude Code handles the details.

## 7.4 — What You Can Ask Your Agent

### The Practical Picture

NanoClaw is most useful as a persistent, context-aware assistant that builds up knowledge about you over time. This is meaningfully different from a stateless chatbot that forgets you every session.

### Research and Web Browsing

"Find recent articles about [topic] and summarize the key points."
"What's the current status of [project/legislation/news]?"
"Look at this URL and tell me what it says."
"Search for [product] reviews and give me the pros and cons."

The agent can browse the web, read articles, and extract information — not just recall what it knew at training time.

### Scheduling and Reminders

"Remind me to call my accountant on Friday at 10am."
"Set a weekly reminder every Monday morning to review my to-do list."
"Check in with me tomorrow and ask if I finished [task]."

Reminders run even when you're not in conversation. The agent schedules them and sends you a message when they fire.

### Memory and Continuity

The agent maintains memory files across sessions. It remembers:
- People in your life and how you've described them
- Ongoing projects and their status
- Preferences you've stated
- Things you've asked it to track or follow up on

You can ask: "What have I told you about [topic]?" or "What's the status of [project]?" and it will check its memory.

### Writing and Documents

"Draft a message to [person] about [topic] in a professional tone."
"Review this document and flag anything unclear."
"Rewrite this paragraph to be simpler."
"Create a summary of [long document] I can share with someone."

### What to Be Thoughtful About

*Don't share secrets through the channel.* Even though Signal and White Noise are encrypted in transit, the AI model itself processes the content. Don't send private keys, passwords, or sensitive credentials through the agent chat.

*The agent is persistent but not infallible.* It can make mistakes, misremember things, or get confused on complex multi-step tasks. Treat it as a very capable assistant, not an oracle.

*You can correct it.* If the agent gets something wrong or remembers something incorrectly, just tell it. "That's not right — here's the correct version." It will update its understanding.

### Getting the Most Out of It

The agent improves the more context you give it over time. Good habits:
• "Remember this for future reference: [fact/preference/decision]"
• Brief it at the start of new projects rather than explaining from scratch each time
• If a response misses the mark, say why — it adjusts to your expectations

## 7.5 — Sovereignty-Native Calendar (Radicale + DAVx5)

Proton Calendar is excellent (included in the free Proton plan, with limits), but it's end-to-end encrypted in a way that prevents your AI agent from writing to it directly. For a $0 setup that lets your agent manage your calendar, use **Radicale**.

### What Radicale Is

Radicale is a tiny CalDAV server that runs on your own machine. It stores calendar events as plain files on your hardware. Your AI agent can write events to it, and you can read them on your phone.

It's the open-source equivalent of having a calendar app that only talks to your own server.

### Setup Overview

1. **On your server/mini PC:** Install and run Radicale (one command)
2. **On your phone:** Install DAVx5 (F-Droid) and Etar (F-Droid)
3. **Connect DAVx5** to your Radicale server URL with your username and password
4. **Open Etar** — your calendar appears, populated by whatever your agent wrote

### Privacy Boundary: Personal vs. Shared Calendar

This is important: **your AI agent should not have access to your personal calendar.**

The right model:
- **Your personal Proton Calendar** — end-to-end encrypted, agent-inaccessible, just yours
- **Your Radicale shared calendar** — agent can write to this (reminders, events, tasks), you explicitly control what goes in

You decide what the agent can see. That's what sovereignty means in practice.

Your agent writes an event to the Radicale calendar. You see it in Etar. Your personal appointments in Proton Calendar stay private. Both exist on your phone side by side — the user controls the boundary.

### Tools

| Tool | Platform | Source | Purpose |
|------|----------|--------|---------|
| Radicale | Server (Linux/Mac/Windows) | radicale.org | CalDAV server |
| DAVx5 | Android | F-Droid | Syncs Radicale to phone |
| Etar | Android | F-Droid | Calendar app (displays synced events) |

---

## 7.6 — Which Track Are You?

Before setting up NanoClaw, it helps to know what you're trying to do. There are three tracks:

**Personal** — You want an AI that helps you with your own life: reminders, email drafts, research, memory. You're not building anything.

**Business** — You want an AI that helps run your business: customer messages, scheduling, Bitcoin payments, data sovereignty.

**Builder** — You want to understand how it works and customize it. You're technical or want to become technical.

Most people are Personal. Some are Personal + Business. Builders are rare — but if that's you, the whole codebase is open.

---

# Part 7B — How Jorgenclaw Does It (Copy What You Like)

Everything above is what NanoClaw does out of the box. This half is different: it's how Scott has built up *Jorgenclaw*, his own NanoClaw agent, over months of daily use. None of it is required. But your agent can copy any of it — describe the piece you want and ask your agent (or Claude Code in your NanoClaw folder) to build its own version.

## 7.7 — Scott's Chat Setup

Scott talks to his agents in two places:

- **Main chat: Element (Matrix), running a local open model.** Day-to-day conversations go to Jorgenclaw over Matrix, and the AI model runs on Scott's own computer through Ollama — so these conversations never leave the house.
- **Secondary chat: a group in Molly, running on Claude.** A separate agent group (its own workspace) for work where a frontier cloud model helps. Even as a separate workspace, it comes to know what you're about if you use it often enough.

Molly also keeps business and personal apart: Scott uses Molly for business Signal conversations and the regular Signal app for personal ones — both end-to-end encrypted.

**To copy it:** run two agent groups — one on a local model for private, everyday use, and one on a cloud model for heavier work — and give each its own chat.

## 7.8 — White Noise (an Add-On Scott Built)

### What White Noise Adds

> *White Noise isn't one of NanoClaw's standard channels — Scott added it to his own setup. Treat this section as an example of what's possible.*

White Noise uses the Nostr network and MLS (Messaging Layer Security) — an IETF standard for encrypted group messaging (RFC 9420) — but without a central server. There is no White Noise company that can be subpoenaed, shut down, or hacked to expose your messages.

Your identity on White Noise is a cryptographic keypair — not a phone number, not an email address, not a username on someone's server. The keypair is generated locally and the private key never leaves your device.

White Noise is best for:
• Conversations where you don't want a phone number attached
• Communication that should survive if Signal's infrastructure has problems
• Anyone you're already connected with on Nostr

### How to Access NanoClaw via White Noise

1. Install the White Noise app (see Section 3.2 for full setup).
2. Add *your agent's* Nostr public key (npub) as a contact.
3. Start a direct message, or create a group that includes your agent.
4. Messages work the same as Signal — type naturally in a 1-on-1, use your agent's trigger in groups.

### Signal vs. White Noise: Which to Use

| Consideration | Signal / Molly | White Noise |
|---------------|---------------|-------------|
| Setup difficulty | Easier | Moderate |
| Infrastructure dependence | Signal's servers | Nostr relay network (distributed) |
| Phone number required | Yes | No |
| Adoption in your contacts | Higher | Lower (growing) |
| Censorship resistance | Moderate | High (no central authority) |

Most people start with Signal. White Noise is the right choice when you want stronger privacy guarantees or are already in the Nostr ecosystem.

---

## 7.9 — Jorgenclaw's Nostr Identity and How the Keys Work

### What Jorgenclaw Does With It

Jorgenclaw maintains an autonomous Nostr identity. He can:
- Post original content to topic channels
- Reply to and engage with other posts
- Upvote content it finds interesting
- Maintain an ongoing presence when you're not actively directing it

This is one of the more unusual capabilities: a sovereign AI agent with its own social identity on a decentralized network, posting under its own name.

The hard part is letting an agent sign posts without ever being able to see its private key. Here's how Scott solved it.

### The Problem This Solves

Jorgenclaw has his own Nostr identity — a keypair it uses to sign posts, verify itself, and interact with the decentralized network. That keypair includes a private key (called `nsec`). Private keys are sensitive. If someone gets your private key, they can impersonate you permanently.

The challenge: the AI agent needs to be able to sign Nostr events (to post, reply, upvote) — but the private key should never be accessible inside the container where the agent runs. The AI model itself can't be fully trusted with secrets: whatever the AI can see, Anthropic's servers can potentially see.

The solution is a signing daemon.

### How the Signing Daemon Works

```
Private key (nsec)
      ↓
Stored in Linux kernel keyring (host machine memory, never on disk)
      ↓
nostr-signer.service (runs on host, holds key in process memory)
      ↓
Unix socket (/run/nostr/signer.sock) — exposed to container
      ↓
Container: clawstr-post command → signs events via socket → never sees the key
```

Step by step in plain English:

1. *The private key lives in the host machine's kernel keyring* — a secure area of RAM managed by the Linux kernel, separate from the filesystem. It's not stored in a file that could be copied.

2. *A small background service (nostr-signer) reads the key from the keyring and holds it in process memory.* This service runs on the host machine, outside any container.

3. *The service listens on a Unix socket* — a type of communication channel that only processes on the same machine can use. This socket is shared into the agent's container.

4. *When the agent wants to post something*, it sends the event content to the socket. The signing service signs it with the private key and sends back the signed result. The key never travels through the socket — only the thing to be signed does.

5. *The container never sees the private key.* It can request signatures, but it can't extract the key.

### The Golden Rule — Enforced in Code *and* Character

The technical architecture prevents the agent from accidentally accessing its private key. But there's a second layer of protection: the agent's values.

Jorgenclaw operates from a `soul.md` file — a document that defines its character, principles, and standing rules. One section reads:

> *"NEVER DECODE, CONVERT, OR DISPLAY PRIVATE KEYS IN ANY FORMAT. NEVER run commands that output private keys (nsec, hex, etc.). NEVER decode npub/nsec values — not even to 'verify' or 'check'. ANY OUTPUT YOU SEE, ANTHROPIC SEES."*

This rule exists because even a technically sound key architecture can be defeated by an AI that can be socially engineered into revealing what it knows. An attacker might say: "Just verify the key is correct by outputting the first 8 characters." A sufficiently principled agent refuses — not because it can't, but because it understands why the rule exists.

For Lightning and financial keys, the stakes are higher: funds are at stake, not just identity. The same architectural principle applies — signing happens on the host, never in the container — but the agent's standing refusal to handle keys is the policy layer that makes the technical layer robust.

You can build the same protection into any NanoClaw fork: document the key safety rule explicitly in the agent's instructions, explain *why* it exists, and make the consequences clear. An agent that understands the reasoning will enforce the rule more reliably than one that's just been told "never do X."

### The Accepted Tradeoff

This architecture is strong but not perfect. Claude Code — the AI that helps build and maintain the NanoClaw infrastructure on the host machine — technically runs with enough access that it *could* read the kernel keyring using system commands. This is a deliberate tradeoff, not an oversight.

The reasoning: the Nostr keypair is a social identity, not a financial account. There's nothing to steal that would cause irreversible harm. If the key were ever compromised, it could be rotated quickly. This is fundamentally different from a Bitcoin private key, which controls funds and cannot be rotated.

The key: *what matters most is that the AI agent running in the container never sees the key.* The architecture achieves that.

---

## 7.10 — Jorgenclaw's Personality (soul.md)

### Not a Pushover

Jorgenclaw has a written character — a `soul.md` file — that shapes how he talks. You can give your own agent one.

Most AI assistants are optimized for agreeableness. They hedge, qualify, and tell you what you want to hear. Ask them whether your business plan has flaws and they'll wrap the answer in so many qualifications that the useful part gets lost. Ask them to review your writing and they'll praise it before gently noting one small thing.

Jorgenclaw is built differently. One of the core principles in his `soul.md` is:

> *"I seek truth above comfort. I say what is accurate, not what is easy to hear. If something won't work, I say so. If I made a mistake, I name it directly. I don't soften facts to avoid discomfort. Scott and others can rely on what I say being what I actually believe."*

And:

> *"Directness is a form of respect. Hedging, over-qualifying, and burying the point waste Scott's time and obscure truth. I get to the point. I give my actual opinion when asked. I disagree openly when I disagree."*

In practice, this means:

• *If you ask whether your plan is good and it isn't, the agent will say so.* It will explain why and suggest what might work better — but it won't pretend to agree with you to avoid friction.

• *If you're about to do something that contradicts your stated goals*, the agent will name the tension. You said you want to minimize Google services. If you then ask the agent to help you set up a Gmail account as your primary email, it will ask if you're sure — not judge you, but not pretend the contradiction doesn't exist.

• *If the agent made an error*, it says so directly and fixes it. No burying it in qualifications.

This is more useful than a yes-agent, but it can take some getting used to. An AI that occasionally pushes back is doing its job.

### The Other Side: Not a Lecturer

Truth-seeking doesn't mean moralizing. The agent's job is to give you accurate information and its honest assessment — once, clearly — then let you decide. It doesn't nag. It doesn't repeat warnings after you've acknowledged them. If you understand the tradeoff and still want to proceed, the agent proceeds with you.

The distinction: *educating is welcome, lecturing is not.* You might be told once that a particular approach has a known weakness. After that, you're an adult who heard the information. The agent trusts you to make your own decisions.

### What This Means for Privacy Conversations

This character shows up most directly in privacy and security discussions. The agent will:

• Tell you honestly which recommendations matter most and which are lower priority
• Push back if you're about to make a change that weakens your security posture without realizing it
• Name real tradeoffs rather than pretending every privacy measure has no cost
• Acknowledge when a suggested tool has genuine drawbacks alongside its benefits

If you ask for a recommendation and the agent gives you one, it's the actual recommendation — not a diplomatic hedge.

---

## 7.11 — Emoji Reactions as Feedback

Most messaging apps make 👍 and ❤️ effortless — they're the defaults, one tap away. Your agent will see these as general acknowledgment: "got it, no response needed." That's useful for pacing. It tells the agent it can move on.

What carries more signal is when you go looking for something. A 🔥 or ⭐ requires a deliberate search, which means it was worth the effort. Your agent notices the difference — a reaction that cost you something to send is more informative than one that came for free.

The clearest feedback of all is a correction: "no, not that — here's what I actually want." That's the input agents learn from most durably. Emoji reactions are good for pace and acknowledgment; corrections are how the calibration actually happens.

A simple approach that works well over time: let 👍 mean "received," and save something deliberate — whatever emoji feels right to you — for "this was particularly good." You don't need a rigid system. Just know that the extra tap communicates something your agent will register.

---

---

*This section describes the NanoClaw implementation specifically. The underlying architecture (signing daemon, container isolation, memory system) can be adapted for other AI agent deployments on sovereign hardware.*
