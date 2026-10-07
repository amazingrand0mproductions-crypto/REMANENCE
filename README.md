# REMANENCE — Memory & Living Minds

**An original AI Dungeon script for lasting story memory, changing facts and NPCs with separate knowledge, beliefs and motivations.**

REMANENCE v2.1.1 combines evidence-based memory with character continuity. It captures selected complete story excerpts, keeps important canon protected, retrieves older episodes, follows explicit relationship links and gives each NPC a small, private set of knowledge and motivations. Updates happen inside ordinary responses; you do not need to insert extra Continue turns.

The name refers to something that remains after its original source has passed. This is a substantial expansion and rename of CONTINUITY v1. Existing state and generated settings migrate automatically.

[Install](INSTALL.md) · [Commands](#commands) · [Settings](docs/SETTINGS.md) · [Research](docs/RESEARCH.md) · [Validation](docs/TEST_REPORT.md) · [Offline archive](docs/OFFLINE_MEMORY.md)

## What's fixed in 2.1.1

| Area | Correction |
| --- | --- |
| Private learning and beliefs | Recognised unquoted claims about a registered owner are excluded from public memory, including model memory entries, suppressed claims, oversized beliefs and disabled direct capture. |
| Existing duplicate records | Upgrade and restore repair automatic public copies only when matching private ownership and source provenance already exist. Protected copy IDs survive with corrected private scope; authored public decisions remain intact. |
| Model belief updates | The entire supported claim must match. Copied names and shortened fragments are rejected; accepted values reuse the original claim and its negation. |
| Changing settings | Turning minds or model updates off between Context and Output cancels the pending footer. |
| Empty responses | An empty response while the engine is disabled produces a retry report. Ordinary prose passes through. |
| Replay verification | Waits for output streams to close before parsing results, rejects failed child processes and measures its output limit in UTF-8 bytes. |

## What's improved since 2.0

| Area | Original 2.0 | Revised 2.1 |
| --- | --- | --- |
| General capture | Up to 3 selected sentences per action; 4,000 characters / 24 candidates. | Default 8 sentences, configurable to 16; 12,000 characters by default, configurable to 20,000; up to 96 candidates. |
| NPC learning and beliefs | Manual commands or a cooperative model footer. | Explicit named-character statements are also captured directly during normal prose, with independent switches. |
| Recall wording | Literal terms and a small synonym map. | Adds common English inflections and occupation, location, code and awareness vocabulary. Exact numeric details receive priority. |
| Crowded prompts | Straight relevance ordering. | Reduces repetitive unprotected evidence and puts the newest input first in the query. |
| Current state | Newest retained version could resurface after a later value was lost. | Retained earlier values become historical only if the current head is evicted. Source undo or explicit reaffirmation can recover a supported earlier value. |
| Grounding | Normalized source substring. | Private/model mind evidence must match a complete unquoted source sentence. Quote state spans multiple sentences. |
| Repeated commands | Repeated canon/knowledge/mind commands could consume additional records. | Identical protected facts, unkeyed manual knowledge and current manual directions reuse their records. |
| Failed writes | Error logging and narrative fallback. | Mutating slash commands roll back engine memory/settings and return a visible failure report. |
| Search/state | Rebuilt lexical text and a character-count storage guard. | Stored token indexes, compact archive terms, per-hook identity/view caches and a UTF-8 byte guard. |

The release includes [25 targeted improvement probes](docs/RECALL_RESULTS.json): the original 2.0 release passed 11; this release passes 25. They exercise chosen capture, recall and ownership cases, including automatic learning/belief exclusion from public recall. They are **not** a general accuracy percentage, live model evaluation or Inner Self comparison.

## Install in five minutes

1. Open a scenario's **Details → Scripting** in the AI Dungeon website and enable scripting.
2. Replace each tab with the corresponding complete file:

| AI Dungeon tab | File |
| --- | --- |
| Library | [scripts/Library.js](scripts/Library.js) |
| Input | [scripts/Input.js](scripts/Input.js) |
| Context | [scripts/Context.js](scripts/Context.js) |
| Output | [scripts/Output.js](scripts/Output.js) |

3. Save, start a test adventure and enter:

```text
/player Your Full Name
/status
/log off
```

4. Play an ordinary turn. Open **View Context** and check for `REMANENCE MEMORY`.
5. Protect a few indispensable facts with `/remember` or `/fact`. Keep essential ongoing rules in Plot Essentials too.

[ALL_TABS.txt](ALL_TABS.txt) contains all four files with clear separators. Copy the code belonging to each tab; do not paste the whole bundle into one tab. The adventure needs no Node installation, API key, external service or paid integration.

Two settings cards appear automatically: **REMANENCE — Memory** and **REMANENCE — Minds**. Edit values in Entry; Notes explain each option. Existing authored cards are preserved. **Do not import CONFIG_CARDS.json over an existing card collection:** AI Dungeon imports replace that collection. The optional [merge tool](#story-card-imports) preserves your exported cards.

On mobile, use the website and request the desktop site if necessary. Install in playable child scenarios when using Multiple Choice. See [INSTALL.md](INSTALL.md) for upgrades and troubleshooting.

## What it does

| System | Behavior |
| --- | --- |
| Active memory | Stores selected complete excerpts with source anchors, importance and stable IDs. Incomplete fragments are excluded. |
| Episode archive | Moves suitable evicted evidence into a separate compact archive. Both tiers participate in recall, edits and backups. |
| Protected canon | Player-established facts survive ordinary eviction. Keyed attributes retain a current value and a historical timeline. |
| Changing world state | Recognizes narrow, literal statements about known characters' address, occupation, location and life status. Quotes, speculation and attempted Do/Say actions stay evidence. |
| Relationship recall | Traverses up to two hops of explicit relationships to find related evidence. It never treats co-occurrence as a relationship or grants shared knowledge. |
| Private NPC knowledge | Explicit learning, seeing, hearing, reading and witnessing can be captured directly for a registered named owner. World recall excludes private records. |
| NPC beliefs | Captures named-character believes/thinks/suspects statements as private, potentially false claims. Explicit withdrawals clear exact matching current claims and retain their history. A belief is never established world truth. |
| Living minds | Maintains a short goal, feeling and tentative intention per NPC. Optional model updates use grounded evidence from the same response. |
| Commitments | Tracks explicit promises and manually authored threads. Resolution is explicit; the engine does not force plot outcomes. |
| Possessions | Tracks manual item ownership and transfers with a single current holder. |
| Authored cards | Reads character anchors and supported world cards without rewriting them. Changed/deleted source cards invalidate copied evidence. |
| Revision handling | Reconciles recent edits, undo and retries across active and archived records. |
| Corrections | Replaces erroneous excerpts, suppresses immediate automatic relearning and exposes conflicting keyed evidence. |
| Diagnostics | Explains selected memories, lexical matches, relationship distance, context displacement and retention counters. |
| Portability | Paged backups, validated restore, reviewed excerpt imports and a local full text archive tool. |

## A practical example

```text
/player Maya Walker
/npc Ruth Barker
/npc Dan Barker
/remember Maya Walker is allergic to penicillin.
/fact Ruth Barker | address | 12 Baker Street
/relation Ruth Barker | Dan Barker | sibling
/know Ruth Barker | The black vault code is 4482.
/believe Ruth Barker | suspect | Dan stole the missing ledger.
/mind Ruth Barker | goal | Find the ledger before accusing anyone.
/thread return-key | Ruth promised to return Maya's brass key on Friday.
/carry Ruth Barker | brass key
```

Play normally. Later:

```text
/recall penicillin
/connections Ruth Barker
/mind Ruth Barker
/mind Dan Barker
/threads
/inventory Ruth Barker
```

Ruth's private code and belief belong to Ruth. Dan does not acquire them through the sibling edge. The belief remains an interpretation. If Ruth moves, use `/fact Ruth Barker | address | 28 Railway Road`; current recall uses the new address, while `/timeline Ruth address` can show retained earlier values.

Address and observed location are different: living somewhere does not prove current presence. `/where Ruth Barker` labels the tracked fields accordingly.

## Commands

Commands work through Story, Do or Say input. Use the spelling and separators shown below. A command consumes a normal generation, whose output is replaced by a deterministic report. Administrative input and reports do not become story memories.

### Memory and canon

| Command | Purpose |
| --- | --- |
| `/remember fact` | Store protected public canon, up to 700 characters. |
| `/fact Subject \| attribute \| value` | Establish a protected keyed fact; supersede that attribute's prior value. |
| `/recall query` | Search current world evidence from both tiers. Unknown lexical queries abstain. |
| `/timeline query` | Inspect retained historical evidence, including superseded values. |
| `/archive query` | Inspect retained historical world excerpts in the compact archive. |
| `/memory mID` | Inspect a specific record, including a privately owned one. |
| `/pin mID` / `/unpin mID` | Protect an existing record or release its protection. Pinning promotes an archived record. |
| `/forget mID` | Delete a record and suppress automatic relearning of equivalent text in the same scope. |
| `/correct mID \| replacement` | Correct an unkeyed excerpt; public replacements become protected canon, private ones retain their owner. |
| `/conflicts` | Show retained automatic keyed facts that disagree with manual canon. |
| `/excluded` | List bounded suppression codes. |
| `/allow code` / `/allow all` | Release suppression. Follow with `/scan` to reconsider available sources. |

Use `/fact`, `/relation` or `/resolve` to correct their respective keyed records. Forgetting a newest keyed revision can expose an older retained value. Suppression is a bounded list of normalized-text hashes, not a permanent semantic blacklist.

### Characters and relationships

| Command | Purpose |
| --- | --- |
| `/player Full Name` | Lock player identity and exclude the player from NPC minds. |
| `/npc Full Name` / `/npcs` | Register or list NPCs. |
| `/alias Full Name \| Alias` | Add an alias. Ambiguous aliases do not resolve. |
| `/mind Name` | Inspect that NPC's profile, current motivations, knowledge and beliefs. |
| `/mind Name \| goal \| text` | Set a motivation. `feeling` and `intention` are also supported. |
| `/know Name \| fact` | Assign knowledge only to this NPC. |
| `/believe Name \| topic \| belief` | Set a private, potentially false belief. A two-part form without a topic also works. |
| `/relation Name \| Name \| type` | Set an explicit relationship, such as sibling, mentor or colleague. |
| `/connections Name` | Inspect explicit edges and related recall nodes. |
| `/scene Name \| Name` / `/scene auto` | Set or clear a manual scene roster. |
| `/where Name` | Inspect tracked address/location fields; absence remains unknown. |
| `/carry Name \| Object` / `/drop Name \| Object` | Set or clear the item's current holder. |
| `/inventory Name` | List manually tracked possessions for this holder. |

Character/NPC cards, cards with an `@` title or an explicit `[NPC]` tag can provide profiles. Conservative narrative discovery requires repeated actor/speaker evidence. Full names work best. First-name collisions, including player collisions, are excluded.

### Commitments, settings and diagnostics

| Command | Purpose |
| --- | --- |
| `/thread label \| commitment` | Create or update a protected open thread. |
| `/threads` | List retained open commitments and their labels. |
| `/resolve label \| established outcome` | Resolve a thread and transfer protection to its outcome. |
| `/status` / `/diagnose` | Inspect capacity, accepted/rejected updates and state health. |
| `/why` | Explain the previous context packet's selection and space use. |
| `/scan` | Restart incremental scanning of available history and cards. |
| `/set setting = value` | Change a documented setting. Invalid values are rejected or bounded. |
| `/preset compact` / `balanced` / `deep` | Apply a capacity/context preset. |
| `/remanence on` / `off` | Enable or disable the engine. |
| `/minds on` / `off` | Enable or disable NPC mind delivery/updates. |
| `/log on` / `off` | Toggle periodic story status lines. |
| `/help` | Show the built-in command reference. |

### Backups and imports

| Command | Purpose |
| --- | --- |
| `/export 1`, `/export 2`, … | Export a consistent paged snapshot. Save every page's JSON block. |
| `/restore {assembled JSON}` | Validate and replace this engine's memory, settings, NPCs and archive. |
| `/import {remanence-import JSON}` | Add 1–40 reviewed excerpts without replacing the existing engine. |

After collecting every export page:

```bash
node tools/backup.mjs assemble backup.json page-1.json page-2.json
node tools/backup.mjs validate backup.json
```

Use the complete list of your pages. Assembly verifies identity and an accidental-corruption checksum; this is not encryption or cryptographic authentication. Paste the assembled backup after `/restore `. Restore checks ownership, IDs, sizes and capacity before replacing memory. It accepts legacy CONTINUITY v1 backups.

A small additive import looks like:

```json
{
  "format": "remanence-import",
  "records": [
    {"kind": "observation", "text": "The spare brass key was hidden in locker 319.", "protected": true},
    {"kind": "knowledge", "npc": "Ruth Barker", "text": "Ruth knows the black vault code is 4482."}
  ]
}
```

Register private owners first. Supported import kinds are observation, intent, knowledge and belief. Excerpts are at most 700 characters; the request is at most 50,000 characters. Repeated identical imports reuse records. Invalid ownership or excessive protection rejects the whole batch. Imports preserve quoted claims as claims.

## Capacity and presets

| Preset | Active records / text characters | Archived records / text characters | Added context cap / share |
| --- | --- | --- | --- |
| Compact | 300 / 80,000 | 300 / 80,000 | 1,800 / 15% |
| Balanced, default | 600 / 160,000 | 800 / 180,000 | 3,600 / 22% |
| Deep | 1,000 / 250,000 | 1,200 / 300,000 | 6,000 / 28% |

Defaults allow 80 protected records, 48 NPC profiles and 60 open threads. These are independent caps, not guaranteed simultaneous retention. A one-million-byte UTF-8 serialized-engine guard can prune earlier, including the search indexes and source/identity fields. Export temporarily caches one additional snapshot; reports also add small control data. JSON size is not sandbox heap usage.

Pruning favors protected canon, current state, motivations/beliefs, manually specified NPC knowledge, open commitments and useful evidence. Suitable unprotected evictions move into the archive; archive count/text limits eventually remove lower-value episodes. Counters in `/status` report both stages. Lowering capacities can remove unprotected data. If a current keyed value is lost, retained earlier versions are marked historical only; capacity loss does not silently make an old address or motivation current again.

Stored memory and model context are different. Every prompt receives only a bounded selection; even protected facts cannot all fit. `/why` explains selection and **View Context** verifies actual delivery. If essential instructions must apply every turn, put them in Plot Essentials.

## How NPC updates stay smooth

Direct capture works even with `modelUpdates = false`. A registered character's affirmative unquoted sentence such as “Ruth Barker reads the dispatch and learns the gate code is 7619.” supplies scoped awareness. “Ruth Barker suspects Dan Barker stole the dispatch.” supplies a private belief. Awareness of an allegation does not verify it. Recognised learning and belief statements are excluded from ordinary public memory even if a direct-capture switch is off, the claim is suppressed or too large, or structured extraction reaches its limit.

The model cannot use a `memories` entry to make those private sentences public. Its `knowledge` and `beliefs` entries must pass owner and whole-sentence checks; a belief must copy the complete supported claim. These rules control stored and recalled excerpts. Original prose remains in adventure history and may still appear in the model's context.

Questions, requested Do/Say actions, conditional statements, dreams, quoted dialogue and reports about somebody else's awareness cannot grant automatic knowledge. Negating a learning verb cannot grant knowledge. An explicit learned or believed negative claim can still be stored. Unique aliases can work, while ambiguous names and the player are excluded. Pronoun-only and indirect learning remain outside these narrow rules.

A complete narrator sentence such as “Ruth Barker no longer suspects Dan Barker stole the dispatch.” withdraws Ruth's exact matching keyed belief. The parser also accepts “does not believe” and “stopped suspecting” forms. It retains the old claim and withdrawal as private history, restores the earlier retained claim on recent undo, and accepts later explicit reaffirmation. Matching ignores case, spacing, a leading “that” and a final period; it does not infer that differently worded beliefs contradict one another. A negated belief verb without a matching current claim creates no belief. Unkeyed imported beliefs require manual correction or forgetting.

One eligible NPC can receive a small update request at the configured minimum interval. For intervals above one, every fourth interval waits one extra output to avoid repeatedly selecting the same character in a rotating cast. The normal model response may append a reserved JSON footer; REMANENCE removes that footer from visible prose.

Footer knowledge requires a complete unquoted same-response sentence showing the selected character learning, seeing, hearing or another supported awareness action. Beliefs require an explicit same-response statement such as “Ruth Barker suspects…” and a copied claim. A bare substring clipped out of a quote or condition is insufficient. Goals, feelings and intentions remain tentative interpretations.

Stale nonces, unsupported keys, wrong owners, fabricated evidence, duplicate/truncated metadata and invalid JSON are rejected. Narrative prose still passes through. Metadata-only output produces a visible retry report rather than inventing a scene.

If a model rarely follows the footer request, use `/status`, increase response length where available, adjust `updateEvery` or set `modelUpdates = false`. Direct knowledge/belief capture, manual NPC commands and ordinary evidence capture continue. No extra model service is called.

## Full text beyond the script

The optional [offline memory tool](docs/OFFLINE_MEMORY.md) retains your own complete plain-text adventure export outside AI Dungeon's scripting state. It searches exact paragraphs and prepares reviewed source excerpts for `/import`.

```bash
node tools/offline-memory.mjs index story.txt archive.json
node tools/offline-memory.mjs search archive.json "brass seal"
node tools/offline-memory.mjs import archive.json "brass seal" import.json --protect
```

The adventure cannot automatically fetch this local archive. Full text retention is available in the offline file; automatic in-game recall remains bounded. No network, embedding service or language model is used by this tool.

## Story-card imports

The script creates its two settings cards automatically. To merge them deliberately with an existing exported collection:

```bash
node tools/merge-cards.mjs your-cards.json merged-cards.json
```

Inspect and import the merged result. The tool preserves authored entries and refuses output-file overwrite. It replaces engine-owned settings cards from either brand with the current generated defaults; keep automatic in-adventure migration if you want to preserve customized settings. Supported source world types are location, item, faction, lore and world, or cards tagged `[WORLD]`. Notes tagged `[PRIVATE]` or `[SECRET]` exclude the card from REMANENCE indexing. This does not change AI Dungeon's independent native card activation.

## Upgrade from earlier releases

For REMANENCE 2.0, back up and replace all four tabs together. Keep the existing settings cards. State remains at schema 2; the next hook preserves existing values, adds the five new options, refreshes engine-generated Notes and rebuilds indexes once. Authored/custom Notes are preserved.

Back up first. Replace all four script tabs together; keep your existing settings cards. On the next hook, valid `state.continuityV1` is migrated to `state.remanenceV2`, and engine-owned configuration cards are renamed in place. Existing canon, NPCs and settings are retained; new options receive defaults. Unsupported future/damaged state is preserved rather than silently reset.

The legacy `/continuity on/off` command remains an alias. Legacy footer/status markers are cleaned and old backup formats are accepted. Older user-customized card Notes may remain as authored; the new [settings reference](docs/SETTINGS.md) documents all added options.

## Verification and honest limits

This release passed **159/159 regression tests**, **25/25 targeted capture/recall probes** and the complete **10,000-turn replay**, with no recorded engine errors. After 30,356 hook calls, all six protected world facts and the protected automatic-learning fixture passed their final recall checks. The learned code remained available to its owner and was excluded from public recall and an unrelated NPC.

The generated [test report](docs/TEST_REPORT.md) records the measured outcome, timing, state size and exact install-file fingerprints. Fixtures exercise direct NPC evidence, private/public duplicate boundaries, complete belief claims, legacy repair, mid-turn setting changes, belief withdrawal/undo, rotating-cast scheduling, quote boundaries, failed writes, Unicode byte limits, large bootstraps, repetitive recall, loss of current state, replay checkpoint integrity and trailing child-process output.

```bash
npm run build
npm test
npm run benchmark
npm run stress
npm run verify
```

Node.js 20+ is only needed for these development commands and optional tools. There are no npm package dependencies. GitHub Actions runs regression tests, targeted recall probes and a shorter 1,000-turn simulation on pushes and pull requests.

The long verification uses 1,000-turn batches of the same continuous adventure. Each batch writes `verification-checkpoint.json`; rerunning `npm run verify` or `npm run stress` resumes a matching checkpoint, including completed results. If the runtime, fixtures, Node version or target length changes, use a new checkpoint or remove the old one deliberately to start again. The file is excluded from Git and release ZIPs.

The tests execute the real install files with JSON round trips, documented card helpers and a two-second per-hook timeout. They do not certify Latitude's live sandbox, measure peak hosted heap usage or compare real narrative quality against Inner Self. Use the [live compatibility checklist](docs/COMPATIBILITY.md).

Automatic state extraction and awareness checks are narrow, English-focused rules. Retrieval is lexical with light inflection handling, synonyms and explicit graph expansion, not semantic embedding search. Larger capture is still selective: sentences shorter than 20 or longer than 480 characters, unfinished fragments and content beyond configured limits are excluded. Model cooperation and knowledge boundaries cannot be guaranteed solely by script instructions. Recent visible edits are reconciled; unseen old edits need manual correction. The API cannot supply hidden past history, and finite state/context cannot promise perfect recall of everything.

Keep native AI Dungeon memory features when useful. Combining two complete script engines requires explicit hook integration. Each installed hook must end with its own single `modifier(text)` call.

## Research, contributions and license

[Research notes](docs/RESEARCH.md) distinguish implemented techniques from ideas that require embeddings, extra inference or external storage. This release claims no academic benchmark score or measured superiority over another script.

[Architecture](docs/ARCHITECTURE.md) explains source anchors, current state, ownership and retrieval. [CONTRIBUTING.md](CONTRIBUTING.md) covers reproducible changes. [MIT license](LICENSE).

Suggested repository name: `remanence-ai-dungeon`. Upload the extracted project contents so this file becomes the repository's `README.md`; keep `scripts`, `docs`, `tools`, `tests` and `.github` intact.
