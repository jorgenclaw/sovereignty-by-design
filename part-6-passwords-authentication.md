# Part 6 — Passwords & Authentication

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## What This Part Covers

Most people have been breached. They just don't know it yet.

The passwords protecting your email, bank, and health records are likely reused across multiple sites — which means when any one of those sites leaks its database (and they all eventually do), an attacker can try those same credentials on everything else. This is called credential stuffing, and it's responsible for the overwhelming majority of account takeovers.

This part covers the two tools that solve this: a password manager (so every account has its own unique, unguessable password) and a second-factor authenticator (so a leaked password alone isn't enough to get in).

---

## The Two Attacks You're Defending Against

**Credential stuffing** — A site you used in 2019 leaked its password database. Someone is now trying your email + that old password on your bank, your email provider, your crypto exchange. If you reused that password anywhere, you're vulnerable.

**SIM swapping** — An attacker calls your mobile carrier, pretends to be you, and convinces them to transfer your number to a SIM they control. Now all the "verify by text message" 2FA codes go to the attacker. They reset your email password. Then your bank. This attack is shockingly common and has cost people their life savings in crypto.

The fix for credential stuffing: a password manager. The fix for SIM swapping: stop using SMS for 2FA.

---

## 6.1 — Password Managers

### Why You Need One

The math is simple: a secure password for each site means a unique 20-character random string per account. No human can remember 200 of those. A password manager remembers them all and types them for you.

What a password manager actually does:
- Generates strong, unique passwords for every account
- Stores them in an encrypted vault only you can unlock
- Fills them in automatically on websites
- Flags when a stored password appears in a known breach

### Recommended: Proton Pass

*Proton Pass* (proton.me/pass) is the recommended starting point for most people, especially if you're already in the Proton ecosystem. It's end-to-end encrypted (Proton cannot read your vault), open-source, and integrates cleanly with Proton Mail and ProtonVPN.

Why Proton Pass works well here:
- *One ecosystem* — same login as your Proton Mail, Drive, and VPN
- *Built-in email aliases* — you can generate SimpleLogin aliases directly inside the app when creating new accounts (more on this in Section 6.3)
- *Cross-device sync* — browser extension on desktop, app on Android/iOS
- *Open-source and audited* — the code is public and has been independently audited

**Quick setup:**
1. Go to proton.me/pass and create an account (or use your existing Proton account)
2. Install the browser extension in LibreWolf or Brave
3. Install the app on your phone
4. Start with your highest-priority accounts: email, banking, crypto exchanges — generate new passwords for each

### Alternative: Bitwarden

*Bitwarden* (bitwarden.com) is the other well-regarded open-source option. Also end-to-end encrypted, also independently audited. Bitwarden is slightly more configurable than Proton Pass and has been around longer. If you want to self-host your vault on your own server, Bitwarden supports that.

Either Proton Pass or Bitwarden is a strong choice. Use whichever integrates better with your existing setup. Don't use LastPass (repeated breaches), 1Password (closed-source, increasing prices), or — especially — your browser's built-in password save.

### Offline Alternative: KeePassXC

If you don't want your vault stored on anyone's server — not even Proton's or Bitwarden's — *KeePassXC* (keepassxc.org) is the answer. It's the most widely recommended implementation of the KeePass format today: actively maintained, FOSS, and well-regarded by its users.

How it differs from Proton Pass and Bitwarden:
- *Fully offline* — your vault is a single encrypted `.kdbx` file that lives only where you put it. No account, no cloud sync, no server to breach.
- *You control sync* — if you want access on multiple devices, you move the file yourself: USB drive, Syncthing, or a shared Proton Drive folder. Many people keep it on a USB drive and that's it.
- *Cross-platform* — KeePassXC runs on Windows, macOS, and Linux. For Android, *KeePassDX* (keepassdx.com) opens the same `.kdbx` files. For iPhone, *Strongbox* is well-regarded (has a free tier).

When KeePassXC makes sense:
- You have a strong preference for nothing leaving your devices
- You're comfortable managing your own backups
- You're in an environment where cloud-connected apps are a concern

The tradeoff: convenience. Auto-fill and sync between devices requires more manual effort than Proton Pass. For most people, the cloud-encrypted options (Proton Pass, Bitwarden) are a better balance of security and usability. KeePassXC is the right call for people who know exactly why they want it.

**Backing up a KeePass vault:** Keep at least two copies — one on your computer, one on a USB drive stored somewhere other than your desk. The vault is encrypted and safe to copy anywhere. If you lose it with no backup, there is no recovery.

### The One Password You Can't Forget

Your password manager has a master password. This is the one password you genuinely need to memorize. It's not stored anywhere — if you lose it, your vault is permanently inaccessible.

Good master password: a passphrase — four or five random words strung together. `correct-horse-battery-staple` is the classic example (and now that it's famous, don't use it). Random word combinations are long enough to be secure and short enough to actually remember.

Write the master password down physically, on paper, and store it somewhere secure: a fireproof safe, a safe deposit box, or with a trusted person. This is the one case where paper beats digital.

---

## 6.2 — Two-Factor Authentication (2FA)

### What It Is and Why It Matters

Two-factor authentication adds a second requirement to log in: not just "something you know" (password) but also "something you have" (a code generated by your phone). Even if an attacker has your password, they can't log in without the one-time code.

The key distinction is *what kind* of second factor you use:

| 2FA Method | Security | Notes |
|-----------|----------|-------|
| Authenticator app (TOTP) | ✅ Good | Codes generated locally, not sent over network |
| Hardware key (FIDO2) | ✅✅ Best | Physical device required; phishing-resistant |
| Email code | ⚠️ Weak | Only as secure as your email account |
| SMS text message | ❌ Vulnerable | Subject to SIM swapping — avoid where possible |

**The rule:** Turn off SMS-based 2FA wherever a better option exists. Enable an authenticator app instead. If a site only offers SMS 2FA, that's better than nothing — but prioritize sites that offer app-based 2FA.

### Recommended: Aegis Authenticator

*Aegis* (getaegis.app) is the recommended 2FA app for Android. It's free, open-source, and stores codes in an encrypted local database that only you control.

What sets Aegis apart from alternatives like Google Authenticator:
- *Encrypted backup* — you can export your vault as an encrypted file and keep it somewhere safe. Google Authenticator can't do this without being tied to a Google account.
- *No cloud sync* — codes never leave your device
- *Import/export* — migrating to a new phone is straightforward
- *Open-source* — the code is public and audited

**Quick setup:**
1. Install Aegis from F-Droid (preferred) or Google Play
2. Set a strong PIN or biometric lock
3. Go to Settings → Backups — enable automatic encrypted backups to a location you control (local storage, then copy to Proton Drive)
4. Start adding accounts: go to the security settings of each important site and look for "Two-Factor Authentication" or "Authenticator app" — scan the QR code

**Priority accounts to enable 2FA on:**
1. Your email (Proton Mail, Gmail) — this is the master key to everything else
2. Crypto exchanges or wallets
3. Proton Pass / password manager
4. Signal (Registration Lock — see Section 3.1)
5. Any account that holds money or sensitive data

### What About Google Authenticator?

Avoid it. Google Authenticator now requires a Google account and syncs to Google's servers by default. Your 2FA codes — the thing protecting your accounts from Google — being stored on Google's servers defeats the point. Aegis is strictly better.

### Backup Codes

When you enable 2FA on any site, you're usually given backup codes — a set of single-use codes to use if you lose your phone. *These matter.*

Store them in your password manager (as a note attached to the login entry). Don't print them and leave them on your desk. Don't put them in an unencrypted file. In your encrypted vault is the right place.

---

## 6.3 — Email Aliases

### The Problem With Your Real Email Address

Every time you give a website your real email address, you're creating a permanent link between your identity and that site. When that site sells its customer list to data brokers (and most eventually do), your real email is in the mix. When it gets breached, your email is associated with the breach. And if you use your email address as your username everywhere, it's one of the two pieces of the credential equation already public.

An alias service solves this by letting you give each site a different address — a throwaway that forwards to your real inbox.

### SimpleLogin

*SimpleLogin* (simplelogin.com) — owned by Proton — is the recommended alias service. It integrates directly into Proton Mail and Proton Pass.

How it works:
- You create aliases like `shopname-randomly-generated@simplelogin.com`
- Emails sent to the alias forward to your Proton Mail inbox
- Replies go back through the alias, so the recipient never sees your real address
- If a site starts spamming you: delete the alias. Spam goes nowhere. Your real inbox is untouched.
- If a site leaks your alias in a breach, you know exactly which site was responsible

**In practice:**
- With Proton Pass browser extension: when you create an account anywhere, the extension offers to generate a SimpleLogin alias on the spot
- For high-risk signups (stores, apps, anything you don't fully trust): always use an alias
- For important accounts (banking, healthcare): you can still use aliases, but note them carefully so you don't lose access

### The Privacy Benefit Beyond Spam

Email aliases also protect against correlation. If you use `yourname@gmail.com` everywhere, a data broker can pull together all the sites and services associated with that address and build a detailed profile. If every site sees a different random alias, there's nothing to correlate.

---

## 6.4 — Putting It Together: Account Security Checklist

Work through this in order of impact. The top items are where most breaches happen.

| Priority | Action | Tool | Notes |
|----------|--------|------|-------|
| 1 | Install a password manager | Proton Pass or Bitwarden | Do this first — everything else builds on it |
| 2 | Generate unique passwords for email + banking + crypto | Password manager | Reused passwords are the #1 attack vector |
| 3 | Enable 2FA on email | Aegis | Your email controls everything else |
| 4 | Enable 2FA on all financial accounts | Aegis | Crypto, banking, payment apps |
| 5 | Turn off SMS 2FA anywhere you've switched to an app | — | SMS 2FA is better than nothing, but app is better |
| 6 | Back up your Aegis vault | Proton Drive | Encrypted export, keep it somewhere safe |
| 7 | Start using email aliases for new signups | SimpleLogin / Proton Pass | Works best as a going-forward habit |
| 8 | Store backup codes in your password manager | Proton Pass / Bitwarden | For every account where you've enabled 2FA |

You don't have to do this in one session. Start with steps 1–3 — those three address the majority of real-world account takeovers.

---

## 6.5 — What These Tools Don't Protect

Be clear-eyed about the limits:

*A password manager doesn't help if your device is compromised.* If someone has persistent malware on your computer, they can capture your master password when you type it. This is why Section 2 (secure OS) matters — you're building layers.

*An authenticator app doesn't stop phishing.* A real-time phishing attack can relay your 2FA code to the attacker's login session faster than you can react. Hardware security keys (FIDO2/WebAuthn) are phishing-resistant; TOTP apps are not. For your highest-value accounts, a hardware key like a YubiKey is worth considering.

*SimpleLogin doesn't make you anonymous.* Aliases prevent correlation and spam, but Proton (the company) knows your real email. For situations where you need genuine anonymity, use Tor Browser and a fully separate identity.

---

*Related: Section 3.5 covers ProtonMail setup. Section 4 covers VPN. The communications stack protects messages in transit; the tools in this section protect your accounts from being taken over in the first place.*
