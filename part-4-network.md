# Part 4 — Network

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## What This Part Covers

Even with encrypted messaging and a secure phone, your internet connection leaks information. Every website you visit, every app that checks for updates, every service that syncs in the background — your Internet Service Provider (ISP) sees all of it. Your DNS queries (the requests your device makes to look up every domain name it visits) are often unencrypted, visible to anyone on your network path.

This part covers DNS and VPNs — the two network-layer tools that address these exposures.

---

## 4.1 — DNS: The First Thing to Fix

### What DNS Is

Every time your device connects to a website or service, it first asks a DNS resolver to translate the domain name (like `signal.org`) into an IP address. By default, your ISP handles these queries — and logs them. Your ISP can see every domain you visit, regardless of whether the connection itself is encrypted.

This is a significant surveillance vector. A detailed log of which domains a device queries over a year creates a detailed picture of that person's life: their health concerns, political interests, financial habits, relationship patterns.

Changing your DNS resolver is the single easiest privacy improvement you can make — takes about 5 minutes — and benefits every device on your network simultaneously if done at the router level.

### What to Change It To

*Quad9 (9.9.9.9)* — Free, run by a Swiss non-profit, logs minimal data, and blocks known malicious domains. Excellent default for anyone.

*Proton DNS* — If you're using Proton VPN (see 4.2), your DNS queries are automatically routed through Proton's servers and encrypted. No separate setup needed — the VPN handles it.

*NextDNS* — Free tier available; paid plans add custom blocklists, analytics, and per-device configuration. Good for families wanting to block ads and trackers network-wide. Based in France.

*Mullvad DNS* — Free, run by Mullvad VPN, strong no-logs track record. Natural pairing if you go the Mullvad route.

All of these support *DNS-over-HTTPS* (DoH) and *DNS-over-TLS* (DoT) — protocols that encrypt your DNS queries so they can't be read in transit, even by your ISP.

### Where to Set It

*At the router (recommended):* Setting DNS on your home router applies it to every device on your network — phones, laptops, smart TVs, everything. You change it once and all devices benefit. Look for the DNS settings in your router's admin panel (typically accessed at `192.168.1.1` or `192.168.0.1` in your browser).

*On individual devices:* Each phone and computer can have DNS configured independently. On Android (GrapheneOS), go to Settings → Network → Private DNS and enter your provider's hostname. On iOS, this requires a configuration profile or a VPN with built-in DNS.

### Naomi Brockwell's Coverage

Naomi Brockwell's video "Encrypt Your DNS (STOP Your ISP SNOOPING!)" covers the same concept in clear video format — a good companion to this section if you prefer to watch rather than read.

### Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| Some websites stop loading after changing DNS | The new DNS resolver has stricter filtering | Try a different DNS provider; or temporarily switch back to diagnose |
| DNS-over-HTTPS not connecting | Incorrect hostname or firewall blocking port 443 | Confirm the correct DoH address from your provider's documentation |
| Not sure if DNS is working | Hard to tell from normal browsing | Visit dnsleaktest.com to see which DNS servers your device is using |

---

## 4.2 — VPN: What It Does and What It Doesn't

### The Honest Explanation

VPNs are one of the most misunderstood tools in personal privacy. Marketing from VPN companies often implies more protection than VPNs actually provide. This section gives you the accurate picture.

*What a VPN does:*
A VPN (Virtual Private Network) creates an encrypted tunnel between your device and a VPN server. All your traffic passes through that tunnel. From the outside, your ISP sees that you're connected to the VPN — and nothing else. Websites you visit see the VPN server's IP address, not yours.

This means:
• Your ISP cannot see which websites you visit
• Websites cannot see your real IP address or location
• Public Wi-Fi attackers cannot intercept your traffic

*What a VPN doesn't do:*
A VPN doesn't make you anonymous. It shifts trust from your ISP to the VPN company. The VPN can see all your traffic — exactly what your ISP could see before. If the VPN logs your activity and is subpoenaed, that log exists.

A VPN also doesn't protect against:
• Your browser fingerprint (a combination of settings that identifies your browser uniquely)
• Tracking cookies and pixels on websites
• Accounts you're logged into (if you're signed into Google while using a VPN, Google still knows it's you)
• Malware on your device

*The right mental model:* A VPN is useful for specific threat scenarios — hiding your activity from your ISP, using public Wi-Fi safely, bypassing geographic restrictions — but it's not a privacy silver bullet.

Naomi Brockwell's video "The DARK side of VPNs" addresses the overclaiming in VPN marketing directly.

### Two Strong Recommendations

For most people starting out, *Proton VPN* is the right first choice. For those who want the maximum separation between their identity and their VPN provider, *Mullvad* is the go-further option. Both are trustworthy — the difference is about how much privacy you need in the VPN relationship itself.

---

#### Proton VPN (protonvpn.com) — Best for most people

*Proton VPN* is part of the Proton ecosystem — the same company behind ProtonMail, Proton Drive, and Proton Pass. If you're already using any Proton service, adding Proton VPN is seamless.

Why Proton VPN works well for most people:
• *Free tier available* — The only reputable VPN with a genuinely functional free plan. Limited to slower servers in a few countries, but works. No credit card required to try it.
• *Open-source client* — All apps are published on GitHub and audited.
• *Strong no-logs policy* — Headquartered in Switzerland, audited by independent security firms.
• *Integrated DNS protection* — When connected, your DNS queries automatically go through Proton's servers. No separate DNS configuration needed.
• *Paid plans accept cryptocurrency* — Use Bitcoin or Monero if you want no identity link to your subscription.

*Setting up Proton VPN:*

> *Disclosure: The Proton link below is a referral link. You may receive extra storage or a free trial of paid features, and Scott receives a small benefit. Proton is recommended because of its genuine quality and privacy track record — not the referral. Go directly to proton.me if you prefer.*

*→ Get started with Proton (referral link): [pr.tn/ref/EY1X01YF](https://pr.tn/ref/EY1X01YF)*

1. Go to protonvpn.com — create a free account (or use your existing Proton account)
2. Download the app for your device (available on Android, iOS, Windows, Mac, Linux)
3. Sign in and connect to any server. That's it.

*Success looks like:* The shield icon turns green. Visit dnsleaktest.com to confirm your traffic is routing through Proton.

---

#### Mullvad VPN (mullvad.net) — For maximum anonymity

*Mullvad* is for situations where you want zero account identity — not even an email address linked to your VPN subscription.

• *No account required.* You generate a random account number; no email, no name, no payment details.
• *Accepts cash by mail.* You can pay completely anonymously — put cash in an envelope and mail it.
• *Accepts Bitcoin and Monero.*
• *Audited no-logs policy.* Third-party audited, no user activity logged.
• *Open-source client.*
• *Based in Sweden,* GDPR jurisdiction, strong history of non-cooperation with government data requests.
• *Fixed price:* €5/month regardless of plan length — no upselling.

*Setting up Mullvad:*
1. Go to mullvad.net and click "Generate account number." Write it down — this is your only credential.
2. Pay (Bitcoin is most private; cash by mail is most anonymous).
3. Download the Mullvad app. Enter your account number. Connect.

*Success looks like:* Green "Connected" indicator. Visit mullvad.net/check to confirm your IP shows as Mullvad's.

---

### VPN at the Router Level

Both Proton VPN and Mullvad support router-level configuration — meaning every device on your home network routes through the VPN with no per-device setup. This covers IoT devices (smart TVs, speakers, thermostats) that can't run a VPN app themselves.

Requires a router that supports WireGuard or OpenVPN — pfSense (see 4.3), OPNsense, or most newer Asus routers handle this.

### Troubleshooting

| Problem | What it means | What to do |
|---------|--------------|------------|
| VPN slows down my connection | Traffic routing through an extra server | Connect to a server geographically closer to you |
| Some sites block the VPN | Site detects and blocks VPN IP ranges | Switch servers; streaming sites are aggressive about this |
| VPN disconnects randomly | Network instability or app issue | Enable "Always-on VPN" and "Block connections without VPN" in phone settings |
| Proton app not on GrapheneOS app store | Not in Graphene's built-in store | Install via Aurora Store or download APK from protonvpn.com |
| Mullvad app not in app store | GrapheneOS without Play Store | Download the Mullvad APK directly from mullvad.net |

---

## 4.3 — Home Network Hardening

### What's Actually Inside Your Router

When you buy or rent a "router," you're actually getting four technologies in one box:

• *Switch* — handles fast communication between devices on your local network (like an internal mail carrier that only works inside your building)
• *Router* — the doorman between your local network and the internet, assigning private IP addresses internally while using one public address externally
• *Firewall* — the bouncer, blocking unsolicited traffic from the internet while letting through traffic you requested
• *Wireless access point* — what actually broadcasts your Wi-Fi signal

The problem: manufacturers bundle all four into cheap consumer hardware, then ship it with months-old firmware full of known vulnerabilities. Hackers scan every public IP address constantly looking for these weaknesses — it's automated, widespread, and happens every day.

A 2020 security report found that nearly every consumer router tested had serious security flaws — some with hard-coded admin passwords baked into the firmware itself, accessible to anyone who finds them.

### The Three-Level Approach

*Level 1 — Basic hardening (15 minutes, everyone should do this):*
• Change the default admin password to something strong and unique
• Update the firmware (find the update option in your router's admin panel at `192.168.1.1`)
• Disable features you don't use: UPnP (Universal Plug and Play), WPS (Wi-Fi Protected Setup), remote management
• Create a guest network for IoT devices (smart TVs, thermostats, speakers) so they can't see your personal devices

*Level 2 — Pi-hole for network-wide ad/tracker blocking (1–2 hours):*
Pi-hole is free, open-source software that runs on a Raspberry Pi (a $35 small computer) on your network. It blocks requests to advertising and tracking domains before they ever reach your devices. Every device on your network benefits — no per-device setup. Combined with Quad9 or NextDNS as upstream DNS, this is a powerful privacy layer.

*Level 3 — pfSense on dedicated hardware (a weekend project):*
For full control over your network, replace the consumer router entirely with a dedicated device running pfSense — free, open-source firewall and router software. This is what Naomi Brockwell covers in her "You won't believe how UNSAFE your home router is!" video.

The hardware she recommends: a *Protectly Vault* — a small dedicated device pre-installed with Coreboot (open-source firmware, transparent and auditable). You install pfSense on it, connect it between your modem and the rest of your network, and put your old router into "AP mode" (Access Point mode — Wi-Fi only, with everything else disabled).

pfSense gives you what she calls "nerd knobs" — complete control over every aspect of your network: block outbound telemetry, segment IoT devices, run a whole-home VPN, set custom DNS rules. It's the setup used by serious privacy and security practitioners.

*Naomi Brockwell's "You won't believe how UNSAFE your home router is!" walks through the full pfSense + Protectly Vault installation step by step. Recommended before attempting Level 3.*

### Firewall Basics

A firewall controls what traffic is allowed in and out of your network. Your router has a basic one built in. pfSense gives you full control.

Key things a properly configured firewall can block:
• Unsolicited inbound connections from the internet
• Outbound telemetry from smart TVs and IoT devices
• Known malicious domains and IP ranges
• Traffic from specific devices (e.g., isolating a gaming console from your work computer)

*Starting point:* Most people don't need to go beyond Level 1 hardening. Level 2 (Pi-hole) is an afternoon project with significant ongoing benefit. Level 3 (pfSense) is for people who want the full sovereign home network setup.

---

*Related: Section 4.1 shows how DNS and VPN work together. For the most effective setup, configure DNS at the router level and use Mullvad VPN on individual devices for additional protection when on public networks.*
