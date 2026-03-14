# Part 2 — Device & Operating System

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

> *Feeling stuck? Don't be afraid to ask your AI assistant exactly where you are in the process and what to do next.*

---

## What This Part Covers

Your device is the foundation of everything else. A privacy-respecting messaging app running on a stock Google Android phone is a leaky bucket — the app protects your messages, but the operating system underneath is harvesting your location, contacts, app usage, and behavior constantly and sending it to Google's servers.

This part covers how to harden the foundation itself.

---

## 2.1 — Mobile: GrapheneOS on Pixel

### Why Your Current Phone Is a Problem

A modern smartphone has three separate computers inside it:

1. *The app processor* — what runs Android or iOS, and what you interact with
2. *The baseband processor* — the radio that handles cellular communication, completely separate from Android/iOS
3. *The SIM card* — which can issue commands to the baseband processor that the main OS never sees

Stock Android (especially on a Google Pixel running the default OS) constantly communicates with Google's servers — your location, app usage, device identifiers, Wi-Fi networks you've been near, and more. iOS does the same for Apple.

Additionally, your VPN on a stock phone may not protect you as fully as you think. Apple's own terms of service acknowledge that certain system services bypass VPN tunnels. Google's services on Android do the same.

GrapheneOS solves the app processor and OS layer — it strips out Google's data collection while keeping full Android functionality.

### What GrapheneOS Is

GrapheneOS is a mobile operating system based on Android, designed from the ground up for security and privacy. It runs on Google Pixel phones (the only hardware that meets its strict security prerequisites). When you install GrapheneOS:

• Google receives nothing from your device by default
• You control exactly which apps get which permissions
• You can run up to 32 isolated "profiles" — separate encrypted environments on the same phone, each with its own PIN and apps
• Optional sandboxed Google Play Services are available — you can run apps that require Google services without giving Google system-level access

### Hardware: Which Pixel to Buy

GrapheneOS only runs on Pixel phones. This is by design — Pixel is the only hardware that allows you to relock the bootloader after installing a custom OS, which ensures the OS hasn't been tampered with.

*Check two pages before buying:*
• grapheneos.org — shows which models are currently supported
• endoflife.date/pixel — shows how long each model will receive security updates

Choose a model that will receive updates for at least 2–3 more years.

*How to buy:*
Pay cash at a physical electronics store (Best Buy, Walmart, etc.). Cash is more private than a credit card, which links your identity to the device's serial numbers.

*Avoid:*
• Carrier-contract phones or "variant" devices — these often have OEM unlocking permanently disabled, which means you cannot install GrapheneOS. They may say "unlocked" but OEM unlocking and carrier unlocking are different things.
• Refurbished phones from unknown sources — confirm with the original owner that OEM unlocking is enabled before buying used.
• Buying online — creates a digital trail linking your identity to the device.

### Installing GrapheneOS

The installation process is simpler than it sounds. Naomi Brockwell's "How To DE-GOOGLE Your Phone!" is an excellent video walkthrough — the written steps below match that process.

*What you need:*
• Your new Pixel (not yet set up)
• A computer (Mac, Windows, or Linux — Mac tends to be most seamless)
• A good quality USB-C cable (ideally the one that came with the phone)
• A Chromium-based browser (Chrome, Brave with shields disabled, or Edge)

*The process:*

1. On the Pixel: Go to Settings → About Phone. Tap Build Number 7 times to enable Developer Mode.
2. Go to Settings → System → Developer Options. Toggle "OEM unlocking" on. (If grayed out, your device may be a variant — return it.)
3. Power off the Pixel, then hold Volume Down while powering on until the Fastboot screen appears (red triangle, "Fastboot Mode").
4. On your computer: Open your Chromium browser and go to `grapheneos.org/install/web`.
5. Plug the Pixel into your computer via USB-C.
6. Follow the on-screen steps: Unlock bootloader → Download release → Flash release → Lock bootloader.

Each step has a clear confirmation button. The whole process takes 15–20 minutes. The website guides you through each confirmation prompt on the phone.

*Success looks like:* The GrapheneOS logo appears when you power on the phone instead of the Android/Google logo. On your first boot, Google will show a warning screen about the non-standard OS — this is normal, dismiss it and proceed.

### Recommended First Settings

After installation, before installing any apps:

Go to Settings → Security & Privacy → Exploit Protections:

| Setting | Recommended | Why |
|---------|-------------|-----|
| Auto-reboot | 12 hours | Clears sensitive data from RAM if phone is stolen |
| USB-C port | Charging only when locked | Blocks data connections while locked |
| Turn off Wi-Fi automatically | 5 minutes | Stops passive location broadcasting when not in use |
| Turn off Bluetooth automatically | 5 minutes | Same reason |

### App Stores on GrapheneOS

GrapheneOS doesn't have Google Play Store by default. Instead, you have four ways to get apps:

*1. Graphene Built-in Store*
Pre-installed. Contains Graphene-verified apps and the option to install sandboxed Google Play Services (see below).

*2. Accrescent*
Available inside the Graphene store. Small curated list of privacy-vetted apps. Install it from inside the Graphene store.

*3. F-Droid*
The main store for free and open-source (FOSS) software. Visit `f-droid.org` in your browser and download the APK to install it. F-Droid builds apps from public source code — you can be confident there are no hidden additions.

*4. Aurora Store*
A privacy-respecting frontend for Google Play. Download and install apps without a Google account. Install it through F-Droid. Note: Aurora uses proxy accounts that Google occasionally rate-limits; if apps fail to download, try again later.

*Installing apps directly (APK):* You can download APK files directly from an app's GitHub page or official website. This is the most direct method but requires managing updates manually.

### Profiles: Siloing Your Digital Life

GrapheneOS supports up to 32 separate user profiles on one device. Each profile has:
• Its own PIN and encryption key
• Its own set of apps (which can't see apps in other profiles)
• The ability to auto-reboot when you exit, clearing RAM

*A simple starting point (two profiles):*
• *Owner profile:* Bare minimum — browser, VPN, app stores. Always running in background.
• *Daily driver profile:* Signal, email, maps, and everything you use regularly.

*More advanced (Naomi Brockwell's 6-profile setup for reference):*
1. Owner: minimal apps only
2. Daily driver: apps used throughout the day
3. Sensitive apps: 2FA app, kept offline until needed
4. Invasive apps: anything you rarely use but need (Spotify, banking)
5. Sandboxed Google Play: for apps that require Google services
6. Sandbox: a throwaway environment for untrusted apps

Most people find two profiles (owner + daily driver) sufficient. You can add more as you find specific use cases.

### Sandboxed Google Play Services

Some apps won't work without Google Play Services. GrapheneOS offers a sandboxed version that behaves like any other app — no privileged system access, can't see other profiles.

To install: Go to your Graphene built-in store → scroll down to "Google Play Services mirror" → Install.

Install it in a separate profile if you want to completely isolate Google from your regular apps.

---

## 2.2 — Desktop: Linux

### Why Linux Over Windows or macOS

Windows and macOS both collect telemetry — usage data, crash reports, application activity — and send it to Microsoft and Apple respectively. Both companies can access data on your machine if legally compelled. Both have had documented cases of data collection that users didn't knowingly consent to.

Linux is an open-source operating system. The code is publicly auditable. No single company controls it. There's no built-in telemetry phoning home to a corporation.

### Which Distribution

For most people coming from Windows or macOS, *Pop!_OS*, *Ubuntu*, or *Linux Mint* are the easiest starting points — familiar desktop environment, large support community, extensive documentation.

*Pop!_OS* (by System76) deserves special mention. It's the distribution recommended by Michael Bazzell in *Extreme Privacy* and it's the one Scott uses personally. Pop!_OS is built on Ubuntu but ships with a cleaner default experience, better hardware support (especially for NVIDIA GPUs), and an intuitive app library. If you're migrating from Windows, Pop!_OS is the smoothest transition — the learning curve is gentle and most things just work out of the box. Download it at `pop.system76.com`.

For more control and privacy focus: *Debian* (stable, minimal, no telemetry by design) or *Arch Linux* (complete control, but requires more technical comfort).

For the highest security posture: *Tails* (runs entirely from a USB drive, leaves no trace on the machine) or *Qubes OS* (isolates everything in separate virtual machines — used by Snowden and security professionals).

*Practical starting point:* Pop!_OS for most people making the switch — it's the hands-on recommendation from both Bazzell and from real-world use in this community. You can always move to a more privacy-focused distribution later once you're comfortable.

### Getting Apps Without App Stores

On Linux, most software is installed via a package manager — a command-line or graphical tool that downloads software from verified repositories. This is actually more transparent than app stores: the source of every package is known and the code is public.

Good defaults:
• Use your distribution's official repositories for most software
• Verify GPG signatures on software you download outside repositories
• Avoid running software as root (administrator) unless necessary

---

## 2.3 — Getting Apps Without Google (On Any Platform)

The privacy problem with Google Play Store and Apple App Store isn't just the stores themselves — it's that both require accounts that aggregate your app purchase history, usage, and identity across devices.

Alternatives:

| Store/Method | Platform | Notes |
|-------------|----------|-------|
| F-Droid | Android/GrapheneOS | Free and open-source apps only; high trust |
| Accrescent | GrapheneOS | Curated security-focused apps |
| Aurora Store | Android/GrapheneOS | Google Play without the account |
| Obtainium | Android/GrapheneOS | Installs directly from GitHub releases; no middleman |
| Direct APK | Android/GrapheneOS | Most direct; requires manual update management |
| Homebrew | macOS | Open-source package manager; avoids Mac App Store |
| Package Manager | Linux | Built-in; open repositories |

*The guiding principle:* The fewer accounts you create and the fewer gatekeepers you route through, the smaller your data footprint.

---

*Related: Section 3.4 covers the SIM card as a separate hardware vulnerability. The device OS is only part of the picture — what happens at the cellular radio layer is addressed there.*

---

## 2.4 — Browser Choice

Your browser is one of the highest-exposure points in your digital life. It touches every website you visit, handles your passwords, runs code from remote servers, and — unless configured carefully — builds a detailed fingerprint of your device that advertisers and trackers can identify even without cookies.

Not all browsers are equally private. Here's a practical three-tier framework for daily use:

---

### Tier 1 — LibreWolf (Daily Driver)

*LibreWolf* is a hardened fork of Firefox. It ships with the privacy defaults that Firefox should have but doesn't: no telemetry, no Mozilla data collection, uBlock Origin pre-installed, and fingerprint resistance enabled out of the box.

Why it's the best daily driver:
• *No Google infrastructure* — Firefox is already not Chromium; LibreWolf removes Mozilla's phone-home behavior on top of that
• *uBlock Origin pre-installed* — The single most effective ad and tracker blocker available, already on by default
• *Fingerprint resistance* — Actively works to make your browser look generic rather than uniquely identifiable
• *Accepts extensions* — You can add Bitwarden, NoScript, or anything in the Firefox extension library

*Getting LibreWolf:*
- Desktop: librewolf.net — available for Windows, macOS, Linux
- Android: Available via F-Droid (search "LibreWolf") or direct APK from the LibreWolf site

*The one tradeoff:* A small number of websites — particularly those relying on Google-specific Chromium APIs — won't work correctly. That's what Tier 2 is for.

---

### Tier 2 — Brave (Fallback)

*Brave* is a Chromium-based browser with privacy defaults significantly better than Chrome. When a site won't load in LibreWolf, Brave is the fallback.

Why Brave works as a fallback:
• *Maximum site compatibility* — Built on Chromium, so any site that works in Chrome works in Brave
• *Shields on by default* — Ads, trackers, and cross-site cookies blocked without configuration
• *No Google account required* — You get the Chromium rendering engine without Google's data collection layer (mostly)

*Important limitation:* Because Brave is Chromium-based, Google's rendering engine is still doing the work. This is fine for most uses. If you're visiting sensitive content or need maximum separation from Google's infrastructure, use Tier 1 or Tier 3.

*Getting Brave:* brave.com — available for all major platforms including Android and iOS.

---

### Tier 3 — Tor Browser (Maximum Anonymity / Last Resort)

*Tor Browser* routes all your traffic through the Tor network — a series of encrypted relays that obscures your IP address and location. It's the only browser where a website genuinely cannot determine your physical location under normal operation.

When to use Tor Browser:
• A site is actively blocking LibreWolf or Brave (some streaming services and forums do this)
• You need genuine anonymity for the content you're accessing — journalism, sensitive research, circumventing censorship
• You're accessing .onion addresses (Tor hidden services)

*Tradeoffs:* Tor Browser is noticeably slower. Some sites require JavaScript to be enabled to function, and Tor Browser restricts it by default. Interactive sites (anything requiring login) are harder to use anonymously — logging in defeats some of the anonymity anyway.

*Getting Tor Browser:* torproject.org — always download from the official site only.

---

### Browser Comparison at a Glance

| Browser | Engine | Best for | Privacy level | Speed |
|---------|--------|----------|--------------|-------|
| LibreWolf | Firefox/Gecko | Daily browsing, most sites | Excellent | Fast |
| Brave | Chromium | Sites that break in LibreWolf | Good | Fast |
| Tor Browser | Firefox+Tor | Maximum anonymity, blocked sites | Strongest | Slow |

---

### What About Search Engines?

The browser is only half the equation. Your default search engine sees every query you type. Switching to a privacy-respecting search engine costs nothing and takes two minutes.

| Search Engine | Notes |
|---------------|-------|
| *Brave Search* | Fully independent index — not built on Google or Bing results. Good default. |
| *DuckDuckGo* | Popular, easy to use. Still sources some results from Bing. |
| *SearXNG* | Self-hosted meta-search engine — maximum control, requires setup |

*Quickest change:* In LibreWolf or Brave, go to Settings → Search → Default Search Engine and switch to Brave Search or DuckDuckGo. Takes 30 seconds.

---

### Extensions Worth Having (LibreWolf / Brave)

| Extension | What it does | Notes |
|-----------|-------------|-------|
| *uBlock Origin* | Blocks ads, trackers, malicious scripts | Pre-installed in LibreWolf. Install manually in Brave. Essential. |
| *Bitwarden* | Password manager browser integration | Syncs with your Bitwarden vault |
| *Privacy Badger* | Learns and blocks tracking as you browse | EFF-made; good complement to uBlock |

*Keep extensions minimal.* Every extension you add is code running on every page you visit from a third party. More extensions = larger attack surface. The three above are well-audited and worth it.

---

*See also:*
- *Part 4.2* — VPN: how it interacts with your browser traffic
- *Section 5.2* — Tor Browser for no-KYC Bitcoin trades (RoboSats)
