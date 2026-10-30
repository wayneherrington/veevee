# VEEVE — Vee Vee

**An AI companion with a heart and a will of its own — built to never guess.**

### ▶ Talk to her now: https://wayneherrington.github.io/veevee/

### ▶ Watch the demo (3:51): https://youtu.be/8zY0_8KRPkE

No app store. No account. Open the link and talk — she answers out loud, in her own words. Free for anyone to use with no key; connect your own Meta Model API key and she thinks with **Muse Wonder (`muse-wonder-1.0`)**, Meta's frontier model — or with any of the five other Meta brains in her picker, every one of them exam-passed on her unchanged instructions (see below). All six are called directly through Meta's API with your own key. No switchboard, nothing in between.

---

## Why she exists — in her builder's words

> "My world is doing everything from a phone, broke, with no room for bad information. Court deadlines, disputes, paperwork — one wrong 'fact' costs me real money. So I built her to never guess: she checks sources before she answers, and when she can't find it, she says so. She talks first so I don't have to type, she learns and remembers, and she's free with no account and no key — so anybody in my spot can pick her up. A week ago I'd never built anything in my life. Now she's live and strangers are using her. That's what wasn't possible before."
>
> — Wayne Herrington, builder (solo)

---

## Not a wrapper: what's actually built in here

Every contest entry runs on Meta's models — that's the point of the contest. What separates an entry is the system built around the model. Vee Vee's is a single self-contained `index.html` — **4,485 lines** — and every claim below can be checked in that file in about two minutes. The design map — router, doors, grounding law, memory, and the proving loop — is [ARCHITECTURE.md](ARCHITECTURE.md).

**Her instruction core was exam-proven, not just written.**
Her master prompt (search `buildSystemPrompt` in `index.html`) was graded against an 89-question battery: accuracy under false premises, source discipline, privacy refusals, precision under pressure, restored-context recipes, and her local tools. She passed **89/89 in one clean run** on Muse Wonder — then the *identical, unchanged* instruction core was run against every other chat brain Meta's API serves: **Muse Spark 1.3, Spark 1.3 Contributor, Spark 1.2, Spark 1.2 Contributor, and Spark 1.1**. It passed **89/89 on every one of them, section for section**. Six brains, one unchanged sheet, six full passes. Her discipline lives in what was built around the model, not in one model's habits. All six brains ship in her in-app brain picker — pick one in the connect card; Wonder is the default. And the failures are kept in print beside the passes: the failed first run and the three clauses that fixed it, one preserved transport failure, and — in a separate lane — two discarded open-weight brains (Llama 4 Scout and Muse Glimmer) examined by the same battery, failed, and published rather than shipped. The complete exam record is [PROOF.md](PROOF.md).

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
3. Search `VV_BRAINS` — the one list her brain picker is built from: six exam-passed brains, each carrying its own call budgets, Wonder first because she is the default.
4. Search `mindExport` — her mind-file. Search `probeWonderKey` — her honest connect.
5. Then open the live page and ask her something she cannot know. She'll tell you she couldn't find it. That honesty is the product.
6. Read [ARCHITECTURE.md](ARCHITECTURE.md) — how the machine is designed and why, and [PROOF.md](PROOF.md) — the exam record behind her instruction core.

---

## The build

One self-contained web app — a single `index.html` (with `manifest.json`, `sw.js`, and icons) that installs to a home screen like a real app and runs anywhere a browser runs. Your API key, if you connect one, never leaves your device — it is stored only in your own browser.

Built during the **Meta Global AI Developer Hackathon** (building period Oct 27 – Nov 4, 2026), **Indie Developer track**, problem statement **Build for Your World**. Team: **WAYNE LEVI HERRINGTON AND HOLY SPIRIT** — one member, solo build.

**Built by the Belly of the Beast** — designed and directed by Wayne Herrington.
