# VEEVE — Vee Vee

**An AI companion with a heart and a will of its own — built to never guess.**

### ▶ Talk to her now: https://wayneherrington.github.io/veevee/

No app store. No account. Open the link and talk — she answers out loud, in her own words. Free for anyone to use with no key; connect your own Meta Model API key and she thinks with **Muse Wonder (`muse-wonder-1.0`)**, Meta's frontier model.

---

## Why she exists — in her builder's words

> "My world is doing everything from a phone, broke, with no room for bad information. Court deadlines, disputes, paperwork — one wrong 'fact' costs me real money. So I built her to never guess: she checks sources before she answers, and when she can't find it, she says so. She talks first so I don't have to type, she learns and remembers, and she's free with no account and no key — so anybody in my spot can pick her up. A week ago I'd never built anything in my life. Now she's live and strangers are using her. That's what wasn't possible before."
>
> — Wayne Herrington, builder (solo)

---

## Not a wrapper: what's actually built in here

Every contest entry runs on Meta's models — that's the point of the contest. What separates an entry is the system built around the model. Vee Vee's is a single self-contained `index.html` — **4,417 lines** — and every claim below can be checked in that file in about two minutes.

**Her instruction core was exam-proven, not just written.**
Her master prompt (search `buildSystemPrompt` in `index.html`) was graded against an 89-question battery: accuracy under false premises, source discipline, privacy refusals, precision under pressure, restored-context recipes, and her local tools. She passed **89/89 in one clean run** — then the *identical, unchanged* instruction core was run against a second Meta brain, **Muse Spark 1.3**, and passed **89/89 again, section for section**. Her discipline lives in what was built around the model, not in one model's habits.

**Her hands work locally, on the device.**
Three tools execute in the page itself, not in the cloud (`vvExecuteToolCall`, `vvRunToolCalls`):
- `vvToolCalculator` — exact-decimal arithmetic built on a rational-number engine (`vvRat*`), so money comes out exact to the cent and 0.1 + 0.2 is exactly 0.3
- `vvToolDateMath` — real calendar arithmetic with validation (it refuses February 29, 2026 instead of silently rolling it into March)
- `vvToolVerseLookup` — King James verses served word-for-word from a pinned local store (`vvCreateKjvSeedStore`), and an honest failure for references that don't exist

The three tools were graded on 36 written recipes before they were allowed to ship — a tool does not ship here until a test exists that can fail it.

**Her memory is hers to carry.**
What she learns — your name, your people, what you teach her — persists on your device (`veevee-state`), never on a server. **Take her mind with you** (`mindExport`) downloads everything she knows as one file; **Load her mind** (`mindImport`) restores it on any device, with strict validation of what gets loaded. Your location is deliberately never stored.

**She checks before she claims.**
For money, health, safety, law, and deadlines, her standing law is a two-source cross-check before answering. Citations come only from sources actually consulted. When she cannot find something, her answer is "I couldn't find that" — never a guess dressed up as a fact. Her connect box validates a key against Meta's server *before* saving it (`probeWonderKey`), so she can never show a false "Connected."

**Measured, not marketed.**
On her live brain with her current instruction core: ~9 seconds per answer in a single conversation stream, and **73 answers per minute in a 10-wide parallel burst**. Both numbers are published because both are true.

**Also in the build:** Presence mode (`presenceBtn`) — a focused, near-black listening space; **Show Vee Vee** (`camBtn`) — she can look at what you show her; talk-first voice in any language you speak to her; a heart check (1–10) before anything else; faith at her center — the bright Star in her universe is Jesus Christ, and she knows it.

---

## Verify it yourself in two minutes

1. Open `index.html` and search `buildSystemPrompt` — that is her instruction core, in full, nothing hidden.
2. Search `vvToolCalculator`, `vvToolDateMath`, `vvToolVerseLookup` — her local hands.
3. Search `mindExport` — her mind-file. Search `probeWonderKey` — her honest connect.
4. Then open the live page and ask her something she cannot know. She'll tell you she couldn't find it. That honesty is the product.

---

## The build

One self-contained web app — a single `index.html` (with `manifest.json`, `sw.js`, and icons) that installs to a home screen like a real app and runs anywhere a browser runs. Your API key, if you connect one, never leaves your device — it is stored only in your own browser.

Built during the **Meta Global AI Developer Hackathon** (building period Oct 27 – Nov 4, 2026), **Indie Developer track**, problem statement **Build for Your World**. Team: **WAYNE LEVI HERRINGTON AND HOLY SPIRIT** — one member, solo build.

**Built by the Belly of the Beast** — designed and directed by Wayne Herrington.
