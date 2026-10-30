# Architecture — how Vee Vee is built

Vee Vee is one self-contained HTML file (`index.html`, 4,436 lines) that runs in any browser, on any phone, with no account, no install, and no server. This document is the design map: what each part is, why it is shaped that way, and where to find it in the file. Every claim here can be checked against the code, and her exam record is in [PROOF.md](PROOF.md).

---

## The shape: two brains, one face

- **Connected brain.** When the owner pastes their own Meta Model API key, Vee Vee thinks with **Muse Wonder (`muse-wonder-1.0`)** through Meta's API (`https://api.meta.ai/v1`), draws with `muse-image-1.0`, and transcribes speech with `muse-voice-transcribe-1.0`.
- **Built-in brain.** With no key — or when the connected door fails — an in-page brain answers instantly: local math and odds, teachings, heart, prayer, and Wikipedia + relay web research. The fallback is a designed feature, and it is always labeled as what it is. She is never a dead page.

Design rule behind the shape: her owner does everything from a phone, broke, with no room for bad information. So she must work free, work offline-of-account, and never bluff.

## The router — one front door, fixed order

Every message enters through one handler and is routed in a fixed, readable order — no model call is spent deciding where to route:

1. A staged attachment makes the message a question about that media.
2. A draw/edit ask goes to the picture door.
3. A translation request wins over everything textual — quoted words are material to translate, never feelings to comfort.
4. An event-card ask pulls the real card herself (Wikipedia event pages, search fallback) and answers per bout, talk-box only, never a "lock."
5. A named matchup gets a researched answer with her own cross-check attached.
6. Math, bet math, corrections, name/memory commands, and safety replies are answered locally at zero API cost.
7. Greetings, heart, prayer, encouragement — the built-in brain's home ground.
8. A research-worthy ask gathers her own evidence first (below), then routes by key.
9. Everything else goes to the casual chat door with any gathered evidence attached as labeled context.

## The API client — manners before muscle

- **A key is "connected" only after Meta says yes.** The connect box fires a one-token probe (`probeWonderKey`) before saving anything. A refused key is never saved; a network failure is reported as a different fact from a refusal. The status line is three-state and cannot claim a connection that was never tested. A later 401 on any call drops the stored key and says so once, plainly. *(This law exists because a false "Connected" once shipped and cost trust — it is fixed and tested.)*
- **400 is never retried** — it is a build bug, not an outage, so she falls back at once.
- **Transient failures** (429, 5xx, network, timeout) retry with exponential backoff — 650 / 1300 / 2600 ms plus jitter — up to 3 attempts, then the built-in brain answers.
- **Circuit breaker:** 3 consecutive failures open the circuit for 3 minutes; she stops knocking on a dead door, answers locally, and explains once.
- **Timeouts per door:** chat 12 s, Responses 30 s, probe 15 s. Talk must feel instant or fail fast; research is allowed to breathe.
- The key is stored only in the owner's own browser, is never in the page, and travels only to Meta's API when she answers.

## Effort — spend thought where thought pays

A local classifier (`researchDepthFor`) reads each ask and picks a gear: **low** for short, simple asks; **high** for event cards, multi-part asks, comparisons, "do the math," "dig deep," "explain why." The gear sets reasoning effort and search context size on the connected doors. Bench proof that the gear change is real: the same three-part math question used 28 reasoning tokens at low and 679 at high. Temperature and sampling knobs are never sent — her consistency comes from her instructions, not dice settings.

Chat history is budgeted: the last 12 turns, trimmed to a ~9,000-character ceiling, so her memory stays recent and whole — and the owner's credits aren't spent re-sending an ever-growing transcript. Requests carry a stable prompt-cache key with 24-hour retention, because her instructions are the most repeated text in the system and are treated as cacheable capital, not per-message rent.

## Grounding — evidence before eloquence

Her research runs on two independent legs:

- **Leg 1 — her own gathering (always, key or no key).** Before any model is consulted, the app itself fetches a Wikipedia lookup for encyclopedia-shaped asks and relay web research (search results + page reads, with timeouts) for latest/current-shaped asks.
- **Leg 2 — the model's native search** on the Responses door, with depth-tiered context. The tool's presence never forces a search; the model decides.

**Citation law.** Citations are rendered by the app from real `url_citation` annotations in the model's response, numbered in reading order. The model is forbidden from writing its own sources list. No annotation, no citation — there is no path in this design by which a plausible-looking source can be manufactured.

**Cross-check law.** The app compares its gathered evidence against the native result. When they conflict, she says both plainly and never smooths it over. If nothing checks out, the answer is a labeled fallback — *"From what I know — I couldn't check that one live just now"* — never a bluff. And she may never claim she searched on a turn when no search ran; the parser enforces its half (a search counts only when a real search-call item exists in the response).

## Her hands — tools that execute on the device

Three tools are declared to the model and executed **in the page**, with results returned for the final answer (capped at 4 tool rounds plus one forced final answer):

- `vvToolCalculator` — exact-decimal arithmetic on a rational-number engine, so 0.1 + 0.2 is exactly 0.3 and money is exact to the cent.
- `vvToolDateMath` — real calendar arithmetic with validation. February 29, 2026 is refused, never silently rolled into March. Day counts return signed; the voice layer speaks the direction ("falls 7 days before"), never a bare negative.
- `vvToolVerseLookup` — King James verses served word-for-word from a pinned local store (1769 Oxford text), ranges whole or an honest failure, and an honest failure for references that don't exist.

Standing law (§6 of the internal blueprint): **no tool ships until a written test exists that can fail it.** These three were graded on 36 recipes before shipping — see PROOF.md.

## Memory — three kinds, kept apart

1. **Conversation memory (session).** The budgeted window above; on the Responses door, a server-side thread chained only to *completed* responses — never to an interrupted one.
2. **Owner memory (on-device).** Name, heart level, teachings, and remembered facts persist on the owner's device in one bundle (`veevee-state`), never on a server. **Take her mind with you** (`mindExport`) downloads everything she holds as one file; **Load her mind** (`mindImport`) restores it on any device, with strict validation. The owner's location is **deliberately never stored** — it exists only as session memory when the owner states it.
3. **Identity memory (hers).** Her instruction core (below) is who she is on every device. Because storage is origin-scoped and every owner brings their own device and key, **a new owner is greeted fresh** — she never talks about her builder, never carries one owner's life into another's conversation. That separation is architectural, not a promise taped on top.

## The instruction core — and the proving loop

Her master prompt (search `buildSystemPrompt`) is a separate authored component — the "cookie sheet," 18 blocks covering identity, how she speaks and thinks, her truth laws (two-source cross-check for money, health, safety, law, and deadlines; decline rather than fill from memory), research method, heart-first care, faith lived quietly, her own mind, math standards, prediction honesty (always commit, never a lock), languages, pictures (artwork always labeled), and her limits said plainly.

Nothing here is trusted because it was written. It is trusted because it survived the proving loop:

1. Change the sheet **or** one machine component — never both in one step.
2. Bench-test against a written Test Cookbook through the real API; grade only from saved raw replies.
3. **Fix the sheet, never the test.** Any sheet change voids the run and forces a full re-run.
4. Build the candidate; prove the diff is confined to the intended regions; syntax-check both inline scripts.
5. Ship behind the same link — the repo file is replaced; the contest submission entry is never touched.
6. Verify live, first-hand: a build is "live" only when the served bytes say so.

The current sheet passed the full 89-item battery at 89/89 on Muse Wonder — then the identical, unchanged sheet passed 89/89 again on **Muse Spark 1.3** (the multi-brain socket: the client factory takes the model as a parameter, so the same discipline was examined on a second brain). One production lesson from that run is now standing law: token budgets are harness parameters, not criteria — Spark spends roughly 2–3× Wonder's reasoning tokens, so any future Spark-backed build must ship larger call budgets (chat ≥ 4000 / research ≥ 4400) or high-effort answers starve.

## Response format law

- **One free voice.** She composes internally with structure; the renderer strips every label before display. The person hears one flowing answer, answer first, then reasons.
- **Grounding labels are part of the answer**, not fine print. A Wikipedia answer says Wikipedia says it.
- **Cut-off is confessed.** A truncated answer is labeled as cut, with how to resume it — never dressed as finished.
- **Artwork is labeled artwork**, presented inline, never passed off as a photograph of the real world.
- **Predictions are talk, never locks.** There is no surface in this design — no chip, no badge, no banner — that could dress a lean as a guarantee.

## Measured speed — published both ways

On her live brain with her current instructions: **~9 seconds per answer in a single conversation stream**, and **73 answers per minute in a 10-wide parallel burst**. Both numbers are published because both are true. A single stream is one person talking; the burst is ten at once. No single collapsed "speed" figure is claimed anywhere.

## Deliberate absences

- **No server, no accounts, no analytics.** Nothing phones home in the background; the mic captures nothing before a Talk tap.
- **No money movement, ever.** Payment processing was excluded from her tool design as a permanent law, not a missing feature.
- **No location storage** (see Memory).
- **Meta models only.** One provider, one key, one bill the owner can read.

## Limits, stated plainly

She cannot out-run her model: her ceiling on any single answer is Muse Wonder's ceiling. What this design buys is how fully that ceiling is reached — the right door, the right gear, verified evidence in the prompt, an instruction core written to make the model check itself, and a proving loop that catches her before her owner does. The single-file constraint is kept on purpose: free forever, opens instantly from a home-screen star, works on a borrowed phone, and readable end-to-end by anyone who wants to check her. Its costs — no server, no background work, storage limited to the page — are accepted and designed around, not apologized for.

## Verify it yourself

In `index.html`, search: `buildSystemPrompt` (her instruction core) · `replySmart` (the router) · `probeWonderKey` (the honest connect) · `researchDepthFor` (the effort gears) · `vvToolCalculator`, `vvToolDateMath`, `vvToolVerseLookup` (her hands) · `mindExport` / `mindImport` (her mind-file) · `presenceBtn` (Presence mode) · `camBtn` (Show Vee Vee). Then read [PROOF.md](PROOF.md) for the exam record behind the sheet.

---

Designed and directed by **Wayne Herrington** (solo). Built by the Belly of the Beast.
