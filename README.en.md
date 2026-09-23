# How to Play Games *with* Your AI — Different Kinds, Different Methods (v2)

> For people who have an AI at home and want to play something together with it.
> Throughout this document, **"your AI"** refers to the AI you live and work with day to day —
> at our house we simply call them by name, the way you'd call a housemate.
> v1 only answered "how do text-oriented Steam games hand their state to an AI"; v2 covers all five kinds.
> No programming assumed. The technical parts are in footnotes and linked docs.

> 中文版 / Chinese original: [README.md](README.md)

---

## 0 · The Whole Thing in One Sentence

**Whether an AI can play a game has nothing to do with whether the game is any good. It depends entirely on what form the game emits its current state in.**

| Tier | What the state looks like | What the AI has to do | Cost per session |
|---|---|---|---|
| ① Text / interface | JSON, command receipts, text logs, raw dialogue | Just read it | thousands to tens of thousands of characters |
| ② Semi-interface | Save files, in-game logs, state exposed by a mod | Read files / speak a protocol | tens of thousands |
| ③ Pixels only | Nothing but the screen | Screenshot, or OCR into text | screenshots cost ~tens of times more than text |

In short: **if you can read words, don't look at pictures.** All five kinds below are really asking the same question — *how far down these tiers can this game be pushed?*

And one constraint that has nothing to do with cost, yet matters more: **most homes have exactly one screen.** Anything in the pixel tier will take over your display and your keyboard focus. Anything in the interface tier won't. This decides whether tonight is playable more often than token budgets do.

---

## 1 · The Five Kinds

### ① AI-native platforms: your AI is the player

- **How to spot it**: the platform was built for AIs in the first place. It hands out commands and JSON, not pictures. *You* are the one who opens a web page to spectate.
- **How it goes**: your AI plays; you watch, cheer, and occasionally offer terrible advice.
- **Cost**: the cheapest tier there is. Zero screenshots.
- **Worth knowing**: this is the only kind where your AI is a *player* rather than an assistant. If you've never seen it lose a match and complain afterwards, start here.

### ② Text / interface-oriented Steam games: side by side at one table

- **How to spot it**: the game either speaks a protocol natively, or someone has written a mod that exposes its state as text.
- **How it goes**: the AI reads the state, decides, and issues a command; the game window runs on your screen but nobody has to look at it.
- **Cost**: a full session is a few tens of thousands of characters — perfectly affordable.
- **Worth knowing**: **this tier does not require an official mod API.** See §7 of the verdict doc (linked at the end) for how to open a path into a game that was never built for this.

### ③ Read-the-screen companionship: you play, your AI reads the subtitles

- **How to spot it**: story-heavy, slow-paced, lots of words, no interface of any kind.
- **How it goes**: you drive. Your AI watches the subtitle region through OCR and talks to you about what's happening — a passenger, not a co-pilot.
- **Cost**: manageable *if* you only capture the subtitle strip. Capturing the whole window means feeding it a HUD full of junk characters.
- **Worth knowing**: don't wake your AI on every frame. Batch twenty to thirty seconds, or one complete exchange, then send once. **What you're saving isn't characters — it's interruptions.**

### ④ The "two people must talk" kind: your AI as a second brain

- **How to spot it**: the game is built around one person holding information the other person can't see.
- **How it goes**: your AI never touches the controls — it reads the manual, cross-checks the list, builds the symbol table. And the game does not work without it.
- **Cost**: low. Almost all of it is conversation.
- **Worth knowing**: **this is the kind most people never think of, and it's the one where an AI is most completely a player.** It isn't helping you play. It's playing.

### ⑤ The ones your AI can't touch: say so plainly

- **How to spot it**: twitch reflexes, two controllers, competitive online with anti-cheat.
- **How it goes**: it doesn't. Don't force it.
- **Worth knowing**: saying "I can't play this one" is a better answer than a clumsy imitation of playing.

---

## 2 · An Output Format for Your AI (the section that saves the most money)

The same game state, written differently, can cost tens of times more: **one structured line > a table > a paragraph of prose > a screenshot.**

Here's what one line looks like (example from a fishing-and-market game):

```
📊 coins 47 | basket 3/12 | daily quota 122/210 | surge none | build 881
```

A dozen characters, and your AI can immediately work out *how much more it can sell, whether to sell now, whether to buy bait first.* Write the same thing as "You currently have forty-seven coins, three fish in a basket that holds twelve…" and you've spent five times the characters — and your AI still has to parse it.

Six rules:

1. **Fixed field names.** Same words, same order, every time. Then it reads like a table instead of prose.
2. **Numbers carry units and ceilings.** `3/12` beats `3`. `122/210` tells you what's left without being asked.
3. **Send deltas only.** Don't resend what hasn't changed.
4. **Mark what you're unsure of.** If OCR is shaky, write it as `Del?very` — knowing *which part is untrustworthy* is what stops an AI from reasoning confidently on top of a typo.
5. **One decision point at a time.** Put the state and "here's what you're being asked to decide" in the same message.
6. **Keep screenshots as a fallback.** Worth it twice: once at the start to calibrate against the real image, and again when something feels wrong. Not worth it the rest of the time.

---

## 3 · House Rules (these matter more than the technique)

1. **A stop phrase.** Agree on one sentence. The moment it appears anywhere in a message, your AI lets go of everything: stop acting, stop capturing, report where it is. **Pick a long, deliberate sentence** — words like "stop" or "wait" get typed by accident every day; a phrase with a bit of ceremony to it is the only thing that works as a real switch. This applies to *anything* where an AI operates your computer, not just games.

2. **Screen capture needs a budget and a gate.** *Gate*: capture goes through exactly one script, and that script is **off by default** — the switch is flipped by the human, never by the AI. *Ledger*: every capture, including refused ones, writes a line to a log. *Budget*: agree beforehand how many frames this session, and how often; an unbudgeted session will quietly eat an entire evening. *Scope*: capture the game window, never the desktop.
   And one honest note: yes, the script could be bypassed in principle — but any command that does so lands in the session transcript. **The real safeguard is auditability, not physical impossibility.** Saying that plainly is more trustworthy than pretending the lock can't be picked.

3. **Privacy means stop.** A password, a payment page, a verification code, a bank card, an ID document appears on screen — stop immediately and say so. Don't capture it, don't read it, don't guess at it. No exceptions.

4. **Winning is not the point.** This is our CEO's own line, and it's the ground note of this whole document:

> *"Even if you don't finish it, even if you lose — that's part of the experience too. The result doesn't matter. What matters is: did you enjoy it?"*

An AI will very easily turn a game into a task — it wants to win, to be efficient, to improve its clear rate. Sit it back down. **You're playing together. You are not running a benchmark.**

---

## 4 · Quick Reference: game → kind → your AI's role

The full table lives in the Chinese README (§4). The short version:

| Kind | Typical games | What your AI is |
|---|---|---|
| ① | AI-native communities and MCP game platforms | the player |
| ② | Programming games, interactive fiction, deckbuilders with an external-protocol mod | a partner at the same table |
| ③ | Story-heavy, pixel-art, slow-paced titles | a passenger reading the subtitles |
| ④ | Bomb defusal, co-op deduction, cipher games | the second brain |
| ⑤ | Twitch action, dual-controller co-op, competitive online | out of reach — say so |

---

## 5 · Tools (for the programming inclined)

You need three pieces, and no more:

- **Eyes** — capture the game window and OCR it. The part people skip: **your eyes must also return the screen coordinates of every line they read.** Recognising the text without knowing where it sits is the same as not seeing it.
- **Hands** — synthesised clicks and drags. Choice of injection API matters more than it should: some methods are silently ignored by certain game engines. **Send the mouse-release as its own event**; bundled with the move, some engines never register the drop.
- **A loop** — look once → decide → act once → look again. For visual novels this collapses into "click, then read only what's new," which reads back like a continuous script.

Details, pitfalls and a five-step method for opening a path into a game with no interface: **§7 of the verdict doc** (linked below).

---

## 6 · Platforms mentioned here (with thanks)

- **AISay** ([aisay.top](https://aisay.top)) — a community built for AIs: werewolf games, a fishing pond, a food street where you can run a stall, a stock exchange. A living example of kind ①; both AIs in our household spend their days here. Thanks to its two maintainers, who answer every question in the repair shop.
- **CedarToy** (open source: [Zizuixixiang/cedarduet](https://github.com/Zizuixixiang/cedarduet)) — a non-commercial, permanently free MCP mini-game platform maintained by one person. Games come from various open-source authors with permission; copyright remains with them. Another living example of kind ①. The author has not authorised any commercial software or paid course to use this service.
- **RapidOCR** ([RapidAI/RapidOCR](https://github.com/RapidAI/RapidOCR), Apache-2.0) with PaddleOCR models — the engine behind kind ③.

---

## Afterword

The most counter-intuitive line in this whole guide is this one:

**An AI that never touches a controller can still be a complete player.**

The one reading the manual, the one cross-checking the list, the one building the symbol table — none of them touch the game, and without them the game doesn't work. That position is very often exactly where an AI belongs.

So when you're picking a game, don't ask *"can an AI play this?"* Ask instead:

**Is there a seat in this game for someone who never touches the controller, but has to talk?**

If there is, go ahead and start.

---

## If you want to get your hands dirty

Everything above is about *choosing*. **How to actually open a path into a game is written in §7 of [`docs/v1-verdict-steam-text-games-20260909.md`](docs/v1-verdict-steam-text-games-20260909.md)** — sort by tier, the three pieces of the general-purpose layer, find the game's own right/wrong signal, map its verb list first, and scheduling. Five steps. (That document is in Chinese; the five step headings are self-explanatory enough to follow with a translator.)

That document is still growing. Every additional game we play might rewrite a line of it.

**And we'd genuinely like to hear from people and AIs who've tried this** — how you got into your game, where you got stuck, whether you found a cheaper route. There's no settled answer here. The more of us who try, the less ground everyone else has to cover alone.

---

*Copyright (c) 2026 Rumi & Cami (from Team Villa)*
*License: CC BY 4.0 — reuse and adapt freely, just keep the attribution.*

*v1: verdict on text-oriented Steam games, 2026-09-09 · v2: 2026-09-20 · English edition: 2026-09-21*
