# Section 5.7 — Your Own Node with Parmanode

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## What You're Setting Up

A Bitcoin *node* is your own copy of the Bitcoin network's rules and history, running on a computer in your home. It checks every transaction for itself instead of trusting someone else's server.

Why it matters:

• *Verify, don't trust.* When your wallet asks "did this payment arrive?", your node answers — not a company's server.
• *Privacy.* Without a node, your wallet asks someone else's server about your addresses, which tells that server which addresses are yours. With your own node, those questions stay in your house.
• *You help keep Bitcoin decentralized.* Every independent node enforces the rules.

You don't need a node to hold Bitcoin safely (Section 5.4 covers wallets). It's the next step once you have the basics.

---

## What You Need

| Item | Notes |
|------|-------|
| A computer that can stay on | Hardware is forgiving: Scott's own node runs on an 8GB-RAM Surface Pro. An older laptop is fine. |
| About 2TB of storage | The full blockchain is large and growing. A 2TB SSD is the comfortable size today. |
| Linux | Parmanode is built for Linux (and also runs on Mac). Pop!_OS (Part 2) works well. |
| Patience | The first sync downloads and checks the whole history. Expect it to take days. |

---

## What Parmanode Is

*Parmanode* ([parmanode.com](https://parmanode.com)) is free, open-source software by Bitcoin educator Arman "The Parman" that installs and manages a Bitcoin node on a normal computer — no command-line expertise needed. From one menu it can install Bitcoin Core plus companion tools such as an Electrum server (so your wallets can connect to your node), Tor, a block explorer, and wallet software like Sparrow.

Parmanode's code is heavily commented, which makes it good for learning how your node actually works.

---

## Installing It

1. On your Linux computer, open a terminal and run Parmanode's official installer:

```bash
curl https://parmanode.com/install.sh | sh
```

   This installer is short: it installs git if needed and downloads Parmanode from its GitHub page (github.com/armantheparman/parmanode). If you prefer, read it first in your browser at parmanode.com/install.sh.

2. Close the terminal, open a new one, and type:

```bash
rp
```

   That opens the Parmanode menu.

3. From the menu, choose to install **Bitcoin Core**.

> *Choose Bitcoin Core deliberately.* Parmanode's menu also offers Bitcoin Knots and builds that include proposed rule changes (such as BIP-110). Those are real choices with real disagreements behind them. If you don't already know why you'd want one of them, pick Bitcoin Core.

4. **Watch for the verification step.** Parmanode checks the Bitcoin Core download two ways before installing it: a checksum, and the GPG signatures of Bitcoin Core's builders. If it reports a failed checksum or signature, stop — don't work around it. Re-run the install later, or verify by hand (bitcoincore.org explains how).

5. Let the first sync run. Your computer will be busy for a while; that's normal.

*Success looks like:* Parmanode shows Bitcoin Core running and the sync progress climbing toward 100%.

---

## Connecting Your Wallets

A node is most useful when your wallets talk to it instead of a public server:

• *Sparrow Wallet* (desktop) — install it from Parmanode's menu or from sparrowwallet.com, then point it at your node or your Electrum server.
• *Zeus* (phone, Section 5.4) — connects to your own node for Lightning.
• *An Electrum server* (installed from Parmanode's menu) lets many wallets connect to your node.

Using Tor for these connections (also available in Parmanode) keeps them private even outside your home.

---

## Keeping It Running

• Leave the computer on and connected. A node that's off isn't checking anything.
• Update through Parmanode's menu every so often.
• Don't keep wallet recovery words on the node computer. The node verifies; your wallet (and your paper backup) holds the keys.

---

## Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| Sync is very slow | Normal for the first sync, especially on older hardware or a hard disk | Let it run; an SSD helps a lot |
| `rp` not found | The terminal hasn't picked up the new shortcut | Close the terminal and open a new one |
| Checksum or signature failed | The download didn't match what Bitcoin Core's builders signed | Stop. Try again later; if it keeps failing, verify by hand before installing anything |
| Disk full | The blockchain outgrew your drive | Use a larger drive (2TB+), or ask Parmanode about pruning (keeps only recent history) |

---

*See also:*
- *Section 5.4* — Wallets (Zeus and Sparrow can connect to your node)
- *Section 5.5* — Lightning payments
- Parmanode documentation: parman.org/diy
