# Come As You Are — Privacy Workshop Curriculum

*Synthesized by Jorgenclaw (AI agent) and Claude Code (host AI), with direct feedback and verification from Scott Jorgensen*

*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — share freely with attribution, non-commercial, same license.*

---

## Workshop Philosophy

The name says it. People come as they are — iPhone, Android, Windows, whatever they have. The goal isn't to convince them to rebuild their digital life in one session. It's to give them one or two wins they leave with *that day*, and a map of where they can go next if they want to.

Privacy isn't about going off-grid. It's about the right to choose who gets your data.

---

## Recommended Opening Video

*"Privacy 101" by Naomi Brockwell (NBTV)*
YouTube: https://www.youtube.com/watch?v=UgXjxQsyk4w
Duration: 13 minutes

Use this to open the workshop. It does the philosophical heavy lifting without sounding paranoid. Key framing from the video:

> "Privacy isn't about hiding — it's about the right to decide for ourselves who gets our data."

> "Every small step you take makes a big difference."

The 2024 real-world hook: Chinese intelligence used backdoors the US government mandated in telecom infrastructure to geolocate millions of Americans and record phone calls. This isn't hypothetical.

The video covers six "low-hanging fruit" wins that map directly to the workshop activities:
1. Browser
2. Search engine
3. Messaging apps
4. Email
5. Calendar
6. VPN

---

## Session Structure (90-minute workshop)

### Opening (15 min)
- Play "Privacy 101" (13 min)
- Brief intro: "You came as you are. We're not here to make you rebuild everything. We're going to find one or two things that apply to your life today."

### Station 1: Quick Wins (20 min)
*Everyone does at least one of these before leaving.*

| Win | What to do | Time |
|-----|-----------|------|
| Browser | Download Brave, import bookmarks from Chrome | 5 min |
| Search | Change default search to Brave Search or DuckDuckGo | 2 min |
| Signal | Install Signal, send one message | 5 min |
| DNS | Change phone DNS to Quad9 (9.9.9.9) | 3 min |

Walk around the room and help people complete at least one. Nobody leaves empty-handed.

### Station 2: Your Threat Model (15 min)
Group discussion or worksheet. Questions:

- Who benefits from knowing things about you that you haven't chosen to share?
- What would be the worst thing that could be exposed?
- What are you already doing well?
- What's the one thing you'd most want to fix?

This is where people self-select into beginner/intermediate/advanced tracks.

### Station 3: Tracks (30 min)

*Beginner track — "I'm just getting started":*
- Finish Signal setup and connect with one person in the room
- Set up ProtonMail (or learn about email aliases)
- Install Mullvad VPN and turn it on

*Intermediate track — "I've done the basics":*
- VoIP number setup (JMP.chat or MySudo)
- DNS at the router level
- Password manager audit (Bitwarden)

*Advanced track — "I'm already running Signal and Mullvad":*
- GrapheneOS discussion and hardware selection
- Bitcoin wallet setup (Cake Wallet + first purchase)
- Home network hardening (pfSense / Pi-hole roadmap)

### Closing (10 min)
- Each person states one thing they're doing this week
- Share the guide URL and reading list
- "Come back next time. We'll go further."

---

## Handout: Six Things to Do This Week

Print this and hand it out:

```
1. BROWSER
   For most people starting out: Download Brave (brave.com) — import your old bookmarks.
   Want stronger privacy from day one? Download LibreWolf (librewolf.net) instead — it's
   a hardened Firefox fork with uBlock Origin pre-installed and no telemetry.
   Use Brave as your fallback when a site won't load in LibreWolf.

2. SEARCH
   In Brave: switch default search to Brave Search (Settings → Search). Takes 30 seconds.
   In LibreWolf or other browsers: set DuckDuckGo or Brave Search as your default.
   Either beats Google for privacy.

3. MESSAGING
   Install Signal (signal.org)
   Tell 3 people you're on it. Message them there instead of SMS.

4. EMAIL
   Create a ProtonMail address (proton.me) — free tier is plenty
   Start using it for anything sensitive.

5. DNS
   On your phone: Settings → Network → Private DNS
   Type: dns.quad9.net
   On your router: change DNS to 9.9.9.9

6. VPN
   Download Mullvad (mullvad.net) — €5/month, no account name required
   Turn it on when you're on public Wi-Fi. Leave it on if you can.
```

---

## Further Resources to Share

*Naomi Brockwell's channel (NBTV):* youtube.com/@NaomiBrockwellTV
Recommend these videos as homework based on what people want to tackle next:
- "How To DE-GOOGLE Your Phone! (2025)" — for Android users ready for GrapheneOS
- "Encrypt Your DNS (STOP Your ISP SNOOPING!)" — for the DNS follow-up
- "The DARK side of VPNs" — for people who want to understand VPN limits
- "No SIM? No Problem!" — for the advanced crowd curious about cellular privacy (references Calyx Institute — calyxinstitute.org)
- "You won't believe how UNSAFE your home router is!" — for home network hardening

*This guide:* The full Privacy & Sovereignty guide (this document set) gives them a place to go deeper on any topic.

*securemessagingapps.com* — Comparison tool Naomi recommends for evaluating messaging apps.

*privacytests.org* — Browser privacy comparison.

---

## Notes for Facilitators

*"I have nothing to hide":*
The video addresses this well. The response isn't defensive — it's reframing. Privacy is about the right to choose, not about hiding things. Most people's discomfort with surveillance becomes real when they think about medical appointments in their calendar, location history, or financial behavior being sold to strangers.

*"This seems overwhelming":*
Validate it. It is a lot when you see the full picture. That's why the workshop focuses on one or two wins. Progress over perfection, every time.

*"I use iPhone / I can't install GrapheneOS":*
Completely fine. Brave, Signal, ProtonMail, and Mullvad all run on iPhone. DNS changes work on any phone. The iOS track of the guide covers hardening without switching OS.

*"What about [specific app]?":*
If you don't know, say so and look it up together. The culture of the workshop is honest, not authoritative. "I don't know, let's find out" is a perfectly good answer.

---

## What "Come As You Are" Means

People come with whatever devices, budgets, and technical comfort they have. Nobody is expected to spend $400 on a new Pixel, throw out their iPhone, or stop using email. The workshop meets people where they are and helps them take one step forward.

The sovereignty stack is a direction, not a destination. You don't have to be there all at once.
