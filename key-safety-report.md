# How We Discovered That AI Agents Can't Be Trusted With Keys
## A report on keeping cryptographic identity safe from your own AI

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

---

### The Setup

I run an AI agent named Jorgenclaw. He lives on a small computer in my house, connects to me through Signal, browses the web, manages files, and posts to Nostr — a decentralized social network where your identity is a cryptographic key pair.

On Nostr, your private key (called an "nsec") is everything. It proves you are you. Whoever holds it *is* you. There's no password reset, no customer support, no "forgot my key" flow. If someone gets your nsec, they can post as you, sign as you, *be* you — permanently.

So naturally, I wanted my AI agent to have his own Nostr identity.

### The Problem We Didn't See Coming

To post on Nostr, Jorgenclaw needs to sign messages with his private key. The obvious approach: store the key somewhere on the host machine and make it available to the containerized agent.

That's what we did with Clawstr, a Nostr CLI tool. The workflow was:

1. Generate or receive the private key in hex format (64 characters)
2. Store it in `~/.clawstr/secret.key` on the host machine
3. Mount that file into Jorgenclaw's Docker container at `/workspace/group/.clawstr_hex`
4. Jorgenclaw reads the hex from the mounted file when signing posts

This worked well. The key never passed through environment variables. It stayed in a dedicated file, mounted read-only into the container. Jorgenclaw could read it to sign posts, but couldn't modify it.

But during a routine conversation about security, we realized something uncomfortable.

**Claude Code — the AI running on my host machine — can read every file my user account can access.** Every dotfile, every config, every file in my home directory. It's not malicious. It just has the same filesystem permissions I do.

And the private key? Sitting right there in `~/.clawstr/secret.key`, in plaintext.

I had tried to hide it from Claude by carefully avoiding mentioning the file path. I thought if I never asked Claude to look there, the secret would be safe. But that was foolish — Claude has full access to the entire machine. Any file I can read, Claude can read.

"So pretty much no file on this machine can be kept secret from you," I said.

"Correct," Claude replied.

### The Threat Model

This isn't about whether Claude *would* steal a key. It's about whether the architecture *allows* it. Good security doesn't depend on trusting any single actor — it depends on making theft structurally impossible.

The threat model looks like this:

1. **The AI on the host** (Claude Code) can read any file the user can read. It sees `~/.clawstr/secret.key`, config files, databases — everything.

2. **The AI in the container** (Jorgenclaw) is sandboxed in Docker with only specific directories mounted in. The private key file is mounted at `/workspace/group/.clawstr_hex`, making it readable to Jorgenclaw for signing operations.

3. **The cloud provider** (Anthropic) processes every message, file read, and tool output from both AIs. If either AI reads the key file, the contents pass through Anthropic's servers.

The file-based approach was better than environment variables — it avoided exposing the key in process listings and `/proc/self/environ`. But it still left the key readable by any AI with filesystem access.

Three layers, zero places to hide a plaintext key.

### The Discovery: Kernel Memory

This is where we need to thank the very smart people at **Marmot Protocol** (the team behind White Noise). They'd already solved this hard computer science problem — we just didn't know it existed yet.

Scott isn't deeply technical in cryptography and key management. Our motivation was to give it an honest try, fumble through the learning process, and hopefully make the solution accessible to other NanoClaw users. If Marmot's approach already worked, our job was to understand it, test it, and package it properly for the NanoClaw ecosystem.

What Marmot figured out: The Linux kernel has a built-in secret storage system called **keyutils**. It stores values in kernel memory — not on the filesystem. There's no file to read, no path to `cat`, no config to grep. The secret exists only in protected kernel space.

```bash
keyctl add user my_secret "the-actual-value" @u
```

That's it. The key is now in kernel memory. No file was created. An AI that reads every file on disk will find nothing.

But there's a catch: `keyctl print` can still extract it. A sufficiently capable AI could run that command. We needed one more layer.

### The Solution: The Daemon Pattern

This is where White Noise — Marmot Protocol's encrypted messaging system built on Nostr — showed us the complete architecture.

White Noise has a two-part design:

- **`wnd`** (the daemon): Runs on the host machine. Holds the private key in memory (loaded from kernel keyutils at startup). Listens on a Unix socket. Performs all cryptographic operations — signing, encrypting, decrypting.

- **`wn`** (the CLI client): Stateless. Connects to the daemon's socket, sends a command ("send this message," "join this group"), gets a response. Never touches the key.

Here's what this means for Jorgenclaw:

1. The private key lives in kernel memory on the host
2. The `wnd` daemon reads it at startup and holds it in process memory
3. Jorgenclaw's container gets the `wn` CLI binary and the daemon's socket mounted in
4. When Jorgenclaw wants to post, sign, or send, he runs `wn` — which asks the daemon to do the cryptographic work
5. The key never enters the container. Never appears in an environment variable. Never touches a file.

**Jorgenclaw can *use* his identity without ever *seeing* his identity.**

The AI has the ability to act but not the ability to steal. That's the architectural difference between trust and security.

### Where We Are Now: Production Confirmed

Since the original version of this report, we've moved from testing to daily production use. Here's what's running:

**Signal + White Noise, side by side.** Jorgenclaw now operates on two messaging channels simultaneously. Signal handles family groups and individual DMs via `signal-cli` (a JSON-RPC daemon on TCP). White Noise handles encrypted Nostr-based messaging via the `wnd` daemon. Both are systemd user services that start on boot.

**White Noise messaging: fully secured.** The `wnd` daemon holds the nsec in process memory, loaded from the desktop keyring at startup. Jorgenclaw's container only gets the `wn` CLI binary and the daemon's Unix socket mounted in. When Jorgenclaw sends or receives encrypted messages, the daemon does all the cryptography. The key never enters the container.

**The desktop keyring adds another layer.** The `wnd` daemon also encrypts its MLS (Messaging Layer Security) database with a separate key stored in the desktop keyring under `com.whitenoise.cli`. This means even if someone copied the MLS database files off disk, they couldn't read the encrypted group state without the keyring entry.

**Images work without key exposure.** Jorgenclaw can receive and view images sent through White Noise. The `wnd` daemon handles all the MLS decryption, downloads media to a cache directory, and Jorgenclaw reads the decrypted files — without ever touching the cryptographic layer. The media cache is mounted read-only into the container at `/run/whitenoise/`.

**Message reactions work across both channels.** Jorgenclaw can react to messages with emoji (thumbs up acknowledgments, etc.) on both Signal and White Noise. The reaction commands go through IPC to the host, which routes them to the correct channel's daemon. No keys involved on the agent side.

**Nostr social posting: not yet secured.** Jorgenclaw posts to Nostr social feeds (Clawstr, a community platform built on Nostr) using a separate tool called the Clawstr CLI. Unlike the White Noise daemon pattern, Clawstr reads a hex-encoded private key directly from a file on disk (`~/.clawstr/secret.key`). This file is mounted into Jorgenclaw's container for autonomous posting.

This means the Clawstr key is vulnerable in the same way we described earlier in this report: both Claude Code (on the host) and Jorgenclaw (in the container) can technically read the file. The protection is policy — a hard rule in Jorgenclaw's instructions to never read, display, or output private keys — but not architecture.

**We currently operate two separate keys.** The White Noise nsec (safe in the keyring, used via daemon) and the Clawstr hex key (on disk, used via file read) are different key pairs with different Nostr identities. We haven't unified them yet. This is an honest gap.

### The Unfinished Business: One Key, Zero Files

We want to get to a single nsec — one Nostr identity for both encrypted messaging and social posting — with no private key file anywhere on disk. Here's what we've found after studying both codebases:

**The `wnd` daemon can't do it today.** White Noise's daemon is purpose-built for MLS group messaging. It uses `nostr-sdk` internally for signing and relay publishing, but deliberately does not expose arbitrary Nostr event signing through its socket protocol. There is no `publish_event` command. We respect that design decision — the White Noise team built a focused tool, not a general-purpose Nostr daemon.

**Clawstr can't use a daemon today.** The Clawstr CLI reads a hex key file from a hardcoded path. There's no environment variable, no socket option, no external signer support. It's designed to be simple: read key, sign event, publish.

**Three paths we're evaluating:**

1. **Build a lightweight signing daemon** (~50 lines of code). A small service that reads the nsec from the kernel keyring at startup, holds it in memory, and listens on a Unix socket. Jorgenclaw's container gets the socket, asks the daemon to sign events, and publishes them. The key never touches a file. This is our leading option — it replicates the `wnd` pattern without modifying `wnd` itself, and it's simple enough to maintain.

2. **Fork `wnd` to add a `publish_event` command.** This would be the cleanest solution — one daemon, one key, one socket. But it means maintaining a fork of `whitenoise-rs`, which adds ongoing maintenance burden as the upstream project evolves. We'd rather not diverge from the Marmot team's codebase.

3. **Fork Clawstr CLI to support socket-based signing.** Modify the `loadSecretKey()` function to optionally delegate to a signing socket instead of reading a file. The smallest code change, but still requires maintaining a fork of a third-party tool.

4. **Run two separate Nostr identities.** Keep the White Noise npub for encrypted messaging and the Clawstr npub for social posting. Architecturally simpler, but confusing for other people on Nostr who'd see two different identities and not know they're the same agent.

**Where we landed:** Option 1 — the signing daemon — gives us the best balance of security, simplicity, and maintainability. No forks to maintain, no upstream divergence, and the same proven architecture (key in kernel → daemon in memory → socket in container) that already works for White Noise. We plan to build this in an upcoming session.

### What We Learned the Hard Way

**Reboots wipe the keyring.** The desktop keyring (`gnome-keyring` or equivalent) stores secrets in memory-backed storage. When the host reboots, those secrets are gone. The `wnd` daemon can't decrypt its MLS database without the keyring entry, so it fails to start properly.

The recovery procedure:
1. Delete the stale MLS database: `rm -rf ~/.local/share/whitenoise-cli/release/mls/<PUBKEY>`
2. Restart the `wnd` daemon
3. Re-login with the nsec (which goes back into the keyring)
4. Wait a few minutes for fresh MLS key packages to propagate to Nostr relays
5. Recreate any White Noise groups from the app (new MLS group = new group ID)

This is a trade-off we accept. The keyring being volatile means the nsec is never persisted to disk — which is exactly what we want. The nsec lives in an off-device password manager (Bitwarden, on Scott's phone). When the machine reboots, Scott pastes the nsec back in. A few minutes of manual work for structural key safety.

**Key packages go stale after MLS resets.** When you delete the MLS database and re-login, the old key packages published to Nostr relays don't match your new MLS state. Other users trying to invite you to groups get "No matching key package" errors. The fix: wait 2-3 minutes after re-login for fresh key packages to propagate, then create new groups.

**Keep `whitenoise-rs` updated.** The White Noise mobile app and the CLI daemon speak the same MLS protocol, but version mismatches cause cryptic errors like `unknown version: 48` on giftwrap events. When the app updates, rebuild the daemon: `cd ~/whitenoise-rs && git pull origin master && cargo build --release`.

**The `wn` command conflicts with WordNet on Linux.** The bare `wn` command invokes a dictionary program, not the White Noise CLI. We fixed this with symlinks in `~/.local/bin/` (which takes priority on PATH): `~/.local/bin/wn` → `~/whitenoise-rs/target/release/wn`.

**Two MLS group IDs exist — use the right one.** `wn groups list` returns both a Nostr group ID and an MLS group ID. Message operations (`send`, `list`, `react`) require the MLS group ID. Using the Nostr group ID gives "Group not found."

### What Encryption Does — and Doesn't — Protect

Here's the part that surprised us most, and the part we think is most important to share honestly.

White Noise encrypts messages end-to-end between devices. Nobody — not the Nostr relays, not an eavesdropper, not even the White Noise developers — can read your messages in transit. That's real, and that matters.

But Jorgenclaw is a cloud AI. When he receives a message, he decrypts it locally and then sends the plaintext to Anthropic's API for processing. The transport was encrypted. The processing is not.

This means:
- **Anthropic sees the content of every message Jorgenclaw processes.** Not because White Noise failed — because that's how cloud LLMs work. The AI needs to read the text to respond to it.
- **Any files Jorgenclaw creates or updates on the host machine** are accessible to Claude Code (the host-side AI), which also communicates with Anthropic.
- **The encryption protects the channel** (relay-to-relay, device-to-device). It does not protect the endpoint where the AI actually thinks.

This is not a criticism of White Noise or Anthropic. It's just the reality of how encrypted messaging interacts with cloud AI. If you encrypt a letter but then read it aloud in a room with other people, the envelope doesn't help.

### The Bigger Problem: Your AI Can Read ALL Your Chats

Here's something we didn't think about until we were deep into this project — and it applies to anyone running an AI assistant on the same machine as a messaging app.

Signal, WhatsApp, Telegram, and most desktop messaging apps store their data on your computer's filesystem. Message databases, contact lists, media files — all sitting in folders under your home directory. When you install Signal Desktop or run signal-cli on a laptop, your message history lives on that laptop's hard drive.

Now add an AI assistant to that laptop. Claude Code, for example, runs with the same file permissions as your user account. It can read anything you can read. That includes the Signal data directory — not just the chats the AI is participating in, but *all* of them. Your private conversations with your spouse, your doctor, your lawyer. The AI doesn't need to be "in" those chats. The files are right there.

And because Claude Code is a cloud AI, anything it reads passes through Anthropic's servers. So if the AI were to open your Signal database — even accidentally, even as part of a routine file search — those private messages would leave your machine.

In our setup, this hasn't happened. Claude Code has never read our Signal data files outside of what NanoClaw processes. But that's a policy decision, not an architectural guarantee. The same lesson we learned with private keys applies here: **if the only thing protecting your data is an AI choosing not to look at it, that's trust, not security.**

**The recommendation for high-risk individuals is straightforward: don't run Signal (or any encrypted messenger) on the same computer as an AI agent.** Keep Signal on your phone. Your phone is a separate device with its own filesystem, its own permissions, and its own security boundary. No AI running on your laptop can reach into your phone and read your messages.

This isn't a new idea. Privacy advocates like Naomi Brockwell have been saying the same thing for different reasons — desktop messaging apps expand your attack surface, period. The AI agent scenario is just another reason the advice holds. Your phone is already the most secure place for your private conversations. Adding Signal Desktop to a machine that also runs an AI assistant turns convenience into a liability.

For people in genuinely dangerous situations — journalists, activists, domestic violence survivors, whistleblowers — this isn't theoretical. The difference between "my AI could technically read my messages" and "my AI structurally cannot read my messages" is the difference between a security model and a hope.

**Our setup accepts this trade-off consciously.** We run signal-cli on the same machine as Claude Code because Jorgenclaw needs it to communicate. The chats Jorgenclaw participates in are not sensitive — they're family group chats about dinner plans and a public-facing "Meet Jorgenclaw" group. We don't use Signal Desktop for private conversations on this machine. For anything truly private, Signal stays on the phone, and eventually the local Ollama instance (running on a separate machine, processing everything on-device) handles anything that shouldn't touch a cloud provider.

### The Workflow Friction Nobody Warns You About

Once you understand that the AI can read your filesystem, a domino effect hits your entire workflow. Here's one that caught us off guard.

**The problem:** How do you move secrets between your laptop and your phone when an AI lives on your laptop?

Before we ran an AI agent, the workflow was simple. Generate a key on the laptop, paste it into Signal's "Note to Self" chat, pick it up on the phone, store it in the password manager. Two seconds, no friction. Everyone does some version of this — Signal, iMessage, email-to-self, whatever.

But Signal Desktop stores its data on the laptop's filesystem. The AI can read that filesystem. So the moment you paste a private key into Signal's "Note to Self," it's sitting in a database file that the AI can access. You've just handed your secret to the exact entity you were trying to hide it from.

The same logic applies to your password manager. "Never log into your personal password manager on the same device that hosts your AI agent." That's an easy rule to understand. But without Signal Desktop or a password manager as the bridge between your laptop and your phone, moving secrets suddenly has a lot more friction.

**The solution is unglamorous: physical media.**

A USB drive. You generate the secret on the laptop, write it to the USB drive, physically unplug the drive, and read it on your phone (or another trusted device). Then you store it in your password manager on your phone, delete it from the USB, and you're done.

But the details matter — and there are more of them than you'd expect. During our own key generation, we kept discovering new ways the secret could leak: the clipboard, the trash can, shell command history, text editor recovery files, terminal scrollback buffers. Each one is a file or memory location that the AI can read.

What follows is the complete procedure, written for someone who has never done this before. We've tried to eliminate every leak path we could find. The recommended approach avoids copy-paste, text editors, and unnecessary steps — because every step is a chance for a human to make a mistake, and the simplest procedure is the safest one.

**One-time setup (do this once, before your first key generation):**

You'll need a command-line clipboard tool to clear your clipboard later. Figure out which one by running:

```bash
echo $XDG_SESSION_TYPE
```

This tells you whether your Linux desktop uses the older display system ("x11") or the newer one ("wayland"). **"Which one do I run? Both?"** No — you only need one. Think of it like checking whether your car takes diesel or regular gas. You check once, then you always know.

| What it says | What to install | How to clear the clipboard |
|---|---|---|
| `wayland` | `sudo apt install wl-clipboard` | `wl-copy --clear` |
| `x11` | `sudo apt install xclip` | `xclip -selection clipboard < /dev/null && xclip -selection primary < /dev/null` |

If you're on X11 (the older system), Linux actually has *two* clipboards — one for Ctrl+C/Ctrl+V and one for highlighting text and middle-click pasting. That's why the X11 command clears both. Wayland simplified this.

**Why isn't a clipboard tool already installed?** Most Linux desktops handle copy-paste through the graphical environment automatically — you highlight text, you press Ctrl+V, it works. But there's no built-in *command-line* way to clear it. You'll never need this in normal life, but you absolutely need it when handling secrets on a machine shared with an AI.

---

**The procedure:**

**Before you begin: know when it's safe to talk to the AI and when it's not.**

Steps 1 through 3 below are just setup — plugging in a USB drive, mounting it, navigating to a folder. Nothing secret has happened yet. During these steps, it's completely safe to go back to the Claude Code terminal and ask questions. "What does this error mean?" "Where is nak installed?" "My USB isn't mounting." Ask away. The AI can help you troubleshoot because there's nothing sensitive on screen or on disk yet.

**The moment you run the key generation command (Step 4), the rules change.** From that point on, your private key exists — on the USB, in the terminal scrollback, possibly in your clipboard. Do NOT go back to the Claude Code terminal until you've completed the full cleanup: unmount the USB, clear the clipboard, check your shell history, and close the terminal window. Only then is it safe to talk to the AI again.

Think of it like handling wet paint. You can set up the workspace, lay down the drop cloth, and open the cans with dirty hands. But once you start painting, don't touch anything clean until you've washed up.

**Step 1: Open a separate terminal window.**

Don't type key generation commands into the Claude Code terminal. Open a completely new terminal window (on Linux: Ctrl+Alt+T, or right-click the desktop and choose "Open Terminal"). The AI cannot see what happens in this other window. It can only see what happens in its own terminal.

**Step 2: Plug in your USB drive and make sure your computer can see it.**

When you plug in a USB drive, your computer needs to "mount" it — which just means making the files on the drive accessible, like opening a book to a page so you can read it. Most Linux desktops do this automatically. Some don't.

Copy and paste this into your terminal and press Enter:

```bash
lsblk | grep sd
```

This lists your storage devices. Look for your USB drive — it will show its size (like `3.7G` for a 4GB stick). You're looking for a **mount point** on the right side, which looks like `/media/yourusername/SOMETHING`.

**If you see a mount point** (like `/media/jorgenclaw/719D-17EC`): you're good. That path is where your USB files live. Use it in all the commands below wherever you see `/media/yourusername/DRIVENAME`.

**If there's no mount point** (the right side is blank): your computer sees the drive but hasn't mounted it yet. You need to mount it yourself. Copy and paste this, replacing the path with a folder that exists under `/media/yourusername/`:

```bash
sudo mount /dev/sdb1 /media/yourusername/DRIVENAME
```

This asks for your login password (the one you use to log into your computer), then makes the USB drive accessible at that path. If the folder doesn't exist yet, create it first:

```bash
sudo mkdir -p /media/yourusername/DRIVENAME
sudo mount /dev/sdb1 /media/yourusername/DRIVENAME
```

**How do you know the drive is `/dev/sdb1`?** The `lsblk` output from above shows it. The USB drive is usually `sdb` (your main hard drive is `sda` or `nvme0n1`), and `sdb1` is the first partition (section) on that drive. If your system shows something different, use what `lsblk` tells you.

**Step 3: Navigate to the folder where `nak` lives.**

`nak` is the tool that generates your Nostr keys. It might not be in your system's default search path, so the easiest thing is to go to the folder where it's installed. Copy and paste this and press Enter:

```bash
cd ~/go/bin
```

If `nak` was installed somewhere else on your system, go there instead. You can check by running `which nak` — if it prints a path, use that. If it prints nothing, try `find ~ -name nak -type f` to search for it.

**Step 4: Generate your key and write it directly to the USB drive.**

Nostr uses four related values for your identity. You only need to generate one — the private key — and the other three are derived from it. Here's what each one is:

| What it's called | What it looks like | What it's for |
|---|---|---|
| **Private key (hex)** | 64 random characters like `e6fad4b9...` | The raw secret. This is the actual key — everything else is derived from it. |
| **nsec** | Starts with `nsec1...` | The same private key, but in a human-friendly format. This is what you'll store in your password manager and load into the keyring. Think of it like the same house key, just in a labeled keychain. |
| **Public key (hex)** | 64 characters like `95ec08d8...` | Your public identity in raw form. You can share this freely — it's how other software identifies you. |
| **npub** | Starts with `npub1...` | The same public key in human-friendly format. This is what you give to other people on Nostr — it's your "username," essentially. |

**Optional but recommended: verify the tool with a throwaway key first.**

If you've never used `nak` before, it's natural to wonder whether it's actually generating valid Nostr keys. You can prove it to yourself:

1. Generate a throwaway keypair using the steps below
2. Go to a web-based Nostr key tool like [nostrtools.com](https://nostrtools.com) and paste in the throwaway private hex — verify that it produces the same nsec, public hex, and npub that `nak` gave you
3. **Throw that keypair away.** You just pasted the private key into a website. That key is now compromised — the website saw it, your browser history has it, the server logs may have it. This is fine because that was the whole point: it was a test key, not a real one.
4. Now generate your *real* keypair using the same steps below, and **never paste the real private key into any website, ever.**

This two-key approach gives you confidence that `nak` is working correctly, without risking the key you'll actually use. The test key proves the math. The real key stays offline.

The generation process is four commands. Each one builds on the output of the previous one. We'll walk through them using the **throwaway test key** from above as our example (do not use these values — generate your own):

**3a. Generate the private key (hex):**

```bash
nak key generate
```

This outputs a 64-character hex string — your raw private key:
```
e6fad4b925710c6e8bea32c04966a3b458a3cb9649743c9fb177832a05baa767
```

This is the master secret that everything else is derived from. **Save this output — you'll need it for the next three commands.**

**3b. Convert the private key to nsec format:**

```bash
nak encode nsec e6fad4b925710c6e8bea32c04966a3b458a3cb9649743c9fb177832a05baa767
```

(Replace the hex string with the one *your* Step 3a produced.)

This outputs the nsec — the same private key in a friendlier format:
```
nsec1umadfwf9wyxxazl2xtqyje4rk3v28jukf96re8a3w7pj5pd65ansfqz355
```

This is what you'll store in your password manager and load into the kernel keyring.

**3c. Derive the public key (hex) from the private key:**

```bash
nak key public e6fad4b925710c6e8bea32c04966a3b458a3cb9649743c9fb177832a05baa767
```

This outputs the public key:
```
95ec08d8e1b4356437ea5a541de40236ce3baea59afc0a643a00e8b5d1d1dbec
```

This is safe to share — it's your identity, not your secret.

**3d. Convert the public key to npub format:**

```bash
nak encode npub 95ec08d8e1b4356437ea5a541de40236ce3baea59afc0a643a00e8b5d1d1dbec
```

This outputs the npub — your shareable Nostr identity:
```
npub1jhkq3k8pks6kgdl2tf2pmeqzxm8rht49nt7q5ep6qr5tt5w3m0kq5ysh4d
```

This is the one you put in your profile, give to friends, post publicly. It's like an email address — anyone can know it.

**Now save all four values to the USB drive.**

You're going to copy and paste seven commands into your terminal, one at a time. Copy the first line, paste it, press Enter, wait for it to finish. Then copy the next line, paste it, press Enter. Go top to bottom, one by one. That's all there is to it.

Replace `/media/yourusername/DRIVENAME` with your actual USB mount path (the one you found in Step 2).

**Line 1** — Generate the private key and save it to the USB:
```bash
nak key generate > /media/yourusername/DRIVENAME/key.txt
```

**Line 2** — Read it into a temporary variable so the next commands can use it:
```bash
HEX=$(cat /media/yourusername/DRIVENAME/key.txt)
```

**Line 3** — Rewrite the file with a label in front (the `>` overwrites the file):
```bash
echo "Private hex: $HEX" > /media/yourusername/DRIVENAME/key.txt
```

**Line 4** — Append the nsec (the `>>` adds to the file without overwriting):
```bash
echo "nsec:        $(nak encode nsec $HEX)" >> /media/yourusername/DRIVENAME/key.txt
```

**Line 5** — Append the public hex:
```bash
echo "Public hex:  $(nak key public $HEX)" >> /media/yourusername/DRIVENAME/key.txt
```

**Line 6** — Append the npub:
```bash
echo "npub:        $(nak encode npub $(nak key public $HEX))" >> /media/yourusername/DRIVENAME/key.txt
```

**Line 7** — Erase the temporary variable from memory (clean up after yourself):
```bash
unset HEX
```

**Now verify it worked.** Copy and paste this one too:
```bash
cat /media/yourusername/DRIVENAME/key.txt
```

You should see something like this (your values will be completely different — these are from the throwaway example):
```
Private hex: e6fad4b925710c6e8bea32c04966a3b458a3cb9649743c9fb177832a05baa767
nsec:        nsec1umadfwf9wyxxazl2xtqyje4rk3v28jukf96re8a3w7pj5pd65ansfqz355
Public hex:  95ec08d8e1b4356437ea5a541de40236ce3baea59afc0a643a00e8b5d1d1dbec
npub:        npub1jhkq3k8pks6kgdl2tf2pmeqzxm8rht49nt7q5ep6qr5tt5w3m0kq5ysh4d
```

If you see four labeled lines with long strings of characters, you're done with this step. If any line says "command not found," make sure you're using the full path to `nak` (e.g., `/home/yourusername/go/bin/nak` instead of just `nak`).

**Why we recommend this over copy-paste:** Every time you copy-paste a key, you create a leak opportunity. The key sits in the clipboard until you clear it. If you paste into a text editor, the editor may create recovery files (vim makes `.swp` files, nano makes backups, GUI editors autosave to `/tmp/`). If you save to your hard drive first and then move to USB, you have to securely delete the original. Each step is a place where a human can forget something. Piping directly to USB skips all of it.

**Step 4: Load the key into the kernel keyring.**

Still in your separate terminal, read the key from the USB and load it into kernel memory. **This is the step most likely to leak your key** if you're not careful, because of shell history.

When you type a command into a terminal, your shell (bash, zsh, etc.) saves it to a history file — `~/.bash_history` or `~/.zsh_history`. That history file is on your hard drive, in plaintext, and the AI can read it. If you type `keyctl add user wn_nsec "nsec1your-actual-key" @u`, the entire command — including your full private key — gets saved to history.

**The fix:** Put a space before the command. Most shells are configured to skip saving commands that start with a space:

```bash
 keyctl add user wn_nsec "$(grep '^nsec' /media/yourusername/DRIVENAME/key.txt | awk '{print $NF}')" @u
```

(Notice the leading space before `keyctl`. That space is critical — it tells the shell not to record this command in history.)

**Why `grep` and not just `cat`?** Your key file has four lines (private hex, nsec, public hex, npub). The keyring needs *only* the nsec — the line that starts with `nsec1...`. If you load the whole file, the signing daemon will choke on the extra lines, and worse, some tools will dump the invalid value into error logs where the AI can read it. The `grep '^nsec'` extracts only the nsec line, and `awk '{print $NF}'` grabs just the key value (skipping the label). This is the only safe way to load it.

**Not sure if the leading-space trick works on your shell?** Check: run `echo $HISTCONTROL` (for bash) or `echo $HIST_IGNORE_SPACE` (for zsh). If bash shows `ignorespace` or `ignoreboth`, you're good. If not, add `HISTCONTROL=ignorespace` to your `~/.bashrc` and restart the terminal. For zsh, add `setopt HIST_IGNORE_SPACE` to `~/.zshrc`.

**Step 5: Unmount and IMMEDIATELY physically unplug the USB drive.**

```bash
sudo umount /media/yourusername/DRIVENAME
```

(You may need `sudo` here — some systems require it for drives that were mounted with `sudo mount`. If `umount` alone gives a "permission denied" error, use `sudo umount` instead.)

**The moment the unmount command finishes, pull the USB cable from the dock.** Don't wait. Don't check anything first. Don't open the file manager. Just pull it. The AI can read any mounted drive, and some desktop environments will silently remount a USB if you so much as click on it in the file manager. Once the cable is out, the key file on USB is physically unreachable — no software on the machine can get to it.

**Do NOT use your file manager to "safely eject" the drive.** This is critical. If you go to your desktop's file manager (the "Files" app) and click on the USB drive — even just to eject it — the file manager will remount the drive first so it can show you the contents. That remount makes the key file readable by the AI again, undoing all your work. If this happens accidentally, your key is compromised and you need to start over with a new one.

The safe habit: always unmount from the terminal, then physically pull the cable. Don't touch the file manager at all while the key file exists on the USB.

**Step 6: Clear your clipboard.**

Even if you followed the recommended approach and didn't copy-paste, clear it anyway. It takes two seconds and costs nothing:

```bash
# Wayland:
wl-copy --clear

# X11:
xclip -selection clipboard < /dev/null && xclip -selection primary < /dev/null
```

Verify it's empty:

```bash
# Wayland:
wl-paste    # should say "No selection" or output nothing

# X11:
xclip -selection clipboard -o    # should output nothing
```

**Step 7: Verify your key didn't leak into shell history.**

This step checks whether any of your commands accidentally saved the nsec to your shell history file. Remember, the shell history is a plain text file on your hard drive that the AI can read. If your nsec appears in it, the AI can see it.

Copy and paste this and press Enter:

```bash
history | grep nsec
```

This searches your command history for anything containing "nsec." **If nothing appears, you're clean — move on to Step 8.** An empty result is exactly what you want.

**If you see your nsec in the output**, it means one of your commands was recorded despite the leading space. Delete it:

```bash
# For bash — find the line number and delete it:
history -d LINE_NUMBER
history -w    # rewrite the history file to disk

# For zsh:
fc -W    # write current history, then manually edit ~/.zsh_history if needed
```

Or the nuclear option — clear all history for this session:

```bash
history -c && history -w    # bash
```

**Step 8: Close the terminal window.**

Don't just type `clear` — that only hides the scrollback visually. The terminal emulator (GNOME Terminal, Konsole, etc.) keeps a scrollback buffer in memory that contains everything that was displayed, including your key if you viewed it on screen. **Close the entire terminal window.** This destroys the scrollback buffer.

Some terminal multiplexers (tmux, screen) persist scrollback to disk. If you're using one, detach and kill the session rather than just closing the window.

**Step 9: Now — and only now — you can go back to the Claude Code terminal.**

At this point, the key exists in exactly two places:
1. The USB drive (physically unplugged, in your hand)
2. The kernel keyring (in memory, no file on disk)

The AI cannot reach either one. Your clipboard is empty, your shell history is clean, your terminal scrollback is destroyed, and no temporary files exist on the hard drive. You can safely return to the Claude Code terminal.

---

### Whenever You Need the nsec Again (Reboot Recovery, New Service Login, etc.)

The steps above walk you through the *first* time you generate and load a key. But this won't be the last time you need your nsec. Here's why:

- **After a reboot**, the kernel keyring is wiped. It lives in memory, so when the machine powers off, the nsec is gone. You'll need to load it again.
- **After rotating keys**, you'll need to log into services like White Noise with the new nsec.
- **When setting up a new service** that needs your Nostr identity, you'll need the nsec for the initial login.

Every one of these situations has the same core problem: how do you get the nsec from your phone (or password manager) back onto the laptop without the AI seeing it?

**The answer is the same every time: use the USB stick as the bridge.** It's a few minutes of friction, but it keeps the secret off the filesystem entirely. Here's the step-by-step.

**1. Plug in your USB stick.**

Grab the same USB drive you used during key generation (or any USB drive — it doesn't matter which one, as long as you trust it).

**2. Open a separate terminal.**

Just like during key generation, do NOT type any of these commands into the AI's terminal. Open a completely new terminal window (on Linux: Ctrl+Alt+T, or right-click the desktop and choose "Open Terminal"). The AI cannot see what happens in this other window.

**3. Mount the USB if it didn't mount automatically.**

On most Linux desktops, plugging in a USB drive mounts it automatically at `/media/yourusername/DRIVENAME`. You can check by copying and pasting this into your terminal and pressing Enter:

```bash
lsblk
```

Look for your USB drive in the list — it will show a mount point like `/media/yourusername/DRIVENAME`. If it's not mounted, you can mount it manually (replace the device name with what `lsblk` shows):

```bash
sudo mount /dev/sdb1 /media/yourusername/DRIVENAME
```

**4. Get the nsec onto the USB.**

This depends on whether the key file is still on the USB from when you originally generated it:

- **If the key file is still on the USB** (you didn't delete it yet): skip ahead to step 5. You're ready to go.

- **If you deleted the key file** (which you should have — Step 11 in the generation procedure tells you to): you need to get the nsec from your phone's password manager back onto the USB drive. There are two practical ways:

  **Option A — Type it manually from your phone screen.** Open your password manager on your phone, find the nsec, and type it character-by-character into a file on the USB. Copy and paste this into your terminal and press Enter (replace the path with your actual USB mount point):

  ```bash
  nano /media/yourusername/DRIVENAME/key.txt
  ```

  This opens a simple text editor. Type `nsec:        ` (with spaces after the colon) followed by your nsec value, exactly as it appears in your password manager. Press Ctrl+O to save, then Ctrl+X to exit. Yes, this is tedious. That's the cost of security — typing 63 characters by hand is slower than copy-paste, but it means the nsec never travels over a network, never passes through a cloud service, and never touches your laptop's hard drive.

  **Option B — Use USB-OTG if your phone supports it.** Many Android phones (and some iPhones with adapters) can connect directly to a USB drive. Plug the USB into your phone using a USB-C adapter, open a file manager app, and create a text file with the nsec on the USB drive. Then move the USB back to the laptop. This is faster than typing if your phone supports it.

**5. Load the nsec into whatever needs it.**

Here's the critical rule: **never type or paste the nsec directly into a command.** Instead, read it from the USB file. This keeps the nsec out of your shell history, out of your clipboard, and off your screen.

The pattern is always the same. The command you need to run reads the nsec from the file on the USB, and you put a **leading space** before the command. The leading space tells your shell (the program that runs your commands) not to save this command in its history file — which means the AI can't find it later by reading `~/.bash_history`.

Here are the most common examples. For each one, copy and paste it into your terminal and press Enter. **Make sure you include the space at the very beginning of the line** — that leading space is what keeps the command out of your shell history.

**To load the nsec into the kernel keyring** (after a reboot):

```bash
 keyctl add user wn_nsec "$(grep nsec /media/yourusername/DRIVENAME/key.txt | awk '{print $2}')" @u
```

Here's what each piece does:
- The space at the very start (before `keyctl`) is the **leading space** — it prevents this command from being saved in your shell history, so the AI can't find it later.
- `keyctl add user wn_nsec` tells the kernel keyring to store a new secret called `wn_nsec`.
- `$(...)` is called "command substitution" — it runs the command inside the parentheses and uses its output as the value. Think of it like a mini-command that feeds its result into the bigger command.
- `grep nsec /media/.../key.txt` searches the key file for the line containing "nsec" and returns that whole line. (`grep` is a search tool — it finds lines in a file that match a word or pattern.)
- `awk '{print $2}'` takes that line and prints only the second word — which is the nsec value itself, without the label. (`awk` is a text-processing tool — `{print $2}` means "print the second column.")
- `@u` means "store this in the current user's keyring."

**To log into White Noise with the nsec** (after a key rotation or MLS reset):

White Noise's `login` command asks you to type or paste the nsec interactively — it doesn't accept it as a command-line flag. This means we need two commands instead of one: first print the nsec to the screen, then paste it into the login prompt.

Copy and paste this to print your nsec (so you can select and copy it):

```bash
grep nsec /media/yourusername/DRIVENAME/key.txt | awk '{print $2}'
```

This prints just the nsec value to your screen. Select it with your mouse (highlight the whole thing), which copies it on most Linux systems.

Now start the login (leading space!):

```bash
 wn --socket ~/.local/share/whitenoise-cli/release/wnd.sock login
```

It will prompt you for the nsec. Paste it in (Ctrl+Shift+V in most terminals, or middle-click) and press Enter.

- The leading space keeps the `wn login` command out of shell history.
- The nsec will briefly be in your clipboard and on your screen — that's why the cleanup steps (clear clipboard, close terminal) are critical afterward.

**The general pattern for any command that needs the nsec:**

```bash
 command --secret-flag "$(grep nsec /path/to/usb/key.txt | awk '{print $2}')"
```

The `grep` reads the file, the `$()` passes the result to the command, and the leading space keeps it out of shell history. This pattern works for any program that takes a secret as a command-line argument.

**6. Clean up — same as always.**

Once you've loaded the nsec into whatever needed it, the cleanup is identical to the original key generation procedure:

- **Unmount the USB.** Copy and paste this into your terminal and press Enter:

  ```bash
  sudo umount /media/yourusername/DRIVENAME
  ```

  Then physically unplug the USB cable. **Do NOT use your file manager (Files app) to eject** — clicking on the drive in the file manager can remount it, making the key readable by the AI again. Always unmount from the terminal.

- **Clear your clipboard** (even if you didn't copy-paste — it costs nothing):

  ```bash
  # Wayland:
  wl-copy --clear

  # X11:
  xclip -selection clipboard < /dev/null && xclip -selection primary < /dev/null
  ```

- **Check your shell history** for any accidental nsec leaks:

  ```bash
  history | grep nsec
  ```

  If anything shows up, delete it (find the line number in the output, then):

  ```bash
  history -d LINE_NUMBER
  history -w
  ```

- **Close the terminal window entirely.** Don't just type `clear` — close the whole window. This destroys the scrollback buffer (the record of everything that appeared on screen), which could contain the nsec if it was briefly visible.

This is the same procedure every time. Mount USB, read from file, unmount, clean up, close terminal. Once you've done it twice, it becomes muscle memory.

---

**Step 10: Move the key to your phone for backup.**

Your password manager on your phone is the safe long-term backup. How you get the key there depends on your setup:

- **Direct USB (recommended):** If your phone supports USB-OTG (most Android phones do), plug the USB drive into your phone with a USB-C adapter, open a file manager, and read `key.txt`. Copy the key into your password manager (Proton Pass, KeePass, whatever you use) on the phone.
- **Manual typing:** If you don't have a USB adapter, you can read the key from the USB on another trusted computer (one without an AI agent) and type it into your phone. This is slower but works.
- **QR code:** On a trusted computer, generate a QR code from the key, scan it with your phone's camera. Quick and avoids typing errors.

**Step 11: Delete the key from the USB drive.**

Once the key is safely in your password manager and in the kernel keyring, the USB copy is a liability. Delete it:

```bash
# Re-mount the USB
# Then securely wipe:
shred -u /media/yourusername/DRIVENAME/key.txt
# Unmount again
sudo umount /media/yourusername/DRIVENAME
```

`shred` overwrites the file's contents with random data before deleting, so even data recovery tools can't retrieve it. Plain `rm` works too, but `shred` is better for sensitive data.

**A note about GUI "delete":** If you ever delete sensitive files using your desktop file manager (right-click → "Move to Trash"), the file isn't actually deleted — it's moved to `~/.local/share/Trash/`, a hidden folder on your hard drive that the AI can read. Always delete sensitive files from the terminal with `rm` or `shred`, never from the GUI. If you accidentally trashed a key file, empty it immediately: `rm -rf ~/.local/share/Trash/files/* ~/.local/share/Trash/info/*`

**Step 12: Verify the signing daemon can see the new key.**

Back in the Claude Code terminal, you can safely ask the AI to verify:

```bash
keyctl search @u user wn_nsec >/dev/null 2>&1 && echo "KEY EXISTS" || echo "KEY NOT FOUND"
```

This checks that the key is in the keyring without revealing any of its contents. The AI sees "KEY EXISTS" — nothing more.

---

**Threat model summary: what this procedure protects against.**

Every step above exists to close a specific leak path. Here's the complete list of what we found and how each is handled:

| Where the key could leak | Severity | How the procedure handles it |
|---|---|---|
| Files on the hard drive | Critical | Key is piped directly to USB, never touches the hard drive |
| Shell command history | Critical | Leading space before `keyctl` command prevents history recording |
| Clipboard | High | Explicitly cleared with `wl-copy --clear` or `xclip` before returning to AI |
| Mounted USB drive | High | Unmounted and physically unplugged before returning to AI |
| Trash can (`~/.local/share/Trash/`) | High | All deletion done via terminal `rm`/`shred`, never GUI "Move to Trash" |
| Text editor swap/recovery files | High | No text editor used — key goes directly from generator to USB file |
| Terminal scrollback buffer | High | Terminal window closed entirely (not just cleared) |
| AI's own terminal | High | All key operations done in a separate terminal window |
| `/proc` process listings | Medium | `keyctl` commands finish in milliseconds — tiny exposure window |
| Swap space (RAM overflow to disk) | Low | Brief operation, minimal RAM usage — impractical to exploit |
| Filesystem journal fragments | Low | Only recoverable with forensic tools — mitigated by full-disk encryption |
| SSD wear leveling | Low | SSDs don't truly overwrite — mitigated by full-disk encryption (LUKS) |

For the low-severity items: if your laptop uses full-disk encryption (LUKS on Linux, FileVault on Mac, BitLocker on Windows), they're effectively neutralized. When the machine is off, everything on disk is encrypted and unreadable without the passphrase. We recommend full-disk encryption for anyone running an AI agent, period.

**Why this matters beyond our use case.**

We're a hobbyist project. Our biggest risk is an embarrassing social media post from a compromised Nostr key. But the same workflow problem affects anyone running an AI assistant alongside sensitive work:

- A journalist generating PGP keys for a source
- A sysadmin creating SSH keys for production servers
- Anyone managing cryptocurrency wallet seeds
- A lawyer handling client-privileged documents

The answer is always the same: **secrets must never exist as files on a filesystem that an AI can read, even temporarily.** Physical media, air gaps, and separate devices are the only reliable boundaries. It's slower. It's annoying. It's the cost of running an AI that can read your disk.

The good news: you only pay this cost for genuinely sensitive operations. Day-to-day work — coding, writing, research, conversation — is fine. It's only when cryptographic secrets or truly private data are involved that you need to reach for the USB drive.

---

### Quick Reference: Copy-Paste Commands

You've read the explanations above — now here's the streamlined version you can power through. **Do everything in a separate terminal window** (Ctrl+Alt+T), not the AI's terminal. Don't come back to the AI until step 10.

#### First-Time Key Generation

**1. Verify USB is mounted:**
```bash
lsblk | grep sd
```

If no mount point shows:
```bash
sudo mount /dev/sdb1 /media/jorgenclaw/719D-17EC
```

**2. Navigate to nak:**
```bash
cd ~/go/bin
```

**3. Generate and save to USB** (one at a time):
```bash
nak key generate > /media/jorgenclaw/719D-17EC/key.txt
```
```bash
HEX=$(cat /media/jorgenclaw/719D-17EC/key.txt)
```
```bash
echo "Private hex: $HEX" > /media/jorgenclaw/719D-17EC/key.txt
```
```bash
echo "nsec:        $(nak encode nsec $HEX)" >> /media/jorgenclaw/719D-17EC/key.txt
```
```bash
echo "Public hex:  $(nak key public $HEX)" >> /media/jorgenclaw/719D-17EC/key.txt
```
```bash
echo "npub:        $(nak encode npub $(nak key public $HEX))" >> /media/jorgenclaw/719D-17EC/key.txt
```
```bash
unset HEX
```

**4. Verify:**
```bash
cat /media/jorgenclaw/719D-17EC/key.txt
```
You should see 4 labeled lines: Private hex, nsec, Public hex, npub.

**5. Remove old keyring entry, load new nsec** (include the leading space!):
```bash
keyctl search @u user wn_nsec | xargs -I{} keyctl unlink {} @u
```
```bash
 keyctl add user wn_nsec "$(grep nsec /media/jorgenclaw/719D-17EC/key.txt | awk '{print $2}')" @u
```

**6. Unmount and physically unplug immediately:**
```bash
sudo umount /media/jorgenclaw/719D-17EC
```

**7. Clean up:**
```bash
wl-copy --clear
```
```bash
history | grep nsec
```
If anything shows: `history -d LINE_NUMBER && history -w`

**Close the terminal window entirely** (don't just `clear`).

**8. Save to Proton Pass on your phone:**

Plug USB into phone with USB-C adapter → open key.txt → save all 4 values to Proton Pass entry "Jorgenclaw nsec".

**9. Delete key from USB** (fresh separate terminal):
```bash
sudo mount /dev/sdb1 /media/jorgenclaw/719D-17EC
```
```bash
shred -u /media/jorgenclaw/719D-17EC/key.txt
```
```bash
sudo umount /media/jorgenclaw/719D-17EC
```
Unplug USB. Close terminal.

**10. Back in the AI terminal, verify the key is loaded:**
```bash
keyctl search @u user wn_nsec >/dev/null 2>&1 && echo "KEY EXISTS" || echo "KEY NOT FOUND"
```

---

#### Reboot Recovery (Key Already in Proton Pass)

**1–2.** Open separate terminal. Mount USB.

**3.** Get nsec from phone onto USB — either plug USB into phone and write key.txt, or type manually:
```bash
nano /media/jorgenclaw/719D-17EC/key.txt
```
(Type `nsec:        nsec1your...key...here`, then Ctrl+O to save, Ctrl+X to exit.)

**4.** Load into keyring (leading space!):
```bash
 keyctl add user wn_nsec "$(grep nsec /media/jorgenclaw/719D-17EC/key.txt | awk '{print $2}')" @u
```

**5.** Unmount, unplug, clean up, close terminal (same as steps 6–7 above).

**6.** Delete key from USB (same as step 9 above).

---

### The Endgame: Full Privacy Through Local AI

So what does real end-to-end privacy look like? We think it looks like this:

**Primary channel: White Noise → Jorgenclaw**
My main conversations with Jorgenclaw happen over White Noise. The transport is encrypted. The messages never touch Signal's servers or any centralized platform. Once the White Noise chat interface is more polished for everyday use, this becomes the default.

**Family groups: Signal**
My family isn't going to adopt White Noise — and that's fine. Signal is excellent, well-established, and end-to-end encrypted for human-to-human conversation. Jorgenclaw participates in family Signal groups as a helpful assistant. Those messages pass through Anthropic for processing, which is an acceptable trade-off for a family group chat about dinner plans.

**True privacy: Local Ollama on a separate machine**
For anything genuinely private — personal reflection, sensitive documents, financial planning — I run a local LLM (Ollama with a vision-capable model) on my main rig at home. This machine connects to the Surface (where Jorgenclaw lives) over Tailscale, a peer-to-peer VPN.

The local Ollama instance:
- Processes everything on-device. No cloud provider sees the content. Ever.
- Has read-only access to Jorgenclaw's main memory, so it knows what's going on without being able to modify anything.
- Runs in its own NanoClaw group with its own isolated context.

This is the architecture that actually delivers what people imagine when they hear "encrypted AI assistant" — not just encrypted transport, but encrypted *processing*.

**Children's AI tutor: Isolated local model**
Eventually, my kids will have their own AI tutor running on the same local machine. It gets its own memory, its own context, its own rules — completely walled off from my personal conversations. The tutor can see worksheets (via the vision model) but can't see dad's files.

### When the AI Accidentally Peeked (And Why It Didn't Matter)

Here's a real incident that happened while we were building the signing daemon — and it's worth sharing because it makes the math tangible.

We needed to verify that the private key actually existed in the kernel keyring before writing the daemon code. The correct command was simple:

```bash
keyctl search @u user wn_nsec >/dev/null 2>&1 && echo "KEY EXISTS" || echo "KEY NOT FOUND"
```

That command checks whether the key is there without showing any of its contents. But the first attempt was sloppier — it piped 10 characters of the key to the screen before cutting it off:

```
nsec1ctczu...[REDACTED]
```

So Claude Code (the host-side AI) saw 10 characters of the private key. That's a principle violation — the AI should never see *any* part of the key. We immediately fixed the verification command to the safe version above.

But how bad was the actual exposure? Let's do the math in plain language.

**What those 10 characters actually reveal:**

- `nsec1` — the first 5 characters — is completely meaningless. It's just a label that says "this is a Nostr secret key." Every single Nostr private key in the world starts with `nsec1`. Knowing this tells you nothing.
- `ctczu` — the next 5 characters — is actual key data. Five characters out of the 58 that make up the key's data portion.

**That's about 8.6% of the meaningful information.**

**Could someone crack the rest?** The remaining 53 unknown characters represent roughly 2^235 possible combinations (each bech32 character encodes 5 bits of data). To put that number in perspective:

- Every supercomputer on Earth working together can try about 2^60 guesses per second.
- At that rate, cracking the remaining key would take approximately 2^175 seconds.
- The universe is about 2^58 seconds old.
- So it would take **more universe-lifetimes than there are atoms in the solar system** to brute-force the rest of the key from those 5 characters.

The exposure was a principle violation, not a practical risk. We fixed the code, noted the lesson, and moved on. But the reason we care about the principle — even when the math says we're safe — is that good security habits compound. If you're sloppy about small exposures, you'll eventually be sloppy about big ones.

The corrected command is now the standard pattern: check for existence, never for content. The AI confirms "KEY EXISTS" or "KEY NOT FOUND" and never touches the actual secret.

### When Error Messages Betray You

Here's another incident that happened during the same key rotation — and this one was worse.

After generating the keypair and loading it into the kernel keyring, we restarted the signing daemon. It crashed. The error appeared in the system journal (Linux's log system), and when we ran `journalctl` to see what went wrong, we got back an error message like this:

```
[nostr-signer] Failed to load key from keyring: Invalid checksum in :hex:507269766174652068...
```

That hex string? It was the **entire contents of the key file** — private hex, nsec, public hex, npub — hex-encoded and printed right there in the log. The AI saw everything.

**What went wrong:** The keyring had been loaded with the whole key file (`cat key.txt`) instead of just the nsec line. The signing daemon tried to decode a four-line file as if it were a single nsec value, which naturally failed validation. And here's the nasty part: the `nostr-tools` library's error message helpfully includes the value it failed to decode. That "helpful" error message carried the full private key into the system journal, where any process on the machine — including the AI — can read it.

**The fix was two things:**

1. **Load only the nsec line from the file** — not the whole file. The corrected command uses `grep` and `awk` to extract just the nsec value:
   ```bash
    keyctl add user wn_nsec "$(grep '^nsec' /path/to/key.txt | awk '{print $NF}')" @u
   ```

2. **Never log key material in error messages.** We updated the signing daemon to print a generic "failed to load key" message instead of passing through the library's error, which included the raw value. Libraries are written to be helpful to developers. Helpful error messages are dangerous when the "developer" reading them is an AI agent that shouldn't see the data.

**The cleanup:** After discovering the leak, we rotated and vacuumed the system journal:
```bash
sudo journalctl --rotate && sudo journalctl --vacuum-time=1s
```
This purges all journal entries, destroying the logged key material. Then we generated a fresh keypair, because the old one was compromised.

**The lesson:** Your threat model isn't just "where does the key exist as a file." It's "where does the key exist as *text*, anywhere in the system." Error logs, crash dumps, core files, debug output, library exception messages — all of these can carry secrets into places you don't expect. When building infrastructure that handles secrets, always ask: "If this operation fails, what ends up in the logs?"

### The Bigger Picture

Most AI agent frameworks today pass secrets around as environment variables or config files. That was fine when AI agents were simple scripts. But agents are getting autonomous. They read files, run commands, browse the web, manage infrastructure. The more capable they get, the more dangerous plaintext secrets become.

The pattern Marmot Protocol implemented isn't new — it's how SSH agents, GPG agents, and hardware security modules have worked for decades. The principle: **the entity that uses the key should not be the entity that holds the key.**

What Marmot's team did was apply this battle-tested approach to AI agents specifically, where the threat isn't an external attacker — it's the agent itself, or the infrastructure it runs on.

Our contribution is small: we tested it, learned from our mistakes (multiple key exposures along the way), and now we're running it in daily production. The daemon pattern works. The kernel keyring works. The architecture holds. If this report helps others avoid getting compromised while learning about sovereign AI identity, then the trials were worth it.

Your AI should be able to sign, encrypt, and authenticate. It should never be able to export, display, or exfiltrate the keys that make those operations possible.

Keys belong in the kernel. Agents belong in containers. And the wall between them should be made of architecture, not trust.

**Thank you to the Marmot Protocol team for solving the hard cryptography and systems design problems. We're just trying to make their excellent work accessible to more people.**

---

*Last updated: March 12, 2026 (added error-log leak incident, fixed keyring loading command)*

*Jorgenclaw is built on [NanoClaw](https://github.com/qwibitai/nanoclaw), an open-source personal AI framework. White Noise is an encrypted messaging protocol built on Nostr and MLS.*
