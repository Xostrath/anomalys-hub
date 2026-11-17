# Sparkling — findings and handoff

Last reviewed: 2026-11-17. This is the initial consolidated handoff from the available source, three reviewed combat captures, the first automatic-defense log, and user feedback. Earlier unrecorded work may be missing. Add evidence rather than reconstructing missing history from guesses.

## Read first

**Current mode: ACTIVE TIMING TEST (Combat page v7 / diagnostics v5).** The user explicitly requested active auto parry with recording and opponent-parry handling. This supersedes the temporary passive-only instruction; see SP-T021 for the latest correlation/LOS changes. Old mode instructions remain historical.

- Universe: `10771972206`. Tutorial: `101713836627444`. Observed main world: `110405289082842`. User-supplied entry place: `120525873606470`.
- Current objective: collect enough diagnostics to explain every automatic-defense attempt or skip before changing its timings.
- Current defense: off-by-default active trial covering five shared combo animations, with PvP/PvE filters and continuous diagnostics. Reliable timing remains unproven (SP-T017–SP-T019).
- Latest implementation: **Combat page v7 / diagnostics v5**. Source and both build variants compile; 155 mocked assertions passed. Ordinary delayed-release correlation and the scoped head-effect exclusion are verified in the v5 live capture (SP-T022); broader reliability remains unproven.
- Keep the user's live session under their control unless the current chat has authorization to interact with it. The user normally reloads and tests the installed script themselves. Check for another chat's work before replacing files; do not infer session ownership from these notes.
- Race availability, PowerLevel meaning, and maximum Ki are unresolved. No dedicated ESP-data report was available in the reviewed evidence.

## Feature status

| Feature | Current behavior | Evidence / status | Remaining question |
| --- | --- | --- | --- |
| Profile identity | Universe routing labels the hub Sparkling across places | Implemented | Do not equate place coverage with identical remotes |
| Interface | Shared Hunter X Enter design; isolated UI root | User-confirmed visual correction | Preserve isolation from game styling |
| Combat recorder | Recorders tab; passive normal-combat capture | Three captures reviewed | Capture caps and hook coverage affect completeness |
| Race ESP | Explicit string Race attributes/direct StringValues only; conflicts/missing data unavailable | Implemented, source inspected | Does the live game expose this field for each player? |
| Power ESP | Direct Character.PowerLevel numeric value; rounded whole numbers with commas | Numeric source observed; formatting compiled | Base, effective, or suppressed power? |
| Ki ESP | Blue bar next to red health bar; requires known Ki and explicit positive MaxKi for fill | Ki observed; bar compiled | No verified MaxKi source; unknown max shows an empty track marked `?` |
| Self ESP | Local player uses the same ESP renderer as others; independent of other-player master | Implemented after user clarification | Live correction still needs explicit validation |
| Auto parry | Five shared combo trials, configurable range/timing, facing/LOS gates and F input | Active v7 / diagnostics v5 reviewed through SP-T022 | Reliable timing and input/outcome attribution unverified |
| NPC ESP v1 (SP-T010) | ESP tab "NPC ESP" card (`src/SparklingNpcESP.lua`): every Humanoid rig in Workspace that isn't a player character. Shows name/type, HP current/max, direct `PowerLevel` value if the model has one, distance, box. Types (v3, SP-T012): **ENEMY** = the model has a `PowerLevel` (child NumberValue/IntValue or attribute), **NPC** = no power level; each type has its own toggle. UI v3: Player ESP and NPC ESP are `ui.Section`s with checkmarks (`ui.CheckGrid`) and a 100–10,000 stud distance slider (`ui.Slider`) at the bottom | Implemented / untested live; both builds compile | Does the health rule hold for every NPC? Where do NPCs live, and do they carry PowerLevel? |
| Combat page v7 (SP-T021) | Five active combo trials; continuous diagnostics v5; narrow local head-effect LOS exclusion | 155 mocked assertions and compilation passed; live fixes verified in SP-T022 | Isolate combo-3 timing; retain other seeds and account for changed receipt latency |
| Parry diagnostics v5 | Recent API/input/request correlation; early releases and intervening input flags; bounded LOS object properties | Live: 25 release pairs and one unmatched cycle correctly separated (SP-T022) | Early-release/ambiguous-window branches remain mock-tested |

## Sources and capture workflow

The public repository contains generated `AnomalysHub.lua`. Editable modules are in the established local source tree; see [project notes](../../PROJECT_NOTES.md#source-and-publishing). Do not substitute an old checkout or publish a private build.

| Source module | Responsibility |
| --- | --- |
| `src/UniversalHub.lua` | Shared layout, UI isolation, ESP renderer, controls, cleanup, fingerprint action |
| `src/SparklingCombatRecorder.lua` | Manual passive recorder; reusable forwarding hooks and single-owner Action observation |
| `src/SparklingESP.lua` | Explicit data resolution and ESP source capture |
| `src/SparklingAutoParry.lua` | Experimental input decisions, diagnostics v2, Recorders Save/Copy controls |
| `tools/build_public.ps1` | Profile routing and module assembly into public/private variants |

Use the existing **Settings → Capture Fingerprint** first. It inventories client-visible remote paths/classes; it does not decompile modules, invoke remotes, or prove a vulnerability. Reports live under `AnomalyHub/universal/` in the executor workspace:

| Capture | Controls / output | Limits and caveats |
| --- | --- | --- |
| Fingerprint | Settings; `remote_fingerprint_10771972206.txt` | Place matters; infrastructure and duplicate client objects are included |
| Manual combat | Recorders → Sparkling Combat Recorder; `sparkling_combat_capture.txt` plus `sparkling_combat_<timestamp>_<nonce>.txt` | 120 seconds, 16,000 lines / 4 MB; saves every 5 seconds; up to 6 nearby actors within 100 studs |
| ESP sources | Recorders → Sparkling ESP Data; `sparkling_esp_data_latest.txt` | Bounded explicit source discovery; needed before claiming race/power/max-Ki accuracy |
| Auto-parry diagnostics | Enable Auto Parry; Recorders → Sparkling Parry Diagnostics → Save Log / Copy Log; disabling Auto Parry finalizes | `sparkling_auto_parry_latest.txt` plus `sparkling_auto_parry_<timestamp>_<GUID>.txt`; 12,000 event lines / 3 MB, 2,048 bytes per line; saves every 5 seconds |

The manual recorder stops auto parry before starting. Auto parry refuses to start while a manual combat recording is active. This preserves one owner of the shared hooks and keeps manual evidence separate from generated inputs.

Diagnostics detail freezes at its size/line cap, while counters and final status continue. A capture-limit marker means missing events cannot be treated as absence. Save failures appear in the UI; Copy Log remains available from memory. Latest and history writes are reported independently. Death, character change, teleport, focus loss, disable, and unload stop auto parry and attempt F release.

### Diagnostic report format v2

The header records version, session ID, universe/place, Unix and local clocks, history path, coverage caveats, limits, and defense configuration. Event lines begin with seconds since session start. The footer retains stop reason, attempts, self-counter event categories, damage-event count, held-key state, dropped-line count, event totals, and skip totals.

| Events | Meaning |
| --- | --- |
| `ACTOR`, `WATCH_ADD/REMOVE/REPLACED`, `COVERAGE` | Session actor IDs, observation scope and hook availability |
| `ANIM_START/EXISTING/STOP`, `TRACK_LIMIT` | Animation ID, phase, speed, length, playing state; existing tracks do not trigger defense |
| `CUE_QUEUED`, `CUE_ACCEPTED`, `SKIP` | Cue ID, actor, observe/execute stage, decision reason, distance, vertical difference, facing dots, LOS obstacle where evaluated, scheduling lateness and gate state |
| `F_DOWN`, `F_DOWN_FAILED`, `F_UP` | Input API result, attempt/cue ID, planned/actual hold time, release lateness and errors |
| `ACTION` | Observed Action OUT/RETURN, hook path, call ID, request start, first arguments, argument count, and attempt being sent/held |
| `INPUT` | Selected combat key/button receipt and processed flag; synthetic origin is unknown |
| `COOLDOWN`, `COUNTER_COOLDOWN`, `EFFECT` | Incoming client notifications, not independently verified enforcement |
| `STATE`, `STATE_REMOVED`, `SELF_HEALTH` | Relevant sampled local fields and event-driven health changes; state sampling every 0.5 seconds can miss short transitions |
| `SELF_COUNTER` | Self-targeted counter message and **all** attempts in the preceding second; zero candidates, one temporal candidate, or multiple ambiguous candidates |
| `CLOCK`, `FRAME_GAP`, `GATE`, `ERROR`, `LOGGER_ERROR`, `STOP` | Clock/ping samples, slow frames, menu/text-entry pauses, errors and lifecycle |

F_DOWN only proves the input API returned. An Action OUT/RETURN is not server acceptance; direct/namecall hooks can duplicate an observation. A self-counter message is a client result observation, not proof that one generated input caused it. No attacker is identified by that message. Do not divide counter-event count by attempt count and label it a success rate. Do not count another actor's counter as the local player's success.

## Findings

### SP-001 — tutorial and main-world scope

The reviewed tutorial fingerprint had 95 entries; the main-world fingerprint had 126. Counts include Roblox infrastructure and duplicated client objects. Main world retains watched combat paths and adds training/dance-related paths. Both belong to the same universe; game identity must not depend only on the entry place.

Observed paths under `ReplicatedStorage.Requests`: `Action`, `AIWallSlam`, `WallSlam`, `Dashed`, `StopDash`, `Cooldown`, `CooldownNT`, `Effects`, `Flight`, `Beserk`, `Oozaru`. Their names do not establish permissions, arguments, or vulnerabilities.

### SP-002 — input and Action mappings

Two tutorial captures lasted 120 seconds and 78.55 seconds and contained 46 and 23 outgoing Action observations, each with a matching return through the namecall path. No direct-hook duplicates appeared in those two captures; this is not a guarantee for future sessions.

| Input / context | Observed Action arguments |
| --- | --- |
| Left click | `MouseButton1`; sometimes boolean and vector-like extra arguments |
| Right click | `MouseButton2`; second argument absent or `Up`, sometimes vector-like third argument |
| G | `Grab` |
| A during grab | `Grab`, `Throw` |
| D during grab | `Grab`, `Slam` |
| F down / up | `Defend`, `true` / `false` |
| M | `Meditate` |
| Left Shift | `Descend` |

Exact meanings of attack booleans/vectors remain unknown. Changed MouseButton1 argument shapes after the One key are a correlation, not a proven mode transition. No money-granting result or vulnerability was established in the evidence available for this note.

### SP-003 — cooldown and defense observations

Incoming `CooldownNT` advertised Heavy=2 seconds and Grab=5 seconds. Defense notifications included Counter=5, 4, and 0.5 seconds; Block=5; ZVanish=3. These are client notifications, not measured server enforcement. Tutorial self-related result messages included `Perfect Counter!` around 50.714 seconds and `Counter Parry!` around 65.849 seconds; PBlock, KiSlap, and CounterCooldown changes were also observed.

The main-world manual capture reached 16,000 lines at roughly 118.45 seconds. Self-targeted `Counter Parry!` messages occurred near 28.747, 31.680, and 72.046 seconds. The first and third followed opponent animation `84394230178792` by about 0.20/0.23 seconds; the second followed `95937874840716` by about 0.10 seconds. These are client receipt intervals, not measured server parry windows.

The experiment maps only those IDs: delays 0.04 and 0 seconds respectively. It requires range ≤8 studs, attacker-facing dot ≥0.2, self-facing dot ≥0, and clear LOS. Hold is 0.12 seconds, pulse gap 0.3 seconds, cue expiry 0.25 seconds, observer range 30 studs. Observed valid Counter durations pause new pulses. Existing manual F, menu, text entry, invalid roots, ended tracks, and lifecycle conditions also gate behavior. Heavy attacks, projectiles, and other styles remain uncovered.

### SP-004 — ESP data limits and user requirements

`Character.PowerLevel` numeric values were observed, including growth from 944 in the reviewed evidence. Whether this is base/effective/suppressed power remains unknown. Read the target player's own replicated field; never substitute local-player data. Format as rounded comma-separated whole numbers, e.g. `12,345`.

Race accepts explicit Race string attributes or direct Race StringValues on the Player/current Character. Missing, conflicting, or numeric values are unavailable. Do not infer race from appearance, preview rigs, or save slots. This source policy is implemented; live availability is unverified.

Direct numeric Character.Ki was observed, including 153 in tutorial and 163.5 in main-world evidence. No maximum was recorded. Fill the blue Ki bar only when explicit positive MaxKi is available without conflicting values. The user requested a bar beside health, not a KI text line. The user also clarified that Self outline means the **same ESP as everybody else**, not a separate Highlight. The saved `SelfOutline` key remains for compatibility; the UI says Self ESP.

### SP-005 — incomplete observation is not absence

Manual capture reached UI discovery's 600-object bound in both tutorial sessions, actor discovery's 1,000-object bound, and character-stat discovery's bound in one session. The main-world capture reached its line cap. Missing fields/events therefore do not prove that the game lacks them. Prioritize `Workspace.Live`, explicit character fields, and relevant HUD branches when investigating further.

## Experiments: successes, failures, and inconclusive results

| ID / date | Context / version | Expected behavior | Observed result | Verdict and follow-up |
| --- | --- | --- | --- | --- |
| SP-T001 / 2026-11-17 | Shared UI in Sparkling | Match Hunter X Enter's hub | PlayerGui mounting allowed Sparkling to overwrite fonts/borders; isolated UI root corrected it and user confirmed | **User-confirmed success** after visual failure. Preserve `gethui()` then CoreGui fallback |
| SP-T002 / 2026-11-17 | Identity/navigation revisions | Sparkling label and dedicated Recorders section | User reported Universal branding and recorder placement mismatch; universe routing and Recorders placement implemented | **Implemented**, prior failures recorded; confirm in future UI check |
| SP-T003 / 2026-11-17 | Tutorial/manual recorder | Capture ordinary combat | Two readable captures, observed mappings above, no callback errors reported in review | **Observed capture success**, limited by discovery bounds; no exploit or defense success implied |
| SP-T004 / 2026-11-17 | Main-world manual recorder | Identify self-counter correlations | Three self-counter receipts with the two candidate IDs; capture hit line cap | **Observed / inconclusive timing**, collect isolated exchanges |
| SP-T005 / 2026-11-17 | Auto-parry v1, user's first test | Explain automatic attempts and results | 14 logged cues, 4 F-down attempts, 2 self-counter messages after attempts; v1 omitted skip reasons and F-up evidence | **Inconclusive performance; diagnostic failure.** Not a proven 50% success rate. Replaced logger with v2 |
| SP-T006 / 2026-11-17 | ESP clarifications | Self uses regular ESP; Ki is a bar; power has commas/no decimals | Previous separate Highlight and numeric KI label mismatched request; shared renderer/blue bar/number formatting implemented | **Implemented / untested live**, source and build checks passed; dedicated ESP data still needed |
| SP-T007 / 2026-11-17 | Diagnostics v2 | Explain decision/outcome paths without changing mapped timing | 47 mocked assertions passed: gates, input errors/release recovery, Action observation, outcome ambiguity, save failure/copy, history, caps and cleanup; builds compile | **Mock/compile success, not live success.** Reload and record a short combat test |

| SP-T008 / 2026-11-17 | Combat page v3 (`src/SparklingAutoParry.lua`, Claude chat) | User's reference layout: Auto Parry section with keybinds and range slider; "Don't Parry If" section | Built in Hunter X Enter section style. Options persist in `sparkling_combat_settings.json`; Auto Parry itself still never turns on after a reload. Every decision is logged (`SETTING`, `SKIP reason=pve_off/player_attacker/self_knocked/off_screen`); the report Config line lists the options. Watch range = max(30, range+10) | **Implemented / untested live.** The PvP row from the reference was left out deliberately (scope: no combat automation against other players; see the Hunter X Enter note's Read first). Knocked = `Character.Ragdolled==true` is a **hypothesis** (the attribute was seen in captures; Effects `Knockback` events also exist). Typing off means F lands in the focused text box | Live test: NPC trainer exchange; confirm rows, keybinds and slider; check a ragdolled moment logs `self_knocked` |
| SP-T009 / 2026-11-17 | Diagnostics v2 build (**not** the Combat page: Config lines show the old fields). Four sessions 13:19–13:25 local | User's parry tests | Sessions 1–2 (9.6 s, 4.4 s): 0 attempts, all cues unmapped. Session 3 (82.6 s): opponents were two **players**; 291 of 304 cues unmapped, 1 queued, 0 attempts; 3 self-counter messages with no attempt candidate (manual). Session 4 (58.0 s): opponent was a **player**; 5 attempts (all on mapped IDs, F held 0.128–0.147 s, every F_UP ok), each followed by one self `Counter Parry!` 0.55–0.96 s later (5 single-candidate, 2 more with no candidate); ~0.56 health taken shortly after each attempt. NPC trainers (`WolfFangTrainer`, `TienshinTrainer`) were **never within the 30-stud watch range** and produced no ANIM_START. They appear only as ACTOR entries from incoming events, with a new actor ID every ~0.4 s (models seem to be replaced or re-sent often) | **Inconclusive for PvE: no NPC attack was observed.** The session-4 temporal correlation is against a player, which the Combat page v3 rule now excludes. The `f_already_down` skips (inputF=true about 0.4 s after a successful F_UP) are unexplained: a manual F, or the game holding the key state? | Fight an NPC that actually attacks, within 30 studs, on the Combat page v3 build (Config shows `players=never`). Establish which NPCs are hostile and their attack animation IDs before mapping anything |
| SP-T010 / 2026-11-17 | NPC ESP v1 (`src/SparklingNpcESP.lua`, Claude chat) | User asked for NPC ESP. Reused [Beyond Nen](beyond-nen.md) `src/BeyondNenNpcESP.lua` structure: one Workspace scan, DescendantAdded/Removing, per-model billboards, shared `NpcEspEnabled`/`NpcEspDistance`/`NpcEspSettings` | Beyond Nen's Trello catalog was **not** transferred. Classification uses only the observed `...Trainer` names; no enemy/boss/quest roles are claimed. New shared setting keys: `Health`, `Power`, `Trainer`, `Npc`. Power uses the same direct-value rule as player ESP | **Implemented / untested live** | Turn on NPC ESP in the main world. Confirm labels on WolfFang/Tienshin trainers, look for NPCs outside Workspace.Live, and note whether any NPC shows POWER. The Sparkling Trello (board `ruce1V2a`) could supply an NPC catalog later |

SP-T005 local-clock details: attempts at 2609.3927 (`84394230178792`), 2609.8081 (`95937874840716`), 2628.8275 (`95937874840716`), and 2659.8905 (`84394230178792`). Self-counter messages at 2610.2804 and 2660.4575. The first message was within one second of **two** attempts, so v1's assignment to attempt 2 was not uniquely supported. These are sanitized offsets from the prior local latest log; raw captures are not committed.

## References from other games

- [Beyond Nen](beyond-nen.md): `src/BeyondNenParryRecorder.lua` for bounded elapsed-time/history reports, clock samples, actor/track IDs, and passive capture lifecycle. `src/BeyondNenAutoParry.lua` for decision reasons, input/remote/result separation, and uncertainty handling. Its current source carries weapon-specific tuning and old experiment claims; none are evidence for Sparkling timing.
- [Hunter X Enter](hunter-x-enter.md): shared visual style and `src/HunterXEnterRecorders.lua` for passive capture/UI patterns. Sparkling originally reused this recorder structure; keep controls under Recorders.
- Reuse Sparkling's existing forwarding hooks rather than stacking another independent hook. Manual and automatic observation have explicit exclusive ownership. Hook availability remains executor-dependent.

## Handoff

1. Reload the installed/public updated hub. Enable Auto Parry, test a short normal encounter, then disable it to finalize diagnostics. No desktop takeover is needed for the user to do this.
2. Inspect `sparkling_auto_parry_latest.txt` and its history. Verify outgoing hook coverage, F_DOWN → observed `Defend,true` → F_UP → observed `Defend,false`, decision reasons, health deltas, candidate ambiguity, and any capture/save/errors. API returns do not prove server acceptance.
3. Before changing attack delays, isolate one opponent and identify known versus unmapped animations. Investigate the measured skip reason or actual input/result gap; do not retune from a missing v1 log entry.
4. Capture ESP Data to settle Race source availability, explicit MaxKi, and each player's PowerLevel field. The meaning of power may need a controlled in-game comparison beyond field discovery.
5. Update this note after each test with its feature version, place, actions, result, evidence offsets, verdict, and next step. Add successes and failures equally. Check other games' notes before adding a new feature.

## Change history

- 2026-11-17 — ESP v5 (SP-T014). **ESP Data capture reviewed** (main world, 12 players, the user's own Ki at full):
  - **Ki maximum found:** it is an attribute on the Ki value itself, `Character.Ki@Max`. All 12 players have both `Ki` (NumberValue) and `Ki@Max`. Most read full; one read 200.7 / 441.6, which confirms a live current/max pair.
  - The Ki bar now fills from `Ki.Value / Ki@Max` (the old `MaxKi` sources are still accepted). This fixes SP-T013's empty bar; untested live.
  - The same `@Max` pattern exists for `Character.Stamina@Max`, `OverCharge@Max` (100) and `Shield@Max`, plus `Output@MaxOutput`/`Output@Overcharge`, `Charges`, `RacePotential` and `Humanoid@BaseMaxHealth`. These are candidates for future bars.
  - **Race verified:** the explicit `Character@Race` string attribute resolved for all 12 players (Human/Saiyan/Namekian).
  - `Character.PowerLevel` was present for all 12.
  - `KiHud.Stage.bars` holds ImageLabels `Health`, `Stamina`, `Ki`, `Food`, `Readout`, `Orbs` (fill method not needed now).
- 2026-11-17 — ESP v4 (SP-T013):
  - Enemy NPCs get a health bar (user request) in the player-ESP style: a 7px dark track right of the box, red fill from the bottom, Humanoid Health/MaxHealth, shown when the Health checkmark is on. Regular NPCs keep the HP text only.
  - **Ki bar failure (user report): "the ki bar does not display anything."** Cause from source: the bar fills only with an explicit `MaxKi` attribute/value, and none exists in any capture. Only `Character.Ki` (153, 163.5) was ever observed; `PlayerGui.KiHud` captures listed text labels only (slots, limbs/Fatigue, BossStage), never a bar fill. No maximum was invented.
  - **Capture ESP Data** now matches any name containing ki/energy/charge/stamina/max and dumps every `PlayerGui.KiHud` GuiObject with its Size scale and text, to find the Ki maximum or the HUD's own fill fraction.
  - Next: the user runs it at full Ki and again after spending Ki.
- 2026-11-17 — ESP v3 (SP-T012):
  - **User-supplied rule:** enemy NPCs are identified by having a power level. It replaces the v2 max-health split (superseded; the user's note that regular NPCs have 100 or 999 max health is kept as context).
  - The user asked for checkmarks and a distance slider at the bottom of each ESP section, so Sparkling's Player ESP (`src/SparklingESP.lua`, which hides the shared button card) and NPC ESP are now sections.
  - New shared helpers `ui.CheckGrid` and `ui.Slider` in `src/UniversalHub.lua`; other games are unchanged.
- 2026-11-17 — NPC ESP v2 (SP-T011): regular vs enemy split by MaxHealth. **User-supplied rule:** regular NPCs have 100 or 999 max health, enemies don't. Replaces v1's name-based TRAINER label (superseded). Shared setting key `Trainer` removed; `Enemy` (already a shared key) toggles enemies, `Npc` toggles regular NPCs.
- 2026-11-17 — NPC ESP v1 (SP-T010): new `src/SparklingNpcESP.lua` card on the ESP tab, wired in `tools/build_public.ps1` after `SparklingESP.lua`.

- 2026-11-17 — Combat page v3 (SP-T008): Auto Parry moved from Auto to a new Combat tab in Hunter X Enter section style. Adds a range slider, PVE row, keybinds and the Don't Parry If options; player attackers are excluded. Diagnostics v2 format unchanged apart from the new SKIP reasons and Config fields.

- 2026-11-17 — Created per-game handoff structure; consolidated SP-001–005 and SP-T001–007. Diagnostics v2 replaces the v1 rolling 400-line log with bounded session reports, shared Action observation, Save/Copy controls, explicit outcomes and skip reasons. Defense IDs/delays/range/hold/pulse gap remain unchanged. Failed F-release retries are spaced by 0.1 seconds and reported; final held state is retained.


## Current targeting — Combat page v4 (2026-11-17)

The user explicitly requested player detection and a PvP checkbox for Sparkling. This supersedes the earlier NPC-only decision recorded in SP-T008 **for Sparkling**; other games' scope is unchanged. This is the current targeting behavior; prior NPC-only findings remain historical evidence.

- **Combat → Auto Parry → PVP (Players)** independently enables player-owned attackers; **PVE (NPCs)** enables unowned NPC models. PvP defaults OFF for existing installations, PvE defaults ON. Both options and their keybinds persist; Auto Parry itself stays OFF on reload. Both checked means both types; neither checked means no automatic responses.
- Ownership uses `Players:GetPlayerFromCharacter`, not display names. Discovery merges current Player.Character models with Workspace.Live and deduplicates models. It excludes self and stale/departed player characters; a known former player character cannot become an NPC target after respawn/removal.
- The Auto Parry section shows nearby player/NPC counts within the observation range (max of 30 studs or configured range + 10). Counts describe discovered living characters, not hostile players, mapped attack coverage, or successful parries.
- Eligibility is checked on observation **and again before sending F**, so switching a type off cancels its queued response. An already-issued F pulse still receives its scheduled release. Range, facing, LOS, known animation IDs and other skip options remain in effect. Player detection does not infer hostility or teams.
- Diagnostics include `targetType=Player/NPC` on actors/cues/watch entries, `TARGET_COUNTS`, `pvp/pve` config fields and `pvp_off/pve_off/stale_player_character` skip reasons.

### SP-T011 — PvP/PvE targeting checks

2026-11-17, Combat page v4: 26 mocked target/filter assertions plus 3 saved-setting/reload assertions passed. Cases include default PvP OFF, ON permitting mapped player cues, independent PvE gating, both OFF, deduplicated discovery, players outside Live, queued checks after toggling, respawn/removal, name-independent NPC classification, self exclusion, persistence and disabled-on-reload. The 47 existing logger assertions also passed. Changed source and public/private variants compile. **Implemented / untested live**, not a new parry-success claim.

Reference: Beyond Nen's player-ownership target filtering (see [Beyond Nen note](beyond-nen.md), `src/BeyondNenAutoParry.lua`) and the existing Sparkling checkbox/keybind/settings helpers. No Beyond Nen attack timings or remote semantics were transferred.

Next check: reload, enable PVP (Players) and Auto Parry, verify nearby counts, then inspect mapped player cues and F release in the log. Toggle PvP off and confirm `pvp_off` skips. Unknown animations remain unmapped; enabling PvP does not add fighting styles.


## Current mode — record only (Combat page v5, 2026-11-17)

The user asked to stop automatic parrying and only record for now. This supersedes active-input behavior in v1–v4, including the v4 targeting instructions above. **Do not restore generated combat input until the user requests it.** Existing PvP/PvE selection and skip options now describe hypothetical decision eligibility.

Use **Combat → Auto Parry // RECORD ONLY → Record Attacks**, fight/parry manually, then turn Record Attacks off to finalize. Save Log / Copy Log remain in Recorders. The recorder stays OFF on reload; PvP/PvE options, range, skip settings and keybinds retain their saved values. No mode setting or checkbox enables real parrying in this version.

- The Sparkling parry module no longer obtains VirtualInputManager or contains SendKeyEvent calls, a generated key-down/key-up path, or a gameplay-remote send path. Shared hooks only observe and forward existing game calls.
- An eligible known attack logs `WOULD_PARRY proposal=N generatedInput=false`. The 0.12-second hold and 0.3-second pulse spacing are simulated decision gates, explicitly named in reports; they do not hold a key. Unknown animations are still recorded and logged as unmapped.
- `SELF_COUNTER` and `SELF_HEALTH` list recent hypothetical proposals for comparison. Attribution is `none_record_only`; counters are never credited to the recorder. Observed Action and INPUT events originate outside this module and are not proof of server acceptance.
- Header: `SPARKLING AUTO PARRY DIAGNOSTICS v3 // RECORD ONLY`; Config declares `mode=record_only generatedInputs=0`. Totals distinguish `proposedParries`, observed self-counter/damage events, and `generatedInputs=0`. Old `F_DOWN/F_UP` events and actual-attempt success counters are not emitted.
- Latest/history filenames, capture limits, five-second autosave, manual combat recorder exclusion, Save/Copy behavior, and stop lifecycle are unchanged. Reloading is required to replace an already-running older active version.

### SP-T012 — passive recorder validation

2026-11-17, Combat page v5 / diagnostics v3: 43 mocked behavioral assertions plus 4 reload/settings assertions passed. They cover eligible PvP proposals, zero key-down/up calls, manual input and outgoing Action observation, no counter attribution, simulated gates, PvP toggles, unmapped attacks, cooldown/manual-F/menu/typing gates, history/autosave/copy on disk failure, report caps, focus loss, teleport, death, unload and saved settings. The module starts without a virtual-input service. Source and public/private builds compile. **Implemented / untested live.** No live parry-performance claim is made.

Reference: the passive observation principle in [Beyond Nen's recorder](beyond-nen.md) and existing Sparkling diagnostics/hooks. This change removes input generation rather than relying on a saved toggle to disable it.

Next check: reload, start Record Attacks, perform a short manual fight and stop recording. Verify `mode=record_only`, hypothetical proposals and observed manual actions, and no generated input events. Read the saved report before changing attack mappings or restoring automation.


## SP-006 / SP-T013 — main-world combat capture reviewed (2026-11-17)

Evidence label: **main-world manual combat capture, start Unix 1794947703**, place `110405289082842`, passive combat recorder v1. Raw file remains local under the standard `sparkling_combat_<timestamp>_<nonce>.txt` pattern; no player identifiers or raw capture are published here. This is the new **Combat Recorder** session, not a diagnostics-v3 Record Attacks session. The latest auto-parry report at review time was still the older 13:25 local session; this capture does not validate loading the record-only UI.

### Capture completeness and observations

- Completed normally at **120.0068 seconds**, `STATUS=TIME_LIMIT`; 15,530 text lines, 2,317,863 bytes. It did not reach the 16,000-line / 4 MB cap. No error events appeared.
- Three discovery bounds were reached: 1,000 actor objects, 600 GUI objects/depth 12, and 400 character-stat objects. Missing actors/stats/UI remain inconclusive even though recording completed its duration.
- Hook coverage advertised namecall plus direct FireServer. All **548 outgoing observations** in this capture used namecall and had matching RETURN observations. No direct-hook OUT duplicates were observed. A RETURN is not server acknowledgment.
- **106 F DOWN and 106 F UP input observations**, plus **106 `Defend,true` and 106 `Defend,false` Action observations**. Input origin cannot be proved from these signals alone; the recorder itself is passive.
- **18 Counter Parry messages total: 8 targeted the local character, 10 targeted other characters.** No Perfect Counter message appeared. Self-targeted receipt times: 17.9808, 53.2563, 56.9077, 78.2704, 81.7938, 92.0078, 101.4314, 116.7551 seconds. These are observed result messages, not an automatic-parry success rate.
- Local health moved from the initial motion sample's 78.2255 to 26.1238. There were 81 observed negative health transitions (about 52.2297 total decrease, offset slightly by positive transitions). Damage causality, attacker and relation to any specific block are not established by totals.
- Named rigs under `Workspace.NPCs` were discovered, but none produced recorded ANIM_START events. This session still does not establish NPC attack mappings. Actor IDs are capture bindings, not unique people: the same path may be rebound under another ID.

### Five-hit combo mapping evidence

For each animation start, correlate the most recent same-actor `Character.Combo` change in the preceding **10 ms**, preserving capture event order. The following repeated associations include the local character and nearby live rigs; they strongly support this combo sequence in the observed fighting style, not all styles.

| Observed Combo | Animation ID | Same-actor close-time associations |
| --- | --- | --- |
| 1 | `114493250337466` | 38 |
| 2 | `84394230178792` | 37 |
| 3 | `95937874840716` | 32 |
| 4 | `139487903747364` | 25 |
| 5 | `131649056441662` | 20 |

Locomotion and other tracks sometimes started near the same attribute change; the table records strong repeated associations, not a claim that every simultaneous track is an attack. Existing hypothetical parry mappings remain the two IDs for combo 2 and 3; this review did not change code or enable input.

### Self-counter correlations

These rows select the latest preceding **non-self animation among the five candidate IDs** within one second of each self-counter receipt. Selection does not establish which actor or attack caused the result. Other animation/state events may coexist.

| Self-counter receipt (s) | Candidate animation | Candidate start (s) | Client receipt interval (s) |
| --- | --- | --- | --- |
| 17.9808 | `84394230178792` | 17.7775 | 0.2033 |
| 53.2563 | `95937874840716` | 53.0967 | 0.1596 |
| 56.9077 | `139487903747364` | 56.8287 | 0.0790 |
| 78.2704 | `95937874840716` | 78.1542 | 0.1162 |
| 81.7938 | `139487903747364` | 81.7209 | 0.0729 |
| 92.0078 | `139487903747364` | 91.9413 | 0.0665 |
| 101.4314 | `84394230178792` | 101.1637 | 0.2677 |
| 116.7551 | `95937874840716` | 116.6235 | 0.1316 |

**New candidate:** combo-4 animation `139487903747364`, supported by 25 close-time Combo=4 associations and three later self-counter messages. The 66.5–79 ms intervals are notification delays after a client animation observation, not server parry windows or recommended input delays. In some exchanges Defend was already pressed before that animation; do not tune an automated delay from these intervals.

### Additional mechanics / unresolved points

Outgoing Action observations included MouseButton1 (205), MouseButton2 (28), AerialM1 (15), Rush (9), ZVanish (5), OffMain (2), and Ki Blast (1), plus Update (40), UpdateRegion (9), Descend (3) and the Defend pairs. Argument semantics beyond previously established mappings remain unverified.

Incoming CooldownNT advertised Counter=0/0.5/5 seconds, Heavy=8, ZVanish=1/2/10, AerialM1=5, Weave=1, Rush=25, KiBlast=2. These differ from some tutorial observations, notably Heavy=2 and ZVanish=3. Treat durations as context-dependent client notifications; do not hardcode one earlier capture's value as universal behavior.

Verdict: **capture completed; repeated combo associations and self-counter observations found; defense timing and causality still inconclusive.** Next investigation should compare the combo-4 candidate with input edges, phase/speed, local state, distance and health in isolated exchanges. Keep record-only mode in force. Review these existing data before requesting another capture or expanding hypothetical mappings. No live automation or success-rate claim is justified by this recording.


## SP-007 / SP-T014 — first record-only live session (2026-11-17)

Evidence label: **Record Attacks session, start Unix 1794948099**, main-world place `110405289082842`, diagnostics v3 / Combat page v5. Reviewed the timestamped history and latest report, both 458,758 bytes. Raw files stay local under `AnomalyHub/universal/sparkling_auto_parry_<timestamp>_<GUID>.txt`; public notes omit player identifiers.

### Result and important configuration

- Stopped manually at **97.3245 seconds**, `STATUS=Disabled.`. The header confirms the record-only version was loaded. Totals: **0 proposed parries, 0 generated inputs, 6 self-counter messages, 43 negative health events, 0 dropped lines**. No ERROR, LOGGER_ERROR, FRAME_GAP or capture-limit events appeared.
- **Both `pvp=false` and `pve=false` throughout the session**; no SETTING events changed them. Range=8, watchRange=30, all three optional skip settings false. Therefore eligible would-be parry decisions were deliberately gated off; this was not a failure to record attacks.
- All 417 animation starts received skip entries: **24 `pve_off`**, **393 `unmapped_animation`**. Unmapped includes locomotion/idle/other tracks, so 393 must not be described as 393 missed attacks. The 24 mapped NPC cues were gated before geometry evaluation; this run cannot validate their range/facing/LOS or hypothetical input scheduling.
- 351 starts were classified NPC, 66 Player. Player/NPC ownership classification and nearby counts appeared in the report. The observer continued recording animation evidence even with both target filters disabled.
- 321 outgoing Action observations and 321 matching returns, through the namecall path; direct FireServer was advertised in coverage but no direct OUT duplicates were seen. Included 84 `Defend,true` / 84 `Defend,false` observations and matching 84 F DOWN / 84 F UP input observations. Those inputs originate outside this recorder; source is not proved to be manual by these events alone. No F_DOWN/F_UP generated-input events were recorded.
- Six local `Counter Parry!` receipts at **10.7785, 14.6842, 18.7138, 50.9944, 62.2250, 65.3536 seconds**, each with `recentProposalCount=0` and `attribution=none_record_only`. These are observed results, not successes credited to automatic parrying.

### First directly watched NPC combat in the reviewed evidence

`Workspace.Live.Thug5` was classified NPC and actually watched playing attacks. Actor IDs 7 and 17 refer to bindings with that same path at different times, not proof of two unique enemies. Other NPC names in incoming EFFECT actor entries alone, including Cell and Thug6, do not establish their attack behavior.

| Candidate combo association from SP-006 | Animation ID | Thug5 start observations across both bindings |
| --- | --- | --- |
| 1 | `114493250337466` | 14 |
| 2 | `84394230178792` | 12 |
| 3 | `95937874840716` | 12 |
| 4 | `139487903747364` | 9 |
| 5 | `131649056441662` | 8 |

This establishes NPC reuse of these animation assets. The combo-number associations came from the preceding manual combat capture; this diagnostic log does not sample the NPC's Combo attribute, so it is not an independent measurement of every NPC combo state.

Three self-counter messages followed Thug5's `95937874840716` starts by **0.1351, 0.1179 and 0.0667 seconds**. One followed `131649056441662` by **0.1588 seconds**; another followed `139487903747364` by **0.2632 seconds**. The last self-counter message had no preceding observed candidate among the five IDs within the chosen 0.75-second comparison window. These are client-time correlations with potentially multiple actors/events present, not identified server attack outcomes or input-delay recommendations. The fifth-hit association is one observation, insufficient to tune a new mapping.

### Verdict and next step

**Live recording success:** the new passive report captured NPC/player animations, external input/actions, damage, and self-counter messages with no generated inputs/errors/drops reported. **Hypothetical decision path not tested live in this session:** both target filters were OFF. This updates the earlier “untested live” status for recording, not for parry performance.

For a future NPC decision-recording test, enable **PVE (NPCs)** while leaving **RECORD ONLY** mode in place. Enable PVP only when evaluating player cues. These options select which cues can produce WOULD_PARRY proposals; neither enables real input in v5. Preserve this capture as evidence before requesting more data. Do not restore active input or guess new attack delays from the receipt intervals.


## SP-008 / SP-T015 — target-enabled decision recordings (2026-11-17)

Evidence: main-world diagnostics-v3 Record Attacks histories starting Unix **1794948410** and **1794948539**. Both report `mode=record_only`, `pvp=true`, `pve=true`, range=8, watchRange=30 and all three optional skip settings false. The latest report was 724,217 bytes. Raw histories remain local; no player identifiers are published.

The user clarified that attack mappings are shared between players and NPCs. Maintain one animation catalog; PvP/PvE selects eligible ownership, not separate attack definitions. Observed differences in speed, phase, geometry and state still matter for individual encounters.

### Capture results

| Session start | Duration / stop | Proposals | Self-counter messages | Negative health events | Generated inputs / dropped lines |
| --- | --- | --- | --- | --- | --- |
| 1794948410 | 51.360 s; stopped on focus loss | 1 | 2 | 8 | 0 / 0 |
| 1794948539 | 130.843 s; disabled manually | 5 | 2 | 30 | 0 / 0 |

The latest session has no ERROR/LOGGER_ERROR or capture-limit events. One 0.5026-second frame gap appeared at 57.9527 seconds, before its five proposals. All five proposals refer to the watched Thug5 NPC. This is live evidence that the enabled **hypothetical decision path** runs; it is not a test of generated blocking or parry success. Earlier-session focus-loss stopping is expected lifecycle behavior.

### Latest run: decision coverage

- 700 ANIM_START events; **42** used the five combo IDs catalogued in SP-006, all classified NPC. Of those, 16 used the currently mapped combo-2/3 IDs. The remaining **26 known combo starts** were still skipped as unmapped: combo 1 = 12, combo 4 = 7, combo 5 = 7.
- The 16 mapped starts yielded five proposals and 11 rejection reasons: Counter cooldown 5, attacker facing 4, self facing 1, line of sight 1. Ten cues were queued; five of those were rejected at execution for the observed cooldown. No PvP/PvE-off skips occurred.
- Overall `unmapped_animation=684` includes the 26 known-but-not-yet-mapped combo starts plus locomotion, idle and other unclassified tracks. It must not be described as 684 distinct missed attacks.
- Observation starts often showed speed=1.000, while all five accepted hypothetical decisions showed speed=1.150 with nonzero animation phase. Fixed wall-clock delays alone do not describe those changing phases. Do not infer a new delay without aligning phase/speed, input and outcome evidence.

### Five proposals versus subsequent observed damage

| Proposal time (s) | Animation / combo association | Distance (studs) | First later negative-health event (s) | Interval (s) |
| --- | --- | --- | --- | --- |
| 81.3282 | `84394230178792` / 2 | 2.500 | 81.5131 | 0.1849 |
| 93.1084 | `95937874840716` / 3 | 4.778 | 93.1921 | 0.0837 |
| 98.2531 | `95937874840716` / 3 | 2.878 | 98.4163 | 0.1632 |
| 105.7300 | `95937874840716` / 3 | 3.061 | 105.8138 | 0.0838 |
| 124.4151 | `95937874840716` / 3 | 2.519 | 124.5658 | 0.1507 |

Each first damage event was about -1.5991 health. These are chronological associations, not identification of the damaging attack or evidence that hypothetical inputs would succeed/fail. **No F press was generated.** Observed next Defend-down calls occurred 0.0336, 0.0878, 0.1293, 0.4390 and 0.8267 seconds after those proposals respectively, so the recorded external input is not equivalent to executing each proposal. The earlier session's proposal at 9.1018 seconds likewise preceded damage by about 0.4352 seconds.

The latest session had 60 observed Defend-down and 60 Defend-up outgoing calls. Its self-result messages were **Perfect Counter! at 118.4274** and **Counter Parry! at 125.6916**; neither had a proposal in the preceding one-second report window. The latter followed unmapped combo-5 animation `131649056441662` at 125.5392 by 0.1524 seconds. Around the Perfect Counter, unmapped animation `126361551838373` started at 118.0286 (0.3988 seconds earlier); its mechanic/type is unknown. Do not invent a heavy-attack classification or causal mapping from this single event.

### Line-of-sight investigation

The LOS rejection at **81.9191 seconds** named `Workspace.Thrown.Zvanishdummy.HumanoidRootPart` as the obstacle, with attacker distance 3.970 and both facing dots passing. The earlier session logged the same obstacle at **1.7510 seconds**, distance 4.027, shortly before a self-counter at 1.8384. This is repeated evidence that the raycast can hit a Z-vanish dummy rather than level geometry. The dummy's collision/query properties and intended treatment are not captured; inspect before broadly excluding folders.

A separate queued cue at 70.2810 logged an obstacle under `Workspace.Visuals.anotherHead<local-character>` but was rejected first for Counter cooldown. Preserve both facts: this is an additional possible visual obstruction, **not** an extra counted LOS rejection. Never publish the embedded character identifier.

### Verdict and next implementation priorities

**Live hypothetical-decision logging verified; actual parry performance remains untested and input generation stays disabled.** Record-only operation, both eligibility options, decision reasons and lifecycle produced usable evidence without dropped lines. No code or settings changed during this review.

Use these existing recordings before asking for another generic test. Next work should (1) inspect and handle confirmed cosmetic LOS obstructions while preserving real geometry, and (2) distinguish recognized combo-1/4/5 candidates from truly unknown animations in the shared passive catalog, without fabricating timing or claiming they are already eligible parries. The evidence also motivates phase/speed-aware analysis. Do not re-enable automatic input or present damage-after-proposal as a measured success/failure rate.


## SP-009 / SP-T016 — shared player/NPC animations and repeated gate cases (2026-11-17)

Evidence: diagnostics-v3 Record Attacks history starting Unix **1794948713**, main-world place `110405289082842`, 298,885 bytes. Stopped manually after **43.5638 seconds**. Both PvP/PvE ON; range=8, watchRange=30; optional knocked/off-screen/typing gates OFF. Raw capture remains local.

Totals: **3 hypothetical proposals, 0 generated inputs, 4 self-counter messages, 18 negative health events, 0 dropped lines**. No errors, frame-gap or limit events. Of 314 animation starts, 294 were unmapped, 11 out of range, 2 attacker-facing rejections, 3 execution-time Counter cooldown rejections, 1 LOS rejection, and 3 proposals. There were no target-type-off rejections.

### Shared animation evidence and coverage

This single session directly records the same five candidate combo IDs on both **Thug5 (NPC)** and a **player-owned character**. This strengthens the user's instruction to maintain one shared animation catalog. It does not establish equal effective speed or timing in every encounter.

| Combo association | Animation ID | NPC starts | Player starts |
| --- | --- | --- | --- |
| 1 | `114493250337466` | 8 | 7 |
| 2 | `84394230178792` | 4 | 7 |
| 3 | `95937874840716` | 3 | 6 |
| 4 | `139487903747364` | 2 | 5 |
| 5 | `131649056441662` | 2 | 2 |

All 13 mapped player cues were evaluated through the same filter path: 11 were outside 8 studs (8.926–25.647 observed), and 2 failed attacker-facing (dots -0.279 and -0.991). No proposal against a player is expected from those gates. The absence of player proposals is not evidence that PvP detection failed. The 26 starts from known combo IDs 1/4/5 are still counted as unmapped, mixed with movement/other tracks.

### Three proposals and observed outcomes

| Proposal time | Animation | Target / distance | Subsequent negative-health observation |
| --- | --- | --- | --- |
| 9.1628 s | `84394230178792` | Thug5 / 4.428 studs | -1.5991 at +0.1110 s |
| 18.7421 s | `95937874840716` | Thug5 / 3.161 studs | -1.6020 at +0.1575 s; another at +0.5058 s |
| 35.4884 s | `95937874840716` | Thug5 / 6.460 studs | No negative-health event in the next 1 s |

All three proposal tracks were running at speed=1.150 during evaluation. Neither damage after a proposal nor no damage afterward measures automatic-defense success: no input was generated, and the responsible attack/outcome is not established.

Self `Counter Parry!` messages occurred at **4.5348, 9.7366, 11.7572, 19.8294 seconds**. Only 9.7366 had a proposal within the preceding second (proposal 1, about 0.574 s earlier), and the report correctly kept `attribution=none_record_only`. A later combo-3 animation occurred just 0.0901 s before that counter, so assigning it to the earlier combo-2 proposal would be especially misleading. At 11.7572 both player and NPC candidate attacks were present nearby; do not pick an attacker from nearest timing alone. Combo-1 and combo-5 associations remain observations, not new timing measurements.

### Repeated LOS obstruction and cooldown scheduling

- At **9.6467 seconds**, combo-3 was rejected at 4.815 studs despite passing both facing checks. The obstacle was **`Workspace.Thrown.Zvanishdummy.Left Arm`**. Earlier captures hit that dummy's HumanoidRootPart. Any validated cosmetic-dummy exclusion needs to consider its model/descendants, not only one part name. The self-counter at 9.7366 followed the blocked cue by 0.0899 s; this remains correlation. Collision/query properties and the intended gameplay role of the dummy have not been captured.
- At **34.6847 seconds**, a queued combo-2 cue was removed with **0.0030 seconds** of observed Counter cooldown remaining. Cue age=0.0708 s, expiry=0.25 s; track still playing, distance=6.729, both facing dots pass, LOS clear. The current one-shot decision discards rather than defers such a cue. This is a concrete scheduling case for a bounded recheck after cooldown expiry, with all gates revalidated; it does not prove a later press would be successful. Other cooldown rejections had 0.2172 s and 2.9118 s remaining, so do not indiscriminately defer all cooldown skips beyond cue expiry.

Verdict: **passive capture and shared player/NPC evaluation observed working; repeated coverage/LOS/scheduling limitations recorded; no active-input performance result.** No implementation or settings changed during this review. Next work should use this evidence to distinguish recognized attack candidates, inspect cosmetic LOS blockers, and assess bounded cooldown rechecks. Keep recording-only mode until the user requests otherwise; another generic capture is not needed to establish these existing cases.


## SP-010 / SP-T017 — latest passive evidence and active timing test (2026-11-17)

### New user instruction and current mode

The user explicitly requested: consider opponents parrying them, build active auto parry to test timings, and continue recording. **This supersedes the previous temporary record-only instruction.** Current implementation is **Combat page v6 / diagnostics v4 // ACTIVE TIMING TEST**, off on every reload. Enable it from **Combat → Auto Parry // TIMING TEST → Enable Auto Parry**. Diagnostics start automatically and retain Save Log / Copy Log under Recorders. Disabling finalizes the history. No game session was controlled during this change.

Both chats may edit the same source and generated builds. This change is scoped to `src/SparklingAutoParry.lua` plus its exact generated-module replacement and game notes. The latest concurrent Ki-bar `Character.Ki@Max` change was retained. Recheck source/file hashes and remote HEAD before future installations; do not replace a newer entire build with a stale build.

### Latest recording evidence

Reviewed the passive report starting Unix **1794948819**, main-world place `110405289082842`, 340,074 bytes. It ended on focus loss at **55.566 seconds**, with 4 proposals, 3 self-counter messages, 15 negative health events and 0 generated inputs/dropped lines. Self-counter receipts: 9.6099 (Counter Parry), 15.8153 (Perfect Counter), 49.2749 (Counter Parry). These remain passive observations, not active parry successes.

There were **20 counter messages targeting other actors**. Five counter receipts on the nearby combat NPC's actor binding (7.9314, 14.1067, 30.4757, 34.7684 and 47.5751 seconds) were followed within roughly 4 ms by a local `Counter=0.5` notification, alongside recent outgoing local attacks. Other actors also produced counters; a sixth nearby-time half-second notice occurred around another actor's counter. The existing report lacks geometry at each recipient-counter receipt, so it cannot establish which opposing counter involved the local player. The repeated local short-cooldown association motivates a carefully labeled correlation, not a definitive “parried me” verdict.

### Shared active trial catalog and controls

| Combo association | Animation | Trial delay before F |
| --- | --- | --- |
| 1 | `114493250337466` | 0.04 s |
| 2 | `84394230178792` | 0.04 s |
| 3 | `95937874840716` | 0 s |
| 4 | `139487903747364` | 0 s |
| 5 | `131649056441662` | 0.04 s |

**These are experimental starting parameters, not measured server parry windows.** Combo 2/3 keep the existing seeds; 1/4/5 use explicitly uncalibrated trial seeds based on the observed catalog and early-input experiment. Player and NPC ownership share this one mapping. Other animations remain logged/unmapped. Track phase/speed, client scheduling lateness and geometry are recorded at execution; the scheduler does not claim to compensate perfectly for speed or network latency.

- **Timing offset (ms):** default 0, bounded -80 to +150; added to the catalog seed, with final delay floored at zero. Negative values schedule earlier, positive later. A timing edit clears queued cues rather than mixing schedules.
- **F hold (ms):** default 120, bounded 60–250. Settings and PvP/PvE preferences persist; enabled state does not. Pulse gap remains 0.3 s, cue expiry 0.25 s.
- Requires living/current target, selected PvP/PvE type, configured range, facing, clear LOS, playing/non-reused track, no manual F, observed Counter cooldown and optional knocked/off-screen gates. Text entry and game menus **always** pause/release active F; the old record-only SkipTyping preference no longer allows typing into a focused field.
- Input API failures stop the experiment and attempt F release even if key-down may have partly succeeded. Release errors are logged and retried, including stop/unload attempts. Focus loss, death/character change, teleport, unload and starting a manual combat recording stop it.

### Opponents parrying the local character

Every non-self Counter Parry / Perfect Counter message now logs `OTHER_COUNTER` with recipient actor, distance, mutual facing, time since the latest observed local attack request and `attackerKnown=false`. This includes unrelated fights without counting them as the local player's failed attack.

`POSSIBLE_PARRIED` requires all of: a recent local attack request (≤0.6 s); recipient within min(configured range, 12 studs); both facing gates; and a local **Counter=0.5** notification within ±0.2 s of the other counter. MouseButton1, non-Up MouseButton2, AerialM1 and Rush request observations supply the attack evidence. Either receipt order works; a local notice is consumed at most once. Without outgoing hook coverage this specific correlation is unavailable, while incoming cooldown and other-counter logging still work.

The label remains **heuristic_only**, since Effects does not identify the attacker. On a match, held F is released and queued cues observed before the opponent counter are discarded. The code honors the actual incoming cooldown duration; it does **not** invent an extra stun time or extend the cooldown. Self-result events are separate and list recent actual attempts only as temporal candidates; they are not counted as proven input successes.

### Prior gate cases addressed

- Counter cooldowns with ≤80 ms remaining can defer a cue, but only if expiry still permits it, at most four rechecks. All target, geometry, playing-track, input and state gates are revalidated afterward. Long cooldowns/expired cues remain skips. This covers the prior 3 ms remainder case without indefinite replay.
- LOS can skip the exact **Model named `Zvanishdummy` directly under `Workspace.Thrown`**, including its limbs/root, then raycast again with a five-iteration bound. It logs ignored models. A wall behind it still blocks. Same-name models elsewhere, other characters, all of Visuals/Thrown, and arbitrary effects are **not** broadly excluded. This is a narrow trial based on repeated dummy obstructions; actual live collision behavior remains to validate. The separately observed `anotherHead<local-character>` obstruction is still not bypassed.

### Diagnostics, validation and handoff

Report header is diagnostics **v4 // ACTIVE TIMING TEST** with trial seeds, timing offset, hold duration and gates. Events include `CUE_ACCEPTED`, `F_DOWN`, `F_DOWN_FAILED`, `F_UP`, `DEFER`, `OTHER_COUNTER`, `POSSIBLE_PARRIED`, observed ACTION/INPUT, health, clocks and stop status. F_DOWN means the API returned, not that the server accepted a defense. The outgoing Action's `duringAttempt` field is contextual correlation; hooks can duplicate observations. Totals separate F attempts, self-counter messages, other-counter messages, possible-parried correlations and damage events. Latest/history filenames and limits remain unchanged.

**Validation:** 121 mocked assertions passed (47 logger/lifecycle regressions, 42 active-catalog/timing/LOS/counter tests, 26 PvP/PvE tests, 6 settings/migration checks). Changed source and public/private variants compile. No live active timing test was performed by this chat. Mock checks include walls behind dummies, near-expiry cooldown rechecks with changed geometry, all five combo IDs, both target types, recipient-counter pairing in both orders, unrelated/distant/stale/wrong-duration exclusions, key-up cleanup and timing bounds.

Next: reload the updated hub; retain offset=0 and hold=120 for the first short active trial so the starting configuration is reproducible. Enable desired target types and Auto Parry. Disable it after the exchange to finalize. Inspect actual F_DOWN/observed Defend/F_UP, damage and self/other counter timing, deferrals and release errors before changing one timing parameter at a time. Do not equate a counter-message ratio or absence of damage with confirmed success. Keep raw captures and player identifiers local.


## SP-011 / SP-T018 — first active baseline results (2026-11-17)

Reviewed diagnostics-v4 histories starting Unix **1794949315** and **1794949417**, both in main-world place `110405289082842`. Configuration: active test, PvP/PvE enabled, range 8, offset **0 ms**, hold **120 ms**, no setting changes. Raw captures remain local.

| Session | Duration / stop | F attempts | Self-counter messages | Other-counter messages | Possible-parried correlations | Negative health events |
| --- | --- | --- | --- | --- | --- | --- |
| 1794949315 | 66.404 s / focus loss | 2 | 1 | 8 | 0 | 17 |
| 1794949417 | 67.990 s / manual disable | 19 | 2 | 5 | 3 | 36 |

Both ended with `held=false`, zero dropped lines, no error/input-failure/release-failure/frame-gap events. All 21 attempted F-down calls had one corresponding observed `Defend,true`; all 21 F-up API calls succeeded and were followed by observed `Defend,false`. All actual attempts in these two sessions targeted Thug5; other player candidates were observed but did not produce attempts.

### Important input-path timing

Observed request timestamps are **client hook receipt**, not server receipt. Pair each true call using its `duringAttempt` ID, then pair the first false call within 150 ms after the corresponding successful F_UP. All 21 releases have such a false call.

| Measurement | Earlier session | Latest session |
| --- | --- | --- |
| F_DOWN log → observed `Defend,true` | 40.7–51.5 ms | **37.9–59.2 ms**, median 46.6 ms |
| F_UP log → observed `Defend,false` | 0.9–1.3 ms | 0.9–2.6 ms, median 1.1 ms |
| Observed true → false request span | 83.6–92.9 ms | **81.4–115.6 ms**, median 91.1 ms |
| Actual API-key hold from F_UP field | 123.1–143.9 ms | 123.7–160.7 ms |

This is evidence of a delayed game request after the synthetic F press and asymmetric press/release processing. A 120 ms configured key hold does not mean a 120 ms observed Defend-request span. The logs do not establish why the delay occurs or its server-side effect. Do not “correct” a delay simply by subtracting network ping.

**Logging limitation:** delayed false requests have `duringAttempt=none` because the module clears its held flag after the F-up API succeeds, before the game processes the event. That does not mean release was absent. Future request correlation should retain recent release IDs/timestamps and explicitly mark temporal matches without claiming manual/synthetic origin is proven.

### Outcomes and attribution limits

The earlier self counter at 17.9765 followed attempt 1 by about 0.3435 s. The latest session's two self counters are both ambiguous: at **17.9543**, attempts 6/7/8 fell in the preceding second; at **38.9380**, attempts 14/15 did. Counter-message count divided by attempt count is not a valid measured success rate.

In the latest session, 16 of 19 attempts were followed by a negative-health observation within 0.5 s, and 17 within 1 s. Some successive attempts share the same later damage event; these are not independent failures or proof of which attack caused damage. Attempts 8 and 19 had no negative-health event in the next second, which likewise does not prove success. The baseline is **not established as reliable** despite functioning input and logging.

Three `POSSIBLE_PARRIED` events occurred at **28.6463, 37.2402, 41.5476 seconds** against the nearby NPC binding. Recent local attack ages were about 0.044, 0.100 and 0.180 s; distances about 3.305, 6.001 and 6.000 studs; the paired local half-second cooldown notices followed the other counter by about 1.7–2.1 ms. These meet the implemented correlation rules, but attacker identity remains unconfirmed. No key was held at these three moments, so this session verifies correlation logging, not a live counter-triggered key-release path.

### Remaining obstructions / decisions

The latest session logged **three** LOS rejections, all naming `Workspace.Visuals.anotherHead<local-character>` at **12.0895, 35.4161 and 63.3236 seconds**, at distances 6.335, 2.057 and 7.216 studs with facing checks passing. The earlier session also logged two of these head-effect obstructions. The current narrow Z-vanish-dummy exclusion deliberately does not cover this separate object; no successful proposal in these sessions logged an ignored Z-vanish dummy, so its live benefit is not demonstrated here.

Other earlier-session LOS blockers were a real nearby NPC's arm/GripBrick and a map tree's Leaves. Do not fix visual obstructions by ignoring every rig or all level geometry. Inspect/identify the exact local head-effect object and scope any exclusion to it.

Latest mapped rejection reasons besides LOS: Counter cooldown 10, attacker facing 4, manual/input F already down 2, out of range 8. There were no DEFER events in either session, so short-cooldown rechecks remain mock-validated only. Existing shared combo coverage remains enabled.

Verdict: **live active input/cleanup/request observation and opponent-counter correlation logging verified; timings and actual parry reliability unproven.** No code or timing values changed during this review. Next priority is the repeatedly observed head-effect obstruction and better delayed-request/release correlation, then timing trials that account for the measured input-to-request lag. Preserve this baseline and change one timing variable at a time; do not infer reliable timing from raw counter ratios.


## SP-012 / SP-T019 — repeated active tests, early releases and live deferral (2026-11-17)

Reviewed three finalized diagnostics-v4 histories from main-world place `110405289082842`. All retain range 8, PvP/PvE enabled, offset 0 ms and hold 120 ms. These are user-run live captures; no runtime changes or game control occurred during review. Raw captures remain local.

| Session start (Unix) | Duration / stop | Automatic attempts | Self counters | Other counters | Possible parried | Negative-health events |
| --- | --- | --- | --- | --- | --- | --- |
| 1794949545 | 73.147 s / disabled | 10 | 4 | 6 | 3 | 33 |
| 1794949665 | 4.601 s / focus loss | 0 | 0 | 0 | 0 | 24 |
| 1794949677 | 65.343 s / disabled | 7 | 2 | 19 | 2 | 26 |

All reports end held=false with zero dropped lines. All 17 automatic key-up API calls report success; each press has an observed Defend=true and a subsequent Defend=false. No error/input-failure/frame-gap events were present. All attempts target the same nearby NPC binding, not players. Six of ten earlier attempts and five of seven latest attempts precede negative-health observations by at most 0.5 s. This is temporal association, not an independent failure rate or attacker attribution.

### Input interleaving changes the interpretation

Two combo-3 (`95937874840716`) trials had close self-counter receipts: earlier attempt 1 at 11.4695 s followed by Counter Parry at 11.5617 s (~92 ms), and latest attempt 5 at 38.8015 s followed by Counter Parry at 38.9104 s (~109 ms). Both tracks ran at speed 1.15; distances were 2.828 and 4.703 studs. These are repeatable temporal candidates, not proven automatic successes: multiple F/Defend cycles outside automatic attempts occur immediately beforehand.

In both cases, an INPUT F-up / Defend=false occurs **before** the module's scheduled F_UP: 11.5700 versus 11.6148 s, and 38.9173 versus 38.9665 s. The logger still considers F held until its deadline. The input origin remains unknown; do not assert the user manually released it or that the game forcibly released it without more evidence. This demonstrates interleaving between observed input/request state and the automation's internal held state.

The previous review's shortcut of pairing only false requests after F_UP is insufficient for these new cases. Pair the next observed Defend request after a candidate true, require it to be false, and never cross an intervening true request. Otherwise the latest attempt 5 would incorrectly acquire the false request from a separate F cycle at 39.1042 s, producing a fabricated 249.8 ms defense span. Its actual adjacent true/false request span is 62.9 ms. Earlier attempt 1's span is 56.3 ms. These remain client request intervals, not server windows or proven input ownership.

Across the 10 earlier attempts, press-to-Defend delay is 44.2–61.4 ms; API holds 131.0–185.2 ms; adjacent true/false spans 56.3–127.8 ms. Across the latest seven, delay is 47.1–69.8 ms; API holds 120.9–165.2 ms; adjacent request spans 51.6–114.3 ms. Other than the two early-release cases, false requests follow scheduled F_UP by 0.9–4.8 ms. A configured 120 ms hold still cannot be treated as a measured 120 ms defense window.

The other earlier self counters were at 14.6305 and 28.8724 s (no automatic attempt in the preceding second), and 57.7432 s (attempt 9 about 626 ms earlier, with damage in between). The latest other self counter at 10.1212 s has no automatic candidate within one second. Do not turn total counter counts into an automatic success rate.

### First observed cooldown deferral

Earlier cue 169, combo 2, deferred at 41.1465 s with 60.8 ms Counter cooldown remaining. It passed revalidation and pressed F at 41.2568 s, cue age 153.7 ms and 49.5 ms late relative to the rescheduled deadline. Negative health arrived at 41.3036 s; observed Defend=true followed at 41.3076 s, about 4 ms later. **The bounded retry mechanism executed live; this instance did not demonstrate timely defense.** Client receipt order is not server execution order. Account for scheduling lateness and input-to-request delay before considering a delayed cue useful.

### Remaining coverage and next handoff

The earlier session has two LOS skips: the local `Workspace.Visuals.anotherHead<local-character>` effect and an unidentified `Workspace.Thrown.Part`. The latest has four: three local head-effect blockers and one Thrown.Part. Do not assume every generic Part is cosmetic or ignore whole folders. The head-effect obstruction remains a repeated, separately scoped investigation.

Possible-parried matches occur earlier at 4.4457, 27.2290 and 40.7071 s, and latest at 8.2369 and 49.0203 s. They remain recipient/attack/geometry/short-cooldown correlations, not proven attacker identities; none overlaps an automatically held F in these reports.

**Current verdict:** input generation, cleanup, diagnostics and one cooldown retry are observed live; reliable parry timing remains unproven. Before another timing adjustment, improve diagnostic correlation to capture early observed releases and intervening F cycles, inspect the exact head-effect obstacle, and use an exchange without extra F input to distinguish automatic outcomes. Preserve offset 0 / hold 120 as the baseline. No runtime code or settings changed in this review; the live session remains under the user's control.


## SP-013 / SP-T020 — mostly isolated automatic-input baseline (2026-11-17)

**Evidence:** finalized diagnostics-v4 session starting Unix 1794949846, main-world place `110405289082842`, 62.601 seconds, manually disabled. Same baseline: range 8, both target filters enabled, offset 0 ms, hold 120 ms. Totals: 10 automatic attempts, 1 self counter, 12 other counters, 0 possible-parried matches, 51 negative-health observations; held=false, zero dropped lines, no input/error/frame-gap failures. All attempted targets were the nearby NPC binding. This review changes documentation only.

**Input isolation:** all 10 automatic presses have their corresponding observed F-down, Defend=true, successful F-up API result, observed F-up and Defend=false. No additional F cycles are present before attempt 10 completes. Three extra F/Defend cycles occur at 59.2983–59.7923 s, after its release; their origin is unknown. Thus the earlier self-counter exchange is isolated from extra observed F input, while the full session is not entirely automatic-only. No outgoing local attack requests were observed, consistent with zero possible-parried matches; the 12 other counters cannot be assigned to attacks by the local character.

### Cleaner positive candidate: combo 2

| Event | Client receipt time |
| --- | --- |
| NPC combo-2 animation `84394230178792` starts | 54.2107 s |
| Attempt 9 presses F | 54.2613 s |
| Observed Defend=true | 54.3179 s |
| F-up API succeeds / observed Defend=false | 54.3831 / 54.3852 s |
| Self Counter Parry message | 54.5123 s |
| Local Counter=0.5 notification / CounterImpact effect | 54.5145 / 54.5149 s |

At the attempt, distance=4.253 studs, attacker facing dot=0.908, self dot=0.851, track speed=1.15, phase=0.115 s, LOS clear. The self counter follows the automatic press by about 251 ms and the observed defense request by 194 ms; no other automatic attempt or extra F cycle is nearby. The next negative-health observation is at 58.0790 s, 3.818 seconds after the press. **This is the strongest isolated automatic-defense candidate in this review series**, with corroborating self effects and no immediate damage. It is still client-side temporal evidence, not server-confirmed attacker identity or a calibrated success window. Other nearby actors were active. The short 67.3 ms true/false request span and counter receipt after release must not be interpreted as an exact server window.

### Reliability and repeated limits

Eight of the ten attempts were followed by negative-health observations within 0.5 s. Attempt 6's next damage was about 0.693 s later; attempt 9's was 3.818 s later. Do not label the session a confirmed 10% success rate or count each damage event as a distinct failed attempt. The earlier combo-3 positive candidates did not reproduce in this cleaner baseline: its three attempts each precede damage by approximately 113–180 ms.

Press-to-Defend delays remain 42.3–60.5 ms. Actual key holds are 121.9–171.5 ms despite the 120 ms setting; adjacent true/false request spans are 67.3–121.3 ms. Releases follow the F-up API by 0.8–4.6 ms, with no early-release cases in the automatic sequences.

Eight Counter=5 notices and one Counter=0.5 notice were observed. The first seven long notices occur roughly 0.48–0.57 s after attempts 1–7; the eighth follows attempt 10 amid the later extra F inputs. This is a repeatable timing association, not proof that five seconds means failure or that short duration always means success. The scheduler rejected 23 cues for observed Counter cooldown, with no DEFER events. Preserve actual cooldown gating rather than bypassing it to increase attempts.

All three LOS skips again name the local `Workspace.Visuals.anotherHead<local-character>` effect, at 18.1444, 33.9440 and 53.7075 s. The middle skip also had 2.588 s cooldown remaining, so removing that obstacle alone would not have made the cue eligible. Other skips: out-of-range 13, self-facing 1, unmapped animation 198.

**Handoff:** the no-extra-F exchange provides a better automatic-result candidate than the mixed-input tests, but baseline timing remains unreliable. Keep this evidence alongside SP-T019's interleaving cases. Next implementation priorities remain precise input/request correlation and inspection of the exact head-effect LOS blocker, followed by one-variable timing trials. More unchanged generic recordings are not required to establish these repeated issues. No script, live-session state or timing setting was changed during this review.


## SP-014 / SP-T021 — request correlation and scoped head-effect LOS fix (2026-11-17)

**User authorization:** proceed with the input-correlation and repeated head-effect obstruction fixes after SP-T020. Implemented in `src/SparklingAutoParry.lua`, **Combat page v7 / diagnostics v5**. The public artifact and installed private artifact receive only the exact module replacement from their current contents, preserving concurrent ESP/other changes. No game-session control or timing adjustment is part of this change.

### Request/input correlation

Recent API pulses are retained in a bounded 16-entry list after key-up. F input events now include `inputSeq`, a temporal candidate `attempt`, and an association: `api_down_window`, `api_up_window`, `early_up_while_api_held`, `ambiguous_api_window`, or `unmatched`. Receipt windows are 200 ms; an early-up candidate requires the same currently held pulse within 0.5 s and no preceding API release request. Multiple eligible pulses stay ambiguous. These are diagnostic windows, not game timing windows. API release still occurs after an early observed release, preserving cleanup.

New **DEFEND_REQUEST** events describe observed outgoing boolean Defend requests. OUT/RETURN observations for one call do not advance the pairing state twice; duplicate OUT IDs are suppressed within a bounded 64-call cache. Each input receipt can be consumed only once. A true begins a request cycle; a later true closes/replaces the prior cycle. A false consumes that current cycle and cannot be reused. A pair has at most a one-second span and cannot cross an intervening F-down. `cycleAttempt` additionally requires both receipts to correlate with the same API pulse. Unmatched, stale, conflicting and ambiguous cases retain unknown attribution rather than borrowing the nearest attempt.

Fields include `inputAttempt`, `cycle`, `requestSpan`, `pairValid`, `cycleAttempt`, `releaseState`, `apiDownAge` or `apiUpAge`, and `interveningDown`. `releaseState=before_api_up_request` exposes SP-T019's early release; delayed ordinary releases retain the candidate attempt through `after_api_up_request`. `pairValid` describes an adjacent request interval, **not a successful parry or proven synthetic input**. The existing raw ACTION `duringAttempt` field remains contextual; use the new correlation events rather than treating a post-release `duringAttempt=none` as a missing release.

SELF_COUNTER now also lists `inputConflicts` for recent attempts with a later observed F-down. Existing counter candidate counts remain temporal. Unknown request/input origin and server acceptance are explicit. Distinct hook paths may still duplicate a request under different call IDs; those do not justify inventing a successful cycle. Hooks disconnect at stop, so asynchronous post-stop request receipts can be unavailable; successful API release and missing post-stop observation remain distinct.

Reference: reused Beyond Nen's separation of API input, input receipt, request receipt and conflicting subsequent inputs (`BeyondNenAutoParry.lua`, `UnmatchedDownAfter` / result-attribution checks). No Beyond Nen remote semantics, timings, weapon IDs or server confirmation assumptions were transferred. Hunter X Enter's UI/recorder conventions remain unchanged.

### Narrow local head-effect exclusion

LOS now reraycasts past a hit only when it is the exact **BasePart named `anotherHead` + current local Character.Name, directly under Workspace.Visuals**. The existing exact Zvanishdummy Model-under-Thrown exclusion remains. Other-player names, stale character names, same-name objects elsewhere, model wrappers, generic Thrown.Part, real rigs and map geometry remain blockers. A wall behind an excluded object still blocks, and the existing five-ray bound remains.

This is a scoped trial based on the repeated ray-hit paths in SP-T018–SP-T020. Actual live properties/gameplay purpose are not proven solely by the name. New **LOS_OBJECT** events capture the hit object's class, CanCollide, CanQuery, Anchored, Size, matched exclusion and excluded root, once per observed object with a 32-object session cap. Cue details retain `ignoredCosmetics`, including when a wall behind an effect blocks the ray. These observations allow the next capture to verify the exact live object instead of expanding exclusions by guesswork.

### Validation and handoff

**155 mocked assertions passed:** 47 logger/lifecycle regressions, 42 active trial/catalog/LOS/deferral/opponent-counter checks, 26 player/NPC checks, 6 settings/reload checks, and 34 new asynchronous correlation/early-release/interleaving/scoped-LOS checks. New cases include delayed requests after API release, early release followed by a separate F cycle, duplicate OUT/RETURN, intervening down without a new request, repeated true, stale/orphan receipts, overlapping release windows, new-session reset, exact head-part matching, walls behind it, wrong names/parents/classes, changed character names and bounded LOS observations. Updated source and both staged public/private variants compile. These are simulated checks, **not live validation of v5**.

Reload the installed hub to use v5, then enable Auto Parry (still off on reload). Preserve offset=0 / hold=120 for the first comparison; diagnostics continue automatically. Inspect new DEFEND_REQUEST/INPUT correlation and LOS_OBJECT plus ignoredCosmetics, then actual damage/counter observations. Do not adjust multiple timing variables or claim reliable defense merely because the obstruction no longer blocks. Source seeds, range/cooldown gates, hold settings, target filters and existing stop/release behavior are retained. The user controls live testing.


## SP-015 / SP-T022 — v5 live correlation and head-effect validation (2026-11-17)

**Evidence:** finalized diagnostics-v5 session starting Unix 1794950578, main-world place `110405289082842`, manually disabled at 71.881 s. Unchanged range 8, PvP/PvE enabled, offset 0 ms, hold 120 ms. Totals: 25 automatic attempts, 23 self-counter messages, no other-counter or possible-parried messages, 25 negative-health observations. All attempted targets were the nearby Thug5 NPC binding. Report ends held=false with zero dropped lines and no input/release/logger/error/frame-gap failures. No runtime changes during this review.

### Fixes verified in this capture

**Asynchronous request correlation:** all 25 automatic presses and releases have successful API calls and corresponding observed Defend true/false requests. All 25 release records carry `pairValid=true`, the correct `cycleAttempt`, `releaseState=after_api_up_request` and no intervening down. There is one additional observed F cycle at 23.5996–24.0815 s, outside the automatic attempts. v5 correctly assigns it `attempt=none` / `cycleAttempt=none`; the self counter at 23.7975 s likewise has zero automatic candidates. The report contains 52 DEFEND_REQUEST events: 50 automatic-correlated plus the two unmatched requests. Early-release and overlapping-window branches did not occur here and remain mock-validated only.

**Local head-effect exclusion:** eight encountered objects match the exact local head-effect path; all are `Part`, CanCollide=false, CanQuery=true, Anchored=false, Size=(2,1,1). The ray can hit these non-collidable but queryable objects. All eight affected cues passed the narrow exclusion at observation and later generated automatic attempts (10, 11, 14, 15, 18, 19, 20, 21). Seven still required the exclusion at execution; attempt 14 had clear geometry by then. All eight were followed by a self-counter message. There were **zero line-of-sight skips** in this session. This verifies the intended live exclusion, not the broader claim that every Visuals object is safe to ignore; wall-behind-effect behavior remains mock-tested.

### Outcomes by observed combo

| Combo | Automatic attempts | Self-counter messages with that sole recent candidate |
| --- | --- | --- |
| 1 / `114493250337466` | 19 | 18 |
| 2 / `84394230178792` | 3 | 3 |
| 3 / `95937874840716` | 2 | 0 |
| 4 / `139487903747364` | 0 | Not tested |
| 5 / `131649056441662` | 1 | 1 |

Thus **22 distinct automatic attempts** had a self-counter message with one recent candidate and `inputConflicts=[]`; the 23rd message belongs to the unmatched F cycle. The 22 automatic-associated receipts follow their candidate presses by approximately 132–201 ms. These are strong repeated client-side correlations, not a general 88% confirmed success rate or server-proven attacker identity. All were NPC trials; PvP performance is not established.

Only two automatic attempts precede damage within 0.5 s, both combo 3: attempt 23 at 58.1138 s precedes damage by 115.2 ms, and attempt 25 at 67.7136 s by 114.5 ms. Both tracks were speed=1.15, phase=0.0192 s at execution, distance about 4.668 and 3.833 studs, with facing/LOS passing. Damage arrived while the API key was still held, and no self counter followed. This repeats the weak combo-3 result from the isolated v4 baseline; it does not identify whether its seed is early/late or the server's acceptance conditions. Attempt 13 (combo 1) also had no self-counter message, but its next damage was 1.614 s later, so absence alone is not a proven failed parry. Total damage includes hits between attempts and gated exchanges.

### Conditions changed despite unchanged trial settings

F_DOWN-log to Defend-true receipt shrank to **14.5–18.9 ms** (median 16.2), versus 42.3–60.5 ms in SP-T020. Actual API holds were 132.0–135.9 ms; adjacent true/false request spans were 115.4–120.5 ms (median 118.0). Release request receipt lag was 0.6–3.5 ms. Displayed ping samples were approximately 99.9–113.6 ms, lower than the preceding baseline's cited 144–153 ms samples. The changed input/request scheduling and network observations are confounders: the correlation code itself does not shorten the injected hold or change attack seeds. Do not attribute the entire outcome improvement to the head-effect fix or assume these conditions persist.

Other rejections: observed Counter cooldown 8, out of range 7, self-facing 7, stopped track 1, unmapped animation 131. No DEFER event, other-counter pairing, or early observed release was exercised.

**Current handoff:** v5 ordinary delayed-release correlation, unmatched-cycle separation and the scoped head-effect exclusion are verified live in this capture. Retain the current timings as the successful comparison baseline for combos 1/2/5. The next timing investigation should isolate combo 3, preserving the other seeds rather than moving the global offset based on this one run. Combo 4 and broader player/weapon coverage remain unvalidated. No additional generic log is needed to establish the two fixes; do not silently promote all automation to reliable based on this favorable session.
