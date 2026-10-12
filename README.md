# REMANENCE — Memory & Living Minds

**Story memory and separate NPC perspectives for AI Dungeon.**

A long adventure leaves a lot behind: an address, a promise, a clue, a conversation that changed someone's mind. REMANENCE keeps selected details from the story and brings them back when they become relevant. It also keeps track of who learned something, what they believe, and what they are trying to do.

Install the four script tabs, play normally, and adjust the two settings cards whenever you need to. No startup commands, API keys or extra Continue actions are required.

[Install](#install) · [Settings](#settings) · [Optimised Context](#optimised-context) · [Optional controls](#optional-controls) · [Full setting reference](docs/SETTINGS.md)

## What it remembers

- **Story evidence:** selected complete excerpts, with their source and a stable record ID.
- **Older episodes:** useful evidence moves into a separate archive as the active ledger fills. Recall searches both.
- **Changing facts:** supported changes to addresses, locations, occupations, life status and possessions get current values. Retained earlier values can be recalled as history.
- **Private knowledge:** explicit learning, seeing, hearing, reading and witnessing belongs to the NPC who experienced it.
- **Beliefs and motives:** suspicions remain potentially wrong; goals, feelings and intentions stay attached to their owner.
- **Relationships and commitments:** explicit connections help find related evidence. Supported promises remain open until a matching completion is established.
- **The detail you ask about:** the latest ordinary input guides recall. A specific code or clue gets priority over unrelated recent conversation.

REMANENCE uses existing Story Cards, API-visible history and available Plot Essentials. It preserves source wording, distinguishes attempted actions from established outcomes, and leaves the player's choices to the player.

## Install

1. Open the scenario on the AI Dungeon website. Go to **Details → Scripting** and enable scripting.
2. Replace the full contents of each tab with its matching file:

| AI Dungeon tab | File |
| --- | --- |
| Library | [scripts/Library.js](scripts/Library.js) |
| Input | [scripts/Input.js](scripts/Input.js) |
| Context | [scripts/Context.js](scripts/Context.js) |
| Output | [scripts/Output.js](scripts/Output.js) |

3. Save all four tabs together and start an adventure.
4. Play an ordinary turn. The settings cards appear automatically.
5. Open **View Context** and look for the `REMANENCE MEMORY` block.

[ALL_TABS.txt](ALL_TABS.txt) contains the same code in one file. Copy each section into its matching tab; the separators stay outside the editor. Each small hook ends with `modifier(text)`—include that line.

On mobile, use the website and request the desktop site if the scripting controls are hidden. For a Multiple Choice scenario, install the script in each playable child option. Node.js is only needed for the optional repository tools.

The full setup guide is in [INSTALL.md](INSTALL.md).

## Identify the player

A clear player identity keeps the player out of NPC mind updates. Available multiplayer names and name placeholders can supply it automatically. For a character card, include the character's full name and:

```text
Role: player character
```

You can also identify the player in Plot Essentials:

```text
Player character: Maya Walker
```

If the player remains unresolved, set `playerName` in the Memory card. Conflicting identity markers need a clear correction. Full names also help avoid collisions between characters who share a first name.

Character cards register NPCs. Narrative discovery can register a full name after separate, complete actor or speaker excerpts establish it; available earlier evidence is then reconsidered for that character.

## Settings

There are **two editable configuration cards**, with **56 settings** between them:

| Card | Settings | Controls |
| --- | --- | --- |
| **REMANENCE — Memory** | 39 | Player identity, capture, retention, archive, recall, delivery and feedback. |
| **REMANENCE — Minds** | 17 | NPC discovery, profiles, knowledge, beliefs, motivations and optional inferred updates. |

**Edit the values in Entry.** Notes explain every setting, including its default and allowed values. Changing Notes does not change configuration. Leave both settings cards unpinned.

Entries use `key = value`. Blank lines and full-line `#` or `//` comments are accepted. Changes apply on the next script hook. Invalid values retain the previous effective setting; numeric values are bounded to the documented range. Missing options are restored. Creator comments and surrounding Notes are preserved.

A few useful settings:

| Card | Setting | Default | Use |
| --- | --- | --- | --- |
| Memory | `enabled` | `true` | Master switch. |
| Minds | `enabled` | `true` | Turn off NPC minds while keeping world memory. |
| Memory | `playerName` | `""` | Set the player's name explicitly. |
| Memory | `deliveryMode` | `"auto"` | Choose automatic, direct Context or native card delivery. |
| Memory | `cardBudget` | `1600` | Maximum characters for the managed native Recall card; context limits can reduce it. |
| Memory | `pinRecall` | `true` | Request native pinning for the Recall card; leave authored pins unchanged. |
| Memory | `memoryBudget` / `budgetShare` | `3600` / `0.22` | Limit recall's characters and share of available context. |
| Memory | `focusRecall` | `true` | Give the latest ordinary input priority. |
| Memory | `historicalRecall` / `historyPerContext` | `true` / `2` | Allow bounded, labelled past values for explicit historical questions. |
| Memory | `autoKeep` / `maxAutoKept` | `true` / `24` | Give selected critical evidence extra retention priority. |
| Memory | `statusEvery` | `10` | Set the status-line interval; `0` hides it. |
| Memory | `settingsFeedback` | `true` | Show a short acknowledgement after settings change. |
| Minds | `autoKnowledge` / `autoBeliefs` / `autoMinds` | `true` | Capture supported explicit narration directly. |
| Minds | `knowledgePerContext` / `beliefsPerContext` | `2` / `1` | Limit private rows delivered for each referenced NPC. |
| Minds | `modelUpdates` | `true` | Allow optional inferred updates from an ordinary model response. |
| Minds | `adaptiveUpdates` | `true` | Slow update requests after repeated missing or rejected replies. |

For quiet play, set `statusEvery = 0` and `settingsFeedback = false` in Memory. For explicit narration only, set `modelUpdates = false` in Minds. Direct capture continues.

The full reference is [docs/SETTINGS.md](docs/SETTINGS.md). Editing examples and the difference between capture, storage and delivery are in [docs/CONFIGURATION.md](docs/CONFIGURATION.md).

## Optimised Context

Leave `deliveryMode = "auto"` for normal use. When AI Dungeon reports Optimised Context through `info.useCacheEfficient`, REMANENCE prepares recall in one managed native Story Card: **REMANENCE — Recall**. This is the delivery card; Memory and Minds remain the only two configuration cards.

The Recall card is refreshed during ordinary Input and Output, with Context providing an additional refresh and an inclusion check when the platform exposes its lore. Recall can therefore reach native context even when returned Context-script text is ignored. The script leaves Plot Essentials and Author's Note alone.

The native packet is smaller than the usual Context packet. Complete evidence rows take priority; focused private knowledge comes before optional profile detail. Optional updates use a compact request, and short-lived request receipts allow a delayed native reply to be validated.

**Use View Context to check inclusion.** The script requests native pinning for the Recall card automatically through AI Dungeon's documented `isPinned` property. Set `pinRecall = false` to request unpinning. Authored pins stay as they are.

Native pin limits can refuse the change, and Story Cards share a finite allowance. A prepared card can still be omitted; selection may take a turn to catch up. If automatic pinning is unsupported, use **only the Recall card's** UI pin control. Reduce `cardBudget` and competing entries if native space is crowded. Pinning still uses the platform's available card space.

| `deliveryMode` | Behaviour |
| --- | --- |
| `"auto"` | Use the latest supplied mode signal. Native card delivery when true; direct Context delivery otherwise. |
| `"card"` | Force native delivery if the platform omits the signal. Useful when Optimised Context is on but automatic detection is unavailable. |
| `"context"` | Force direct Context insertion. Use where the model accepts Context changes; it cannot override platform restrictions. |

If the signal first arrives in Context or Output, native delivery may need a warm-up turn. Switching back to direct delivery removes the managed Recall card. Disabling the master switch clears it. A stale or echoed packet is stripped from ordinary output before capture.

Details, platform sources and the live setup checklist are in [docs/COMPATIBILITY.md](docs/COMPATIBILITY.md) and [docs/RESEARCH.md](docs/RESEARCH.md).

## Ordinary play

Once the characters are known, clear narrator statements can establish details such as:

```text
Maya Walker is allergic to penicillin.
Ruth Barker lives at 12 Baker Street.
Ruth Barker learns the hatch code is 7392.
Ruth Barker suspects Dan Mercer stole the missing ledger.
Ruth Barker wants to check the evidence before accusing anyone.
Ruth Barker picks up the brass key.
Ruth Barker promises to return the brass key to Maya Walker.
```

Later narration can change them:

```text
Ruth Barker lives at 28 Railway Road.
Ruth Barker no longer suspects Dan Mercer stole the missing ledger.
Ruth Barker returns the brass key to Maya Walker.
```

Ask about Ruth's current address, the hatch code or her earlier suspicion during play. Supported details are selected for the recall packet. An explicit historical question can bring back a retained earlier value under a historical label.

Ruth's learned code stays attached to Ruth. A relationship with Dan does not grant him that knowledge. Requests, quoted dialogue and conditional statements do not automatically establish the same facts as clear narration. The English-focused rules handle explicit patterns best; indirect or ambiguous phrasing may remain a plain excerpt or be missed.

NPC goals can also receive short inferred updates from the model's normal response. These are optional, use copied evidence from that response and never require a separate generation. Missing metadata leaves ordinary capture running. Repeated unanswered requests back off to conserve space.

## Optional controls

Normal play needs none of these. They are useful for a deliberate correction, inspection or backup. Administrative reports stay out of story memory. A command uses an ordinary generation, whose output is replaced with the report.

| Command | Purpose |
| --- | --- |
| `/remember fact` | Keep protected public canon. |
| `/fact Subject \| attribute \| value` | Set a protected current fact. |
| `/recall query` / `/timeline query` | Inspect current or retained historical evidence. |
| `/memory mID` | Inspect a specific record. |
| `/forget mID` / `/correct mID \| text` | Remove or correct evidence. |
| `/pin mID` / `/unpin mID` | Protect a script record or release its protection. |
| `/mind Name` | Inspect an NPC's perspective. |
| `/know Name \| fact` | Give one NPC explicit private knowledge. |
| `/believe Name \| topic \| claim` | Set a private belief. |
| `/relation Name \| Name \| type` | Establish an explicit connection. |
| `/threads` / `/resolve label \| outcome` | Inspect or resolve a commitment. |
| `/status` / `/why` | Inspect state and recall selection. |
| `/scan` | Reconsider currently available sources. |
| `/export 1`, `/export 2`, … | Collect a paged backup. |
| `/restore {JSON}` / `/import {JSON}` | Restore a complete backup or add reviewed excerpts. |
| `/help` | Show the full command reference. |

Record protection is separate from native Story Card pinning. Public lookup excludes private records; a deliberate NPC or record inspection can show private information and spoilers. Unknown slash input passes through by default; `strictCommands` changes that behaviour.

Backups use [tools/backup.mjs](tools/backup.mjs). Save every exported JSON page before assembling:

```bash
node tools/backup.mjs assemble backup.json page-1.json page-2.json
node tools/backup.mjs validate backup.json
```

Provide the complete page list. Restore validates the assembled data before replacing this engine's state. The checksum detects accidental corruption; backups are readable JSON.

## Existing stories and longer archives

Keep existing authored cards in place. Automatic setup creates the settings cards alongside them. If you import [CONFIG_CARDS.json](CONFIG_CARDS.json), take care: AI Dungeon card imports can replace a collection. The optional merge tool combines files first:

```bash
node tools/merge-cards.mjs existing-cards.json CONFIG_CARDS.json merged-cards.json
```

`[PRIVATE]` or `[SECRET]` in a source card's Notes excludes that card from script copying. Those tags do not stop AI Dungeon from triggering the original card natively.

The optional [offline memory tool](docs/OFFLINE_MEMORY.md) can keep complete text you supply in a local archive, search it and prepare reviewed imports. It runs outside the adventure. The script cannot fetch hidden old history or an offline archive on its own.

## Capacity

The defaults allow up to 600 active records and 800 archived excerpts, with separate text limits. Protected canon, selected critical details, useful private evidence and current state receive retention preference. Storage remains bounded; older unprotected data can eventually be removed.

Only a small relevant selection reaches each model call. Raising storage limits does not enlarge the model's context, and increasing a delivery budget does not guarantee native card inclusion. Deep undo or edits beyond the visible source window can require a deliberate correction.

Owner labels keep script records and recall scoped. Original story prose and independently activated native cards may already reveal a detail to the model. Model behaviour still determines whether the narrative respects a character's knowledge and the player's agency.

## Repository contents

| File or folder | Contents |
| --- | --- |
| `scripts/` | Four files to paste into AI Dungeon. |
| [INSTALL.md](INSTALL.md) | Setup and delivery troubleshooting. |
| [SCENARIO_DESCRIPTION.txt](SCENARIO_DESCRIPTION.txt) | Ready-to-use scenario listing text. |
| [CONFIG_CARDS.json](CONFIG_CARDS.json) | The two settings cards in AI Dungeon import format. |
| [docs/SETTINGS.md](docs/SETTINGS.md) | Every setting, default and allowed value. |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Implementation details for contributors. |
| [docs/RESEARCH.md](docs/RESEARCH.md) | Primary sources and design decisions. |
| `tests/` and `tools/` | Local checks, replay fixtures and optional archive/backup utilities. |

For repository work, use Node.js 20 or later. The code has no npm dependencies:

```bash
npm run build
npm test
npm run verify
```

The verification report records its scope and limitations in [docs/TEST_REPORT.md](docs/TEST_REPORT.md). Script composition with another engine requires reviewing both sets of modifiers and shared controls.

## License

[MIT](LICENSE). REMANENCE is an original implementation; it does not bundle another memory script.
