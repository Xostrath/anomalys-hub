# Sparkling — findings and handoff

Last reviewed: 2026-11-19. This is the initial consolidated handoff from the available source, three reviewed combat captures, the first automatic-defense log, and user feedback. Earlier unrecorded work may be missing. Add evidence rather than reconstructing missing history from guesses.

## Read first

**Current mode: ALL-HIT AUTO PARRY (Combat page v14 / diagnostics v7; bounded PvP range recheck and baseline timing), including the first hit; temporary test restrictions removed at user request.** The user explicitly requested active auto parry with recording and opponent-parry handling. This supersedes the temporary passive-only instruction; see SP-T021 for the latest correlation/LOS changes. Old mode instructions remain historical.

- Universe: `10771972206`. Tutorial: `101713836627444`. Observed main world: `110405289082842`. Additional recorded place: `135537125271116` (place name/role not verified). User-supplied entry place: `120525873606470`.
- Current objective: validate bounded recovery of fresh player swings that enter the configured range after observation. SP-T036 audits the existing range rejections; old logs cannot reconstruct their movement. Preserve NPC behavior and baseline timing.
- Current defense: off-by-default all-five-combo auto parry with a short PvP approach recheck; original seeds/hold settings and normal gates remain. See SP-T036.
- Latest implementation: **Combat page v14 / diagnostics v7**. Up to 80 ms of range rechecks for fresh player cues within Range+3, while actual input still requires Range. 231 mocked assertions and source/both-build compilation passed; live benefit unverified. Event-state capture was validated in SP-T035.
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
| Auto parry | All five hits; baseline timings; bounded player approach queue | v14 / SP-T036 implemented and mocked; PvP benefit unverified | Keep prior NPC counter evidence separate; server eligibility remains unknown |
| NPC ESP (SP-V001…V005) | ESP tab "NPC ESP" card (`src/SparklingNpcESP.lua`): every Humanoid rig in Workspace that isn't a player character. Shows name/type, HP current/max, direct `PowerLevel` value if the model has one, distance, box. Types (v3, SP-V003): **ENEMY** = the model has a `PowerLevel` (child NumberValue/IntValue or attribute), **NPC** = no power level; each type has its own toggle. UI v3: Player ESP and NPC ESP are `ui.Section`s with checkmarks (`ui.CheckGrid`) and a 100–10,000 stud distance slider (`ui.Slider`) at the bottom | Implemented / untested live; both builds compile | Does the health rule hold for every NPC? Where do NPCs live, and do they carry PowerLevel? |
| Combat page v11 (SP-T030) | Normal Auto Parry controls; no temporary single-hit filters | 174 mocked assertions and compilation passed; all-hit coverage/cleanup live-verified in SP-T031 | Preserve target context and distinguish conflicted counter receipts from automatic results |
| Parry diagnostics v7 | Adds RANGE_WAIT/CHECK/ENTERED to existing request/state/outcome records | v6 state snapshots verified live in SP-T035; new geometry trace mocked in SP-T036 | Range entry is not a successful parry; live recovery outcomes outstanding |

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
| SP-V001 (was SP-T010) / 2026-11-17 | NPC ESP v1 (`src/SparklingNpcESP.lua`, Claude chat) | User asked for NPC ESP. Reused [Beyond Nen](beyond-nen.md) `src/BeyondNenNpcESP.lua` structure: one Workspace scan, DescendantAdded/Removing, per-model billboards, shared `NpcEspEnabled`/`NpcEspDistance`/`NpcEspSettings` | Beyond Nen's Trello catalog was **not** transferred. Classification uses only the observed `...Trainer` names; no enemy/boss/quest roles are claimed. New shared setting keys: `Health`, `Power`, `Trainer`, `Npc`. Power uses the same direct-value rule as player ESP | **Implemented / untested live** | Turn on NPC ESP in the main world. Confirm labels on WolfFang/Tienshin trainers, look for NPCs outside Workspace.Live, and note whether any NPC shows POWER. The Sparkling Trello (board `ruce1V2a`) could supply an NPC catalog later |

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

- 2026-11-17 — **ID note:** ESP/Visuals entries use their own **SP-V###** series because two chats were numbering SP-T### in parallel and collided (SP-T011–T014). The renumbered entries are V001 (was T010), V002 (T011), V003 (T012), V004 (T013) and V005 (T014). Commit messages 9189a4c, de2c942, 115adaf and 0cd2f94 still cite the old numbers. GPT's SP-T011–T014 are unrelated parry entries.
- 2026-11-17 — Auto Meditation v1 (SP-A001; autos use the **SP-A###** series). The Auto tab is back for Sparkling, with an "Auto Training" section (`src/SparklingAutoMeditation.lua`).
  - **Evidence (tutorial capture, 24–45 s):**
    - M sends `Action("Meditate")`, which opens `PlayerGui["Mind Training"].Frame.Start` (TextButton).
    - The text shows W/A/S/D about every 1 s and the user pressed the shown key after ~0.45–0.55 s. A repeated letter produced no text change; the user re-pressed W at ~1.05 s intervals.
    - The UI hid after ~20 s.
    - No per-key remote appeared, but only the combat remotes were watched.
  - **Guide:** M raises the Ki stat up to 400 (profile `Mind`, 100 for the user); J enters the Meditation World (Meditation Type only).
  - **Behaviour:**
    - Press M when the prompt is closed (1.5 s after a session ends, retry every 4 s).
    - Press each new letter after 0.15–0.35 s; re-press the same letter if it is still shown 1.05 s after the last press.
    - A hide under 0.8 s is not the session end.
    - Stops at Mind ≥ 400 (profile, re-read every 30 s), on damage, death, chat/menu or teleport.
    - Logs PROMPT/PRESS/REPEAT/CHANGE/SESSION to `sparkling_auto_meditation_latest.txt`. CHANGE records other prompt properties, to find a cleaner new-prompt signal.
  - **Reference:** Hunter X Enter training key macros; Beyond Nen Auto Meditation (which used server prompt events, not available here).
  - Implemented / untested live.
- 2026-11-17 — CEXP Finder v1 (SP-V011). The user's main reason for the Visuals tab is CEXP, which the client profile doesn't contain.
  - New **Recorders → Sparkling CEXP Finder**: start, talk to Varrik (and the Title Master), stop; auto-stops after 3 min. Output: `sparkling_cexp_capture_latest.txt`.
  - It logs every ReplicatedStorage RemoteEvent's incoming payloads, outgoing FireServer/InvokeServer calls **with their replies**, new or changed numeric text in PlayerGui and Workspace (NPC billboards), and TextChatService messages. It sends nothing.
  - For outgoing calls it reuses the combat recorder's shared hooks through a new generic `controller.BeginSparklingRemoteObservation(targets, callback)` in `src/SparklingCombatRecorder.lua`. That entry point refuses if the manual recorder or another capture (Auto Parry diagnostics) owns the hooks; turn Auto Parry off first.
  - Implemented / untested live.
- 2026-11-17 — Visuals v4 (SP-V010). **Hidden Value Scan v3 reviewed: the client profile table, fully dumped** (16 snapshot copies; 13 identical, 3 older ones with lower PowerLevel and Emotion).
  - **Keys:** Ability, Age, Agility, AuraColor, AutoFire, CharacterName, CombatStyle, Concentration, CoreStr, CounterOnR, Emotion (number), Emp, EventWins, EyeColor, Faction, Fatigue, Gender, GravityMastery, HairColor, Height, Hunger, Jared, Karma, Ki, KiAwakened, KiStyle, Limit, LimitBreak, Location, MenuSettings, Mind (Ki stat), MuscleDense, NewLocation, Personality, Pose, PowerLevel, Quests (JSON), Race, Reactions, Relations (JSON map player→score), ShiftRun, SkillMastery (JSON list), SlotData (JSON), StatCap, StatPoints, Strength, TalentCards (JSON), TalentDraw, Title, Trait, Weapon, Width, Zeni.
  - **No CEXP key.** The "CEXP" container is a reward-label table ({"5,000","COMBAT EXP"} etc.), not player data. Per the guide, only the NPC Varrik shows CEXP, so it is treated as server-only.
  - **The profile is your own only;** other players' profiles are not in client memory.
  - Visuals now reads stats (STR/BODY/CONC/REACT/AGI/KI, points, cap), personality, the emotion number, form mastery (SkillMastery list + GravityMastery) and relationships from the profile.
    - The profile is refreshed every 15 s while the tab is open (getgc, next-only reads); the snapshot whose PowerLevel is closest to the live `Character.PowerLevel` is used.
    - For another player, Relationships shows your score with them from your `Relations`, plus their character's `[Rank]` tag (hypothesis).
    - Latent Power stays `RacePotential` (?); the profile has no latent-power key.
  - Implemented / untested live.
- 2026-11-17 — Visuals v3 / Hidden Value Scan v3 (SP-V009).
  - **User-confirmed:** Race Variant (`Character@Trait`) is correct.
  - **Inconclusive:** Limit Break. The user's BP is ~15k, below the first limit, so 0 is expected either way.
  - **Trello (board `ruce1V2a`, Mechanics list):**
    - Limit breaks happen at 100k / 250k / 450k / 700k / 1M BP; a grey bar fills by fighting.
    - CEXP comes from queues, fights and quests (one talent-card hand per 10k).
    - Latent Power sets BP gain (raised by a Shenron wish).
    - Relationship ranks: Good Friend, Friends, Familiar, Stranger, Adversary, Enemy, Nemesis.
    - Emotion and Karma have numeric scales shown as images.
    - Stats: Strength, Body Strength, Concentration, Reactions, Agility, Ki.
    - The in-game guide (`NEWUI.KiMenu.GuideTopics.*@Words`) mirrors these cards.
  - **Relationship lead (hypothesis):** the user's character has a child named `"[Stranger]\nAlmo"` (rank + newline + character name), apparently the overhead tag. Visuals now reads the bracketed rank from the target character and marks it (?).
  - **Remote names found** (not called): `Requests.DataUpdated`, `Requests.FetchData` (RemoteFunction), `Requests.Relations`, `Requests.TitleData`, `Requests.SetCharacterInfo`, `Requests.AddStat`, `Requests.UpdateSlotData`.
  - **Scan v2 failure:** v2 dumped nothing (`PROFILE_TABLES 0`) although its first pass matched the same 16 profile copies. Its second pass used `rawget`/exact key lookups that missed those tables (cause unknown). v3 dumps the first pass's own matched objects via `next` and logs the raw key types.
- 2026-11-17 — Hidden Value Scan v1 reviewed (SP-V008). 56,018 client tables checked, 18 distinct matches:
  - **Client profile data table found:** keys include `CharacterName`, `Emotion` (number: −57.29 for the user, whose menu shows "Sad"), `Trait`, `Agility`, `Reactions`, `CoreStr`, `Limit`, `EventWins`, `TalentDraw`, `SkillMastery` (JSON string, "[]"), `SlotData` (JSON), `KiAwakened`, `MuscleDense`, `HairColor`, `Width`, `CounterOnR`. 16 copies exist (staleness unknown).
  - A label→key map: Ki Style→`KiStyle`, Zeni, Age, Combat Style→`CombatStyle`, Title, Name→`CharacterName`, Faction, Emotion, Karma, Personality, **Sub Type→`Trait`**. This supports Race Variant = `Trait`.
  - A script environment exposes `getEmotion` and `getKarma`.
  - A container with `CEXP`/`PowerLevel`/`Zeni`/`DragonBall`/`Namekian`/`EventWins`/`Spins`/`Capsule` sub-tables (purpose unknown, possibly leaderboards), plus a sibling table of formatter functions with the same keys.
  - Also seen: `Unbound_Potential` (a talent list) and `Korin's_Potential_Unlock` (quest list).
  - Scan v2 dumps the full profile table (and how its copies differ) and the CEXP container to depth 2.
- 2026-11-17 — Visuals v2 (SP-V007). **Profile Scan reviewed** (main world; the user's own character plus 3 other players, keyword-only for others). The user notes some values are hidden in-game.
  - **Replicated character data, readable for every player:**
    - `@Karma`: a number (user −35; others 0/−5/−15); the menu shows only a label.
    - `@Trait`: SaiyanHybrid / TriHuman / Traditional. Shown as Race Variant, a **hypothesis**.
    - `LimitBreak` (NumberValue) and `@Limit` (0/1). Shown as Limit Break Progress, a **hypothesis**.
    - `RacePotential` (1, 0.85). Shown as Latent Power, a **hypothesis**.
    - `@Form` (current form).
    - Also seen: `@EventWins`, `@TalentDraw`, `@Title`, `@Weaves`, `@SparPartner`, `@Prey`, `Focus`, `Posture`, `Wanted`, `GravityAugment`, `CurrentArea`, `TempStrength`.
  - **The user's own UI only:**
    - `PlayerGui.Inventory.PlayerStat.CharacterInfoHolder.CharacterInfo.infos`: "Personality: …", "Combat Style: …", "Karma: <label>", "Emotion: <label>" (rich text).
    - `…PlayerStat.Stats.StatHolder.<Stat>.StatName/Amount`: Strength, Agility, CoreStr ("Body Strength"), Concentration, Reactions; `Stats.Points` "Stat Points: N".
    - Hidden HUD counters `HUD.PlayerBody.KarmaCounter.Number` / `EmotionCounter.Number` ("[9%]", "[6%]", `visible=false`).
    - Other players' personality, emotion and stats were not found replicated.
  - **Not found:** CEXP, form mastery, relationships. ReplicatedStorage has a guide topic `NEWUI.KiMenu.GuideTopics["Mechanics - Battle Power & CombatEXP"]`, and `ReplicatedStorage.Trainers.<name>.Stats.Strength` / `PowerLevel` (trainer reference data).
  - **Implemented / untested live:** the Visuals rows read the sources above; hypotheses show "(?)" and own-only fields show "Hidden (only shown for you)" for other players. A **Hidden Value Scan** button searches client memory tables (`getgc`, raw `next`), dumps the guide text for these mechanics and lists data-like remote names (never called). Output: `sparkling_hidden_scan_latest.txt`.
- 2026-11-17 — Visuals tab v1 (SP-V006): new **Visuals** tab (`src/SparklingVisuals.lua`), plus **Recorders → Sparkling Profile Scan**.
  - Player dropdown at the top, defaulting to you and listing every player.
  - Profile rows: CEXP, Battle Power, Stats, Personality, Race, Race Variant, Form Mastery, Limit Break Progress, Karma, Latent Power, Emotion, Relationships.
  - Only confirmed sources are read: Battle Power = `Character.PowerLevel`, Race = `Character@Race`. Every other row shows "Not found yet". None of those names appeared in any capture, including the ESP Data scan of Player/Character to depth 4, so the data is elsewhere (ReplicatedStorage, deeper folders, or only the game's own menu). If it's menu-only, other players' values may not be readable.
  - The Profile Scan dumps the user's own Player/Character in full, keyword matches for other players and ReplicatedStorage, and keyword-matching text in the user's PlayerGui (run it with the stats menu open). Output: `sparkling_profile_scan_latest.txt`. Implemented / untested live.
- 2026-11-17 — ESP v5 (SP-V005, was SP-T014). **ESP Data capture reviewed** (main world, 12 players, the user's own Ki at full):
  - **Ki maximum found:** it is an attribute on the Ki value itself, `Character.Ki@Max`. All 12 players have both `Ki` (NumberValue) and `Ki@Max`. Most read full; one read 200.7 / 441.6, which confirms a live current/max pair.
  - The Ki bar now fills from `Ki.Value / Ki@Max` (the old `MaxKi` sources are still accepted). This fixes SP-V004's empty bar; untested live.
  - The same `@Max` pattern exists for `Character.Stamina@Max`, `OverCharge@Max` (100) and `Shield@Max`, plus `Output@MaxOutput`/`Output@Overcharge`, `Charges`, `RacePotential` and `Humanoid@BaseMaxHealth`. These are candidates for future bars.
  - **Race verified:** the explicit `Character@Race` string attribute resolved for all 12 players (Human/Saiyan/Namekian).
  - `Character.PowerLevel` was present for all 12.
  - `KiHud.Stage.bars` holds ImageLabels `Health`, `Stamina`, `Ki`, `Food`, `Readout`, `Orbs` (fill method not needed now).
- 2026-11-17 — ESP v4 (SP-V004, was SP-T013):
  - Enemy NPCs get a health bar (user request) in the player-ESP style: a 7px dark track right of the box, red fill from the bottom, Humanoid Health/MaxHealth, shown when the Health checkmark is on. Regular NPCs keep the HP text only.
  - **Ki bar failure (user report): "the ki bar does not display anything."** Cause from source: the bar fills only with an explicit `MaxKi` attribute/value, and none exists in any capture. Only `Character.Ki` (153, 163.5) was ever observed; `PlayerGui.KiHud` captures listed text labels only (slots, limbs/Fatigue, BossStage), never a bar fill. No maximum was invented.
  - **Capture ESP Data** now matches any name containing ki/energy/charge/stamina/max and dumps every `PlayerGui.KiHud` GuiObject with its Size scale and text, to find the Ki maximum or the HUD's own fill fraction.
  - Next: the user runs it at full Ki and again after spending Ki.
- 2026-11-17 — ESP v3 (SP-V003, was SP-T012):
  - **User-supplied rule:** enemy NPCs are identified by having a power level. It replaces the v2 max-health split (superseded; the user's note that regular NPCs have 100 or 999 max health is kept as context).
  - The user asked for checkmarks and a distance slider at the bottom of each ESP section, so Sparkling's Player ESP (`src/SparklingESP.lua`, which hides the shared button card) and NPC ESP are now sections.
  - New shared helpers `ui.CheckGrid` and `ui.Slider` in `src/UniversalHub.lua`; other games are unchanged.
- 2026-11-17 — NPC ESP v2 (SP-V002, was SP-T011): regular vs enemy split by MaxHealth. **User-supplied rule:** regular NPCs have 100 or 999 max health, enemies don't. Replaces v1's name-based TRAINER label (superseded). Shared setting key `Trainer` removed; `Enemy` (already a shared key) toggles enemies, `Npc` toggles regular NPCs.
- 2026-11-17 — NPC ESP v1 (SP-V001, was SP-T010): new `src/SparklingNpcESP.lua` card on the ESP tab, wired in `tools/build_public.ps1` after `SparklingESP.lua`.

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


## SP-016 / SP-T023 — first-hit disabled for later-hit trials (2026-11-17)

**User instruction:** review the new recording and remove first-hit parry to test the others. Current **Combat page v8 / diagnostics v5** is explicitly **Auto Parry // HITS 2-5 TEST**. This supersedes the earlier instruction to retain all five active seeds for the next comparison.

### Latest pre-change recording

Reviewed finalized diagnostics-v5 session starting Unix 1794952495, main-world place `110405289082842`. It stopped on focus loss at 80.948 s, held=false, no dropped lines or input/release/logger/error/frame-gap failures. Baseline remained range 8, offset 0 ms, hold 120 ms, PvP/PvE enabled. Totals: 29 automatic attempts, 19 self-counter messages, 8 other-counter messages, 3 possible-parried correlations and 30 negative-health observations. All 29 API releases succeeded with observed corresponding Defend false requests.

| Combo | Attempts | Sole recent automatic candidate for self counter | Damage within 0.5 s after attempt |
| --- | --- | --- | --- |
| 1 | 12 | 11 | 1 |
| 2 | 6 | 4 | 1 |
| 3 | 5 | 2 | 3 |
| 4 | 4 | 0 | 4 |
| 5 | 2 | 0 | 0 |

The 17 sole-candidate messages had no recorded input conflicts. Two additional self-counter messages were ambiguous: at 51.1438 s, attempts 21/22 were recent with an intervening input flagged for 21; at 61.4087 s, attempts 25/26 were recent with an intervening input flagged for 25. Do not credit either ambiguous message to a particular combo or turn this table into confirmed success rates. No damage after combo 5 does not itself establish a successful parry. The three possible-parried events remain heuristic. A short-cooldown defer occurred at 60.2114 s with 46.8 ms remaining; this review does not claim its eventual outcome as a distinct success.

Input-to-request delay remained low: 12.9–19.6 ms, median 16.4; actual holds 120.4–135.9 ms; adjacent Defend request spans 105.2–120.8 ms. No LOS rejection appeared. The new evidence reinforces repeated first-hit counter correlations while exposing later-hit variability, motivating the user's requested experiment rather than global timing changes.

### Implementation and validation

Animation **114493250337466 (combo 1)** is removed from the active timing catalog. Its observed animation start/stop remains recorded; the observer explicitly logs **SKIP reason=first_hit_disabled stage=observe** before scheduling. It never queues a defense attempt, consumes a pulse, or generates F input on its own. This applies to both NPC and player ownership because both use the same observer. It is deliberately disabled, not mislabeled as an unknown animation.

Hits 2–5 keep their existing seeds: combo 2=0.04 s, 3=0 s, 4=0 s, 5=0.04 s. Range, facing/LOS, cooldowns, input cleanup, offset/hold controls, target filters and continuous diagnostics are retained. Header Config now states `trialSeeds=combo1:disabled,...`; the Combat section and initial status explicitly identify the experiment. There is no new persisted toggle; restore hit 1 only on a later request. Diagnostics remain schema v5; implementation is Combat page v8.

**157 mocked assertions passed:** 47 logger/lifecycle, 44 active-feature/catalog checks (including first-hit animation logged/skipped with no generated input and all four remaining hits still active), 26 PvP/PvE, 6 settings/reload, 34 correlation/LOS. Source and both staged builds compile. This change is not live-validated yet. Source and exact module replacements preserve the concurrent Visuals tab / Profile Scan change from commit 2bac7ed and the latest installed private contents.

**Next check:** reload the installed hub, enable the HITS 2-5 TEST, and retain offset=0 / hold=120. Verify first-hit skip events and no combo-1 F_DOWN; then inspect subsequent combo attempts, request/counter receipts and cooldowns. First-hit damage is expected to become visible when its automatic defense is intentionally disabled; distinguish it from a failed later-hit attempt. Actual cooldown or other gates may still prevent a later hit from being attempted. Keep the user's live session under their control and record later findings here.


## SP-017 / SP-T024 — later-hit trial verified live (2026-11-17)

Reviewed finalized session starting Unix 1794952746, main-world place 110405289082842, Combat page v8 / diagnostics v5. Manually disabled at 72.016 s. Config confirms combo1:disabled, offset=0 ms, hold=120 ms, range=8 and both target filters enabled. All actual attempts target the Thug5 NPC binding. No runtime code/settings changed during this review.

**First-hit exclusion verified live:** 17 first-hit animation cues logged first_hit_disabled; zero first-hit F_DOWN attempts. Later hits still generated 11 attempts. All 11 successful API releases have corresponding observed Defend=false with correct cycleAttempt and no intervening down. No error, input/release failure, dropped line or frame-gap events; final held=false. One additional F cycle at 38.1999–38.6155 s is correctly unmatched, with no assigned automatic attempt.

| Combo | Attempts | Sole-candidate self counters | Negative health within 0.5 s |
| --- | --- | --- | --- |
| 1 | 0 (17 deliberately skipped cues) | Not applicable | Not an automatic-defense trial |
| 2 | 6 | 5 | 0 |
| 3 | 4 | 1 | 2 |
| 4 | 1 | 0 | 1 |
| 5 | 0 | Not tested | Not tested |

Seven total self-counter messages: six have a sole recent automatic candidate with no input conflict, while the message at 12.6651 s has two candidates, attempts 2 (combo 2, 649 ms earlier) and 3 (combo 3, 148 ms earlier). It flags inputConflicts=[2] and remains ambiguous. Do not allocate it to either combo or derive a confirmed success rate from the table. Combo 3's sole-candidate counter at 7.9995 s follows attempt 1 by 133 ms. Its other no-counter attempts are mixed: attempt 8 at 33.1032 s and attempt 11 at 52.6479 s precede damage by 477.5 and 130.8 ms respectively.

The only combo-4 attempt, 9 at 33.4969 s, precedes damage by 83.8 ms while its key is still held. This is the **same negative-health event** at 33.5807 s counted after combo-3 attempt 8; do not count it as two independently proven failed attacks. No self counter accompanies that exchange. Overall there are 27 negative-health observations, including exchanges where the first hit is intentionally not parried.

Combo 5 has no attempt: two cues were blocked by observed Counter cooldown (4.4183 s remaining at 34.1327 s, 1.1689 s at 41.3830 s); a third was blocked by real map geometry at 62.3629 s. The other LOS skip, combo 4 at 61.5981 s, also hit Workspace.Map.Model.MeshPart. Both observed parts are CanCollide=true, CanQuery=true, Anchored=true, approximately size (9.733,10.247,0.471). The narrow cosmetic exclusion correctly leaves these map blockers in place. No head-effect exclusion was exercised in this report.

Six other-counter messages produced five possible-parried correlations, still heuristic. No automatic key was held at those five timestamps. No DEFER event occurred. Other skip totals: cooldown 6, attacker facing 2, range 2, self facing 1, unmapped animation 355. Many idle/movement animation observations are unmapped; do not equate that total with 355 missed attacks.

Input-to-Defend delay remains 15.7–22.7 ms (median 16.9); API holds 131.5–140.4 ms; adjacent request spans 110.0–122.7 ms. Ordinary request correlation remains functional without adjusting timing.

**Handoff:** retain first-hit-disabled mode as requested. Hit 2 has repeated strong counter correlations, hit 3 remains mixed, hit 4 has another damage-associated trial, and hit 5 was not eligible here. Next investigation should isolate hits 3/4 and compare their animation phases/request timing, preserving hit 2's seed. To get hit-5 evidence, obtain an eligible exchange with cooldown clear and unobstructed geometry; do not bypass actual cooldown or wall checks. Current evidence does not determine an exact replacement timing. The mode itself is live-validated; broader parry reliability remains unproven.


## SP-018 / SP-T025 — isolated hit-3 / hit-4 selector (2026-11-17)

**Authorization and purpose:** user said to continue after the later-hit review and focus on hits 3/4 while preserving hit 2's timing. Current **Combat page v9 / diagnostics v5** provides **Auto Parry // SINGLE HIT TEST** with a **Test hit: 3 (click to switch)** button. It alternates between hit 3 and hit 4; default is hit 3. This replaces simultaneous hits 2-5 for the next diagnostic trial. The selected hit persists; active auto parry still starts OFF on reload.

SP-T023–SP-T024 show mixed hit-3 results and damage associated with hit 4, but do not establish a defensible replacement delay. In SP-T024, one damage event followed both a hit-3 and hit-4 attempt; one self counter likewise had hit-2/hit-3 candidates. Isolating the selected animation prevents automatic attempts for other combos from consuming the same input/cooldown window or creating these automatic-attempt overlaps. It does not eliminate other actors, manual input, or game-origin cooldowns.

### Behavior

- Hit 3 targets animation 95937874840716; hit 4 targets 139487903747364. Only the selected ID can queue an automatic defense. Both retain seed delay 0 s; timing offset and hold controls remain available and unchanged.
- First-hit animation stays recorded with first_hit_disabled. Other known nonselected hits log trial_hit_filtered with testHit; unknown animations retain unmapped_animation. Hit 2's 0.04 s seed and hit 5's 0.04 s seed remain in source but are filtered out of this isolated trial. They cannot generate F input in this mode. Ownership and existing PvP/PvE filters still apply to the selected hit.
- Switching the selector stops/saves an active session, clears watched tracks and pending cues, releases held F, disconnects the observation hook, then changes the saved selection. It never automatically enables the next run. If key release fails, the selection remains unchanged until release succeeds.
- Each session snapshots TestHit. Header Config, START and CUE_ACCEPTED identify the selection; copying a stopped report after switching still reports the original hit. Live status identifies HIT 3 TEST or HIT 4 TEST. Old settings without TestHit default to 3; only 4 selects the alternative, with invalid values falling back to 3.
- The shared section expands vertically to accommodate the selector above the existing timing boxes. Recorder controls, theme, LOS, cooldown gates and cleanup remain unchanged. No live game session is controlled during deployment.

### Validation and next check

**187 mocked assertions passed:** 47 logger/lifecycle, 41 active feature/deferral/counter checks, 26 PvP/PvE, 7 settings/reload, 1 invalid-hit fallback, 34 asynchronous correlation/LOS, and 31 selector-specific cases. Selector cases cover each known combo under both selections, first-hit logging without input, selected-hit persistence, session headers after switching, key-up on switch, stopped hooks, disabled next run, queued-cue removal, release failure/recovery and player-owned targets. Updated source and staged public/private builds compile. The selector and resulting single-hit trials are **not yet live-validated**.

Next: reload, leave **Test hit: 3**, offset=0 and hold=120, then enable and finish a short exchange. For hit 4, click the selector and enable again; switching finalizes the earlier run. Inspect the histories separately. Required acceptance: no F_DOWN animation outside the selected hit, consistent testHit header, expected filtered skips, paired release requests and cleanup. Then compare selected-hit phase/request/counter/damage observations before proposing a delay adjustment. Nonselected-hit damage is expected and must not be automatically attributed to the selected attempt. Keep the previously favorable hit-2 seed intact; do not tune a global delay from mixed-combo totals.


## SP-019 / SP-T026 — isolated hit-3 test against Shadow (2026-11-17)

**Evidence:** finalized diagnostics-v5 session starting Unix 1794954744, main-world place 110405289082842, Combat page v9. Header testHit=3, offset=0 ms, hold=120 ms, range=8, both target filters enabled. Manually disabled at 33.467 s; held=false, zero dropped lines, no logger/input/release/error/frame-gap failures. Totals: 3 automatic attempts, 1 self-counter message, 0 negative-health observations, no other-counter or possible-parried events. Runtime code and settings unchanged during this review.

**Important context change:** all three attempts targeted the NPC binding named **Shadow**, not the Thug5 binding from prior tests. The capture does not establish this target's damage rules or confirm that every observed animation was an attack that would have damaged the local character. Do not compare zero observed damage directly with the earlier Thug5 failure counts or count all three as confirmed successful parries.

**Selected-hit filtering verified live:** all F_DOWN events use hit-3 animation 95937874840716. Five first-hit cues log first_hit_disabled; 13 other known-hit cues log trial_hit_filtered. There are zero automatic inputs for any nonselected hit. Four selected cues queued: three executed and one was rejected for Counter cooldown. Another selected cue failed self-facing. Ordinary input correlation and release cleanup worked for all three executed attempts.

| Attempt | F press time | Distance | Track phase / speed | Subsequent self-counter receipt |
| --- | --- | --- | --- | --- |
| 1 | 4.5005 s | 4.485 studs | 0.0192 s / 1.15 | None |
| 2 | 23.7989 s | 4.968 studs | 0.0192 s / 1.15 | 23.9597 s, approximately 161 ms after press |
| 3 | 26.7390 s | 3.812 studs | 0.0192 s / 1.15 | None |

Attempt 2 is the only recent automatic candidate for that counter and has inputConflicts=[]. Its Defend=true receipt is at 23.8179 s, false at 23.9337 s, then the self counter at 23.9597 s. This is a useful isolated temporal correlation, not a server-confirmed success window. Attempts 1/3 have neither negative health nor a counter; their result remains inconclusive.

All three release records have pairValid=true, correct cycleAttempt, after_api_up_request and no intervening down. Input-to-request delay is 19.0–23.6 ms; API holds 134.0–152.3 ms; adjacent true/false request spans 115.8–133.8 ms. Three additional F cycles at 28.0395–28.3880, 30.8840–31.0768 and 31.1147–31.2698 s correctly remain unmatched. They occur after the three automatic attempts and do not create additional automatic successes. Their source is unknown. At 31.2896 s, a later hit-3 cue was skipped with 0.9033 s observed Counter cooldown remaining; do not bypass that gate.

**Handoff:** hit-3-only filtering, report selection, ordinary request correlation and cleanup are now live-validated. The hit-4 selection and switch-between-runs behavior still need live validation; their mocked tests remain separate evidence. Next select Test hit: 4, then enable a separate run at offset=0 / hold=120. Record the target context: Shadow and Thug5 cannot be silently pooled as equivalent damage trials. The current small hit-3 sample does not establish a replacement delay, so preserve the seeds until a discriminating comparison is available.


## SP-020 / SP-T027 — short hit-3 follow-up against Thug5 (2026-11-17)

Reviewed finalized session starting Unix 1794954940, main-world place 110405289082842, 14.317 s, manually disabled. **Header still reads testHit=3, not 4.** Offset=0 ms, hold=120 ms, range=8 and both target filters remain unchanged. This time the attempted target is Thug5; the preceding SP-T026 target was Shadow. No code/settings changed during review.

One automatic hit-3 attempt occurred at 7.0358 s: animation 95937874840716, phase=0.0240 s, speed=1.15, distance=3.793 studs, facing/LOS passing. Defend=true was observed at 7.0627 s, negative health (-1.5962) at 7.1410 s, then Defend=false at 7.1725 s. Damage followed the press by 105.2 ms while F was still held; there was no self-counter message. The release API succeeded, request correlation identified cycleAttempt=1, and final held=false. This is another damage-associated hit-3 attempt under the unchanged baseline, not enough to identify an exact timing correction or prove the damage source.

A second selected cue at 11.4580 s was skipped with 1.0497 s Counter cooldown remaining. Other gates recorded 3 first_hit_disabled and 7 trial_hit_filtered skips; there were no nonselected automatic inputs. The session contains 13 negative-health events overall, including hits outside the sole attempt. No dropped lines, input/release/logger/errors or frame-gap events. No extra F cycle appears in this capture.

**Handoff:** hit-3 filtering and request cleanup remain verified, and the Thug5 result differs from the preceding zero-damage Shadow session. The isolated hit-4 test remains outstanding; do not label this recording as hit-4 evidence. Use the Combat selector to change its visible label from Test hit: 3 to Test hit: 4, then enable a new run at offset=0 / hold=120. A successful switch should produce testHit=4 in the next report and only animation 139487903747364 in its F_DOWN events.


## SP-021 / SP-T028 — repeated hit-3 test and clearer hit buttons (2026-11-17)

Latest finalized session starts Unix 1794955088, main-world place 110405289082842, diagnostics v5, 19.846 s, manually disabled. Header still testHit=3, offset=0 / hold=120. All three automatic attempts target Thug5 using hit-3 animation 95937874840716. Press times 0.5436, 11.8570 and 17.5915 s precede negative-health observations by 95.0, 105.0 and 126.3 ms respectively; no self-counter message occurred. All three releases succeed and correlate correctly. Total negative-health observations=23; six other-counter messages are not local successes. Final held=false, no dropped lines or input/logger/error/frame-gap failures. First-hit-disabled skips=4, other-known-hit filtered skips=11. This is further hit-3 baseline evidence, **not hit-4 evidence**.

The user clarified that they could not find the hit selector. Installed/source inspection found the v9 toggle below target counts, connected through the normal shared button helper; the saved settings had no TestHit entry. This does not prove a click-handler failure. The concrete usability issue is the user's inability to locate the control; the layout was changed accordingly.

**Combat page v10 / diagnostics v5** replaces the buried click-to-toggle control with two explicit top-row buttons, **Hit 3** and **Hit 4**, immediately above Enable Auto Parry. The selected button says **Hit 3: SELECTED** or **Hit 4: SELECTED** and uses the shared accent color. Controls below it are shifted with non-overlapping fixed rows; section height expanded to 368 body pixels. Default remains hit 3 unless a valid saved hit-4 selection exists. Timing seeds, offset and hold settings are unchanged.

Selecting a different hit retains v9's stop/save, queue clear, key release and disabled-next-run behavior. Failed release prevents the change. Clicking the already-selected hit is a no-op and does not stop a running test. Both choices directly select their named hit; neither requires inferring the current toggle direction. Existing session snapshots retain their original testHit after a change.

**Validation:** 188 mocked assertions passed (47 logger/lifecycle, 41 active-feature, 26 ownership, 7 settings/reload, 1 invalid-setting fallback, 34 correlation/LOS, 32 selection/lifecycle checks). Updated source and both staged public/private variants compile. The new layout was source-inspected, not visually verified in the live game. Button behavior is mock-validated; live hit-4 testing remains outstanding.

**Next action:** reload the hub, open Combat, click **Hit 4** at the top of SINGLE HIT TEST and confirm it reads **Hit 4: SELECTED**, then enable Auto Parry. Preserve offset=0 / hold=120. The resulting report must say testHit=4 and only emit automatic input for animation 139487903747364. Do not keep treating unchanged hit-3 sessions as hit-4 timing trials.


## SP-022 / SP-T029 — hit switch verified; isolated hit-4 baseline (2026-11-17)

Reviewed two new finalized diagnostics-v5 histories in main-world place 110405289082842. Both use offset=0 ms, hold=120 ms, range=8 and both target filters. No runtime code or settings changed during this review.

| Session start | Selection / target | Duration / stop | Attempts | Self counters | Negative-health observations |
| --- | --- | --- | --- | --- | --- |
| 1794955705 | Hit 3 / Thug6 | 48.497 s / Test hit changed | 3 | 1 | 13 |
| 1794955781 | Hit 4 / Thug5 | 36.185 s / manually disabled | 4 | 0 | 39 |

**Selection/switch behavior verified live:** the earlier report finalizes with the hit-change stop reason and retains testHit=3. The later report says testHit=4 and all four F_DOWN entries use animation 139487903747364, with no other automatic attack IDs. Its 10 first-hit cues and 24 other known-hit cues are explicitly filtered. Both end held=false, with no dropped lines, logger/input/release/error/frame-gap failures. All seven API key-ups succeeded and have correctly paired Defend=false receipts, with no unmatched extra F cycles. Three additional hit-4 cues were blocked by observed Counter cooldown. No LOS rejection or defer occurred in the hit-4 run.

### Hit-4 baseline result

| Attempt | F press | Defend=true receipt | Negative-health receipt | Press to damage |
| --- | --- | --- | --- | --- |
| 1 | 5.0519 s | 5.0669 s | 5.1475 s | 95.6 ms |
| 2 | 14.3170 s | 14.3330 s | 14.3970 s | 80.0 ms |
| 3 | 22.9657 s | 22.9834 s | 23.0637 s | 98.0 ms |
| 4 | 30.9825 s | 30.9991 s | 31.0798 s | 97.3 ms |

Each next damage delta is -1.4911; each occurs after the observed Defend=true and before API/observed release. No local counter message accompanies any attempt. All tracks run at speed 1.15, phase 0.0192–0.0240 s at execution; distances 2.259–4.541 studs with clear LOS and facing gates passing. Actual API holds were 130.8–134.3 ms, press-to-request delay 15.0–17.8 ms, request spans 116.8–118.5 ms. Each attempt is later followed by an observed Counter=5 notice. These observations demonstrate an unsuccessful damage-prevention baseline under these conditions; they do not reveal server input acceptance or prove the responsible attack. A missing synthetic press or premature key release is not supported by this capture.

All four attempts were also preceded by local damage approximately 0.40–0.42 s earlier. Being hit by deliberately un-parried preceding combos is a potential confounder; the capture has no affirmative server acceptance or measured stun eligibility. Do not assume changing delay alone fixes this. Longer holds are not supported as the immediate explanation: damage occurred while F remained held.

### Hit-3 follow-up and next comparison

Against Thug6, attempt 1 at 7.6004 s has a sole-candidate self counter at 7.7316 s (~131 ms afterward, no input conflicts). Attempts 2 and 3 precede damage by 113.6 and 113.2 ms with no self counter. This remains mixed and must retain its target context separately from Shadow/Thug5.

**Handoff:** both selected-hit filters and stop/save-on-switch are now observed live. The hit-4 zero-offset baseline has four repeated damage-associated outcomes; another unchanged generic baseline is unnecessary. A bounded next comparison can keep hit 4 selected and hold=120 while changing only offset to **+30 ms**. This deliberately tests a later press, not a claimed calibrated fix: the baseline requests precede damage by roughly 64–81 ms, leaving room for a small controlled shift in client scheduling. Server receipt order, hit-stun/eligibility and attacker attribution remain unknown. Keep the original seed at 0 and record the offset explicitly; do not update other combo seeds from this trial. The +30 ms comparison is proposed, **not applied** during this review.


## SP-023 / SP-T030 — all-hit auto parry restored (2026-11-17)

**User instruction:** add auto parry back. Combat page **v11 / diagnostics v5** restores automatic evaluation of all five known combo IDs, including the previously disabled first hit. This supersedes the temporary first-hit-disabled and single-hit-only experiments. Combat section is simply **Auto Parry**; the Hit 3 / Hit 4 test buttons and their filters are removed, and the original compact control layout is restored.

Catalog: combo 1 (114493250337466)=0.04 s, combo 2 (84394230178792)=0.04 s, combo 3 (95937874840716)=0 s, combo 4 (139487903747364)=0 s, combo 5 (131649056441662)=0.04 s. These are retained experimental seeds, not newly calibrated timings. The proposed +30 ms hit-4 experiment was not applied. Installed saved offset=0 / hold=120 at preparation; those settings remain unchanged. Existing saved TestHit is ignored and omitted on subsequent normal saves, so a prior TestHit=4 cannot restrict restored coverage.

Continuous diagnostics, request/input correlation, scoped head-effect/Z-vanish LOS exclusions, observed cooldown gates, range/facing checks, target ownership/PvP/PvE preferences, typing/menu pauses and release/stop behavior remain. Auto parry still starts OFF on reload; the user enables it. Reports identify scope=all_five in Config/START/CUE_ACCEPTED, while unknown animations remain unmapped. Removing the temporary restrictions does not resolve the recorded hit-3/hit-4 timing limitations; preserve their evidence in SP-T024–SP-T029.

**Validation:** 174 mocked assertions passed: 47 logger/lifecycle, 42 active catalog/LOS/deferral/counter, 26 PvP/PvE, 6 settings/reload, 34 async correlation/LOS, and 19 restoration-specific checks. The latter load legacy TestHit=4 and verify every known hit generates an eligible input for both NPC and player ownership, test controls/filters are absent, old selection is omitted on save, scope is explicit and cleanup completes. Updated source and both staged builds compile. Restoration is not yet live-validated. Exact module replacement preserves concurrent changes in public/private builds.

**Handoff:** reload and enable **Combat → Auto Parry**. All five known combos can now trigger under their existing gates, with logging automatic while enabled. Do not reintroduce single-hit restrictions without a new request; retain the recorder to evaluate real outcomes and unresolved timing issues.


## SP-024 / SP-T031 — all-hit restoration verified live against Shadow (2026-11-17)

Reviewed the completed diagnostics-v5 session starting Unix 1794956187, main-world place 110405289082842, 152.328 s, manually disabled. Header scope=all_five, range=8, offset=0 ms, hold=120 ms, PvP/PvE enabled. A newer history starting 1794956345 was still RUNNING when inspected (snapshot at 10.019 s), so its partial counts are not merged with this finalized report; held=true in that mid-pulse snapshot is not evidence of a stuck key.

**All-hit restoration verified:** 29 automatic attempts cover every known combo, all targeting the Shadow NPC binding: combo 1=10, combo 2=8, combo 3=6, combo 4=4, combo 5=1. No first_hit_disabled or trial_hit_filtered rejection remains. All 29 F-up API calls succeed and have correctly paired Defend=false with matching cycleAttempt. Final held=false, zero dropped lines, no input/release/logger/error/frame-gap failures. The restored scope and cleanup meet their acceptance checks; timing reliability is a separate question.

Totals: 9 self-counter messages, 15 other-counter messages, 6 possible-parried heuristic matches, 0 negative-health observations. The lack of damage against Shadow cannot establish that 29 attacks were successfully parried or be pooled directly with Thug5 damage trials. Other player/NPC bindings were observed, but none was an actual automatic-input target.

Of the nine self-counter receipts, only **two** have a sole automatic candidate with no subsequent-input conflict: attempt 7 at 50.6310 s and attempt 12 at 78.4632 s (both combo 2; approximately 167/183 ms after their presses). The message at 12.1483 s has sole candidate attempt 2 but inputConflicts=[2], so it must not be counted as an isolated automatic result. Four more receipts are ambiguous between two attempts with intervening input flags (41.1305, 93.2612, 101.5622, 128.4103 s); two have no recent automatic candidate (8.8991, 115.1778 s). The logger retains these distinctions rather than crediting every counter to automation.

Observed extra F cycles are correctly retained as unmatched. The report contains 94 DEFEND_REQUEST events in total, including 58 request events for the 29 paired automatic pulses. Request observation coverage is present; neither API success nor request observation is server acceptance.

Press-to-Defend delay is 13.2–19.8 ms (median 16.3); actual API holds 128.4–137.2 ms; adjacent automatic request spans 113.2–122.9 ms. Release-request lag is 0.7–3.6 ms. One bounded cooldown deferral occurs at 10.9678 s with 49.1 ms remaining, followed by attempt 1 at 11.0172 s; this confirms execution, not a successful outcome. Skip totals: attacker facing 8, Counter cooldown 22, F already down 5, range 8, self-facing 5, unmapped animation 554. No LOS rejection occurs.

**Handoff:** retain restored all-five-hit mode as requested; no timing/settings/code changed during this review. Scope and input cleanup are live-verified. Counter attribution remains mixed, and zero damage against Shadow is not evidence of general reliability. Review the newer run only once enough evidence is available, keeping session boundaries and target context explicit. Do not reintroduce single-hit restrictions or silently tune seeds from raw counter totals.


## SP-025 / SP-T032 — later-hit eligibility audit after restoration (2026-11-17)

The previously partial session starting Unix 1794956345 is now finalized: diagnostics v5, main-world place 110405289082842, scope=all_five, offset=0 / hold=120, both target filters enabled, 119.398 seconds, manually disabled. All 21 automatic attempts target Shadow: hit 1=3, hit 2=12, hit 3=6, hits 4/5=0. All 21 releases succeed and have paired Defend=false receipts with cycleAttempt; final held=false, no dropped lines or input/release/logger/error/frame-gap events. Zero negative-health observations again cannot prove successful parries against this target.

Nine self-counter messages comprise six sole-candidate, conflict-free temporal associations (five hit 2, one hit 3), one ambiguous receipt between attempts 12/13 with input conflict on 12, and two without recent automatic candidates. This is correlation, not confirmed causation or a six-of-21 success rate. Seven other-counter messages and seven possible-parried heuristic matches are separate from local success.

**Later-hit finding:** all seven observed hit-4 cues and all five hit-5 cues reach the mapped cue path. Hit 4 skips: four Counter cooldown, three F already down. Hit 5 skips: four Counter cooldown, one F already down. Thus all 12 later-hit cues are accounted for without a selector/filter regression. Three of the four F-down-priority skips also have a positive cooldown, so skip totals reflect the first failing gate, not mutually exclusive underlying conditions. At these decisions the hub itself is not holding F. Input state alone does not establish whether an extra press came from the user or another input source.

**Cooldown audit:** reconstructing each later-hit deadline from the latest received Counter duration reproduces the reported remaining time within 0.4 ms. Explicit cooldown skips have 0.838–4.948 seconds remaining, well beyond the existing 80 ms bounded deferral. Source uses the latest named Counter notice to replace the deadline, including shorter notices. No arithmetic/stale-deadline bug is demonstrated. Whether the named notice always denotes actual parry unavailability is still unverified; do not bypass it or assume its server semantics from its name alone.

Two separate LOS rejections involve Workspace.Thrown.explosion (non-collidable, queryable Part) and Workspace.Thrown.Debris (collidable, queryable Part). A cosmetic explosion exclusion is a candidate for further inspection, not applied based on this single observation. Do not ignore all Thrown descendants or collidable debris. Existing exact local-head exclusion remains observed.

**Decision / next action:** keep all five hits and automatic recording active-capable; runtime and saved settings unchanged. Before changing timing, obtain a defensive-only encounter against a damage-dealing NPC with normal parry availability and no extra F input or outgoing attacks. This separates cooldown/input eligibility from timing and opponent-parry effects while retaining full combo coverage. Compare per-hit attempts, preceding damage/stun evidence, cooldown receipts, request pairs and counter/damage outcomes. If hit 4 reaches input but repeats the earlier damage baseline, compare a bounded +30 ms hit-4-only delay while leaving other seeds unchanged; that comparison is still proposed, not installed. No generic global timing shift or single-hit restriction is justified by this session.


## SP-026 / SP-T033 — Thug8 baseline and bounded hit-3 delay comparison (2026-11-17)

Reviewed finalized session starting Unix 1794956677, diagnostics v5, main-world place 110405289082842, all-five scope, offset=0 / hold=120, both target types enabled. Duration 75.725 seconds, manually disabled. All 20 automatic attempts target Thug8: hit 1=13, hit 2=4, hit 3=3, hits 4/5=0. All 20 API releases succeed and have paired Defend=false receipts with matching cycleAttempt. Final held=false, no dropped lines or input/release/logger/error/frame-gap failures. There are 22 negative-health observations, 14 self-counter messages, two other-counter messages and one possible-parried heuristic match.

**First hit:** 12 of 13 attempts have a sole-candidate self-counter association without conflicting input, 172–232 ms after the API press. Attempt 17 has no such isolated result; a later counter lists both attempts 17/18. Preserve this stronger first-hit baseline, while retaining that receipt attribution is temporal rather than server-confirmed causation. **Second hit:** one clean sole-candidate counter, one ambiguous counter shared with the prior hit-1 attempt, and two damage-associated attempts (+172.3 / +168.8 ms). The two damage receipts arrive after F release; this observation alone does not establish that extending the hold would fix them.

**Hit 3 repeatedly reaches input but takes damage:** attempts 14/16/20 press at 39.6267 / 52.9930 / 63.8961 seconds and precede damage by 98.2 / 96.8 / 113.1 ms, with no self-counter. Each damage receipt occurs after Defend=true and before both API and observed Defend=false releases. Tracks execute at phase 0.0192 seconds, speed 1.15, distance 3.910–4.322 studs, with facing/LOS passing and no reported cooldown or pre-existing F press. Preceding local damage is approximately 319 / 370 / 354 ms before these presses, so hit-stun or other server eligibility remains a confounder. The final sequence also contains outgoing attack activity and a possible-parried match: do not label the whole capture perfectly passive or blame extra input on the user without proof.

**Later-hit eligibility:** both observed hit-4 cues and both hit-5 cues are skipped for Counter cooldown, with remaining durations 0.8156 / 4.9130 seconds and 0.0985 / 4.2889 seconds respectively. Their reconstructed deadlines agree with received notices. The 98.5 ms remainder is above the existing 80 ms deferral bound, so it is skipped as designed. No f_already_down skip or LOS rejection occurs in this session. There is no new hit-4/5 timing result.

**Implemented comparison — Combat page v12 / diagnostics v5:** change only hit-3 animation 95937874840716 seed from 0 to 0.03 seconds. Keep hits 1/2/4/5 at 0.04/0.04/0/0.04, all-five coverage, hold preference, additive global offset, cooldown handling, filters and continuous diagnostics. This tests a slightly later press against the repeated zero-delay baseline; it is not a calibrated fix. Observed hit-3 Defend requests precede damage by approximately 82–88 ms, giving a bounded later-press comparison without claiming a server window. Do not extend holds or bypass cooldowns at the same time. The previously proposed hit-4 +30 ms comparison remains unapplied.

Config, START and CUE_ACCEPTED include timingTrial=hit3_plus30ms, and the seed catalog reports combo3:0.03. The Combat status identifies Hit 3 +30 ms trial while retaining all five hits and the normal controls. Starts OFF after reload. No saved settings are overwritten; a nonzero user offset still adds to every seed and must be retained in analysis.

**Validation:** source and both staged builds compile. 193 mocked assertions pass: 47 logger/lifecycle, 42 active-feature, 26 PvP/PvE, 6 settings, 34 asynchronous correlation/LOS, 19 all-hit restoration, 19 targeted timing/cleanup. Targeted checks confirm no hit-3 input before 30 ms, input after the deadline, unchanged other-hit scheduling, normal hold/release, geometry revalidation, cancellation on stop and additive user offset. Existing correlation fixtures were advanced past the new hit-3 deadline while preserving their API-relative event spacing. These are mocked scheduling checks, not live confirmation of improved parrying. Exact module replacement preserves unrelated concurrent modules. Existing Beyond Nen notes inform evidence separation; no foreign timing constants or remotes are imported.

**Handoff:** reload the installed hub and enable Combat auto parry; confirm the status identifies Hit 3 +30 ms trial. Repeat a comparable Thug encounter with offset=0 / hold=120, allowing automation to defend. Keep all hits enabled. Compare hit-3 request/health/counter timing against this baseline, retaining prior-damage and cooldown context. Revert the 30 ms trial if evidence worsens; do not claim success from request/API counts. Hit-4/5 cooldown eligibility and hit-2 mixed outcomes remain unresolved.


## SP-027 / SP-T034 — hit-3 delay shows no benefit; event-state diagnostics (2026-11-17)

Reviewed completed session starting Unix 1794957002, main-world place 110405289082842, 171.667 seconds, manually disabled. Config confirms timingTrial=hit3_plus30ms, combo3:0.03, all-five scope, offset=0 / hold=120. There are 25 automatic attempts: two against Shadow, then 23 against Thug5. All 25 releases succeed and have correctly paired Defend=false receipts; final held=false, zero dropped lines, no input/release/logger errors. A single 126.9 ms frame gap at 0.2914 seconds precedes the first automatic attempt by about 35 seconds; do not attribute later misses to it.

Totals: 11 self counters (all sole-candidate, no input conflicts: eight hit 1 and three hit 2), three other counters, two possible-parried heuristic matches, 44 negative-health observations. Automatic coverage: hit 1=11, hit 2=5, hit 3=6, hit 4=2, hit 5=1. API-to-observed-Defend latency is 15.7–34.6 ms, median 20.2; API hold 122.9–141.3 ms. These figures are not a server success rate. Outgoing attack activity and unmatched input remain present, so this is not a fully isolated passive comparison.

**Hit-3 trial verified, benefit not demonstrated:** one attempt against Shadow has no near-term damage or counter and is kept separate. All five against Thug5 precede damage, with no self counter: attempts 11/15/16/22/24 at 109.3524 / 126.5997 / 131.6854 / 147.6094 / 158.0434 seconds, damage after 76.5 / 54.7 / 72.2 / 50.4 / 82.5 ms. All damage occurs after observed Defend=true and before both API and observed release. Actual cue ages 38.1–50.6 ms confirm the later scheduling, with speed 1.15, phases 0.0623–0.0767 and facing/LOS passing. This failed to produce a successful hit-3 outcome in the damage-dealing encounter. The previous baseline used Thug8 and different encounter conditions, so do not claim a statistically established regression or infer an exact server parry window from receipt delays.

**Later hits now execute:** both hit-4 attempts against Thug5 precede damage (+53.6 / +99.0 ms) while F remains held, with no counter. The single hit-5 attempt precedes damage by 165.0 ms after release, with no counter. These do not prove the damage source or server acceptance. Additional hit-4 skips: five cooldown, one F already down; hit-5 skips: six cooldown, one F already down. Keep those eligibility failures separate from attempts that reached input. No LOS skip occurred.

**Implemented — Combat page v13 / diagnostics v6:** remove the unsuccessful +30 ms trial and restore hit-3 seed=0. All other seeds, all-hit coverage, settings and gates are retained. Config/START/CUE_ACCEPTED identify timingTrial=baseline_state_capture. Combat status identifies Diagnostics v6. All inputs remain off after reload until enabled by the user.

Add bounded event-time local state snapshots immediately after a successful F-down API call and on negative-health events, alongside the existing half-second state-delta sampler. STATE_CONTEXT records trigger=after_input with attempt/cue, or trigger=damage with damage delta/recent candidates, plus lastDamageAge, fields and omitted. STATE_POINT lists up to 64 sorted relevant client-visible fields from character/humanoid/player attributes and direct character ValueBases, plus Health/MaxHealth. Repeated unchanged fields are retained in these snapshots; no absent state is invented. At a damage trigger, lastDamageAge refers to the preceding damage observation; at an input trigger it refers to the latest observed damage. It is unknown before the first recorded damage and resets for each session. Events obey existing report line/byte limits. Missing fields are unknown, not proof of no stun or server eligibility.

Snapshot collection runs after the key-down call so it does not postpone scheduling that press; it can still incur client overhead that needs live observation. Snapshot errors log STATE_CAPTURE_ERROR without interrupting normal F-release cleanup. The periodic sampler remains unchanged. As with the Beyond Nen reference workflow, use these observations to distinguish decision/input/result stages; no game-specific timing or remote semantics are imported.

**Validation:** source and both staged builds compile; 192 mocked assertions pass (174 existing logger/active/PvP/settings/correlation/restoration checks plus 18 event-state checks). New checks cover a transient state between periodic samples, previous-damage age/session reset, 64-field bound, absent fields remaining absent, zero-delay rollback, and snapshot-error isolation from release cleanup. These validate the implementation, not live performance or improved parrying. Exact module transplantation preserves unrelated concurrent changes; saved settings are not overwritten.

**Handoff:** reload and enable the Diagnostics v6 build, with offset=0 / hold=120 for comparison. All five hits and automatic recording remain. Review after_input and damage snapshots for short-lived replicated stun/busy/attack/counter fields and previous-damage timing before another timing adjustment. A field appearing is evidence of client state only; do not invent a stun gate or bypass cooldowns based solely on its name. If no useful state is exposed, record that limitation rather than repeating arbitrary delay trials. Hits 2–5 remain unreliable; the hit-4 +30 ms proposal remains unapplied.


## SP-028 / SP-T035 — event snapshots validated; separate Shadow and PvP results (2026-11-19)

**Evidence and configuration:** reviewed three finalized diagnostics-v6 histories starting Unix 1795076236, 1795076353 and 1795076420. All report universe 10771972206 and place **135537125271116**, an additional recorded place rather than the previously documented main-world place. Its name/role is not verified. All use baseline_state_capture, all-five scope, offset=0 ms, hold=120 ms, range=8, PvP/PvE enabled, and end by manual disable. Exact source-module comparison confirms the current public artifact and installed private build each contain the same v13 module. No runtime or settings changed during this review.

| Capture | Duration | Actual automatic target | Attempts by hit 1/2/3/4/5 | Self-counter receipts | Negative-health events |
| --- | --- | --- | --- | --- | --- |
| First, 1795076236 | 88.696 s | Shadow NPC | 1 / 3 / 2 / 2 / 0 | 2, both sole-candidate and conflict-free | 0 |
| Second, 1795076353 | 66.397 s | Shadow NPC | 2 / 7 / 1 / 0 / 0 | 8, all sole-candidate and conflict-free | 0 |
| Third, 1795076420 | 195.844 s | Player binding 4 | 2 / 2 / 0 / 1 / 2 | 5: three without an automatic candidate, two ambiguous | 33 |

**NPC observations:** the first run has a hit-4 counter association at 45.3453 s, about 81 ms after attempt 2, and a hit-1 association at 47.0294 s, about 184 ms after attempt 3. The second has two hit-1 and six hit-2 associations, about 181–184 ms after input. These ten receipts have one candidate and inputConflicts=[]; they are stronger temporal evidence, not server-confirmed attribution or a ten-of-18 success rate. The three hit-3 attempts across the two runs have neither a self-counter nor negative health. Shadow's zero-damage encounters cannot establish general defense reliability or replace damage-dealing NPC evidence. Later-hit cooldown skips reconstruct from the received Counter notices; no selector regression or cooldown arithmetic error was found.

**PvP observations:** all seven automatic attempts target a player, confirming that player recognition reaches input with PvP enabled. They do not establish successful PvP defense. Counter receipts at 26.9143, 56.3801 and 79.4638 s have no recent automatic candidate. Receipts at 59.6648 and 100.9121 s list two candidates with intervening-input conflicts. None qualifies as an isolated automatic result. Unmatched F cycles and outgoing attack activity remain in the capture; their source and causal effect cannot be assumed.

Three of the seven PvP attempts precede damage within half a second: hit 5 at 4.8502 s (+133.5 ms), hit 1 at 58.6807 s (+165.4 ms), and hit 2 at 59.1313 s (+165.0 ms). The hit-5 damage occurs just before API release; the other two follow release. Their preceding-damage ages at input are approximately 401 ms, 3.117 s and 286 ms. This mixed pattern does not establish a single hold/delay correction or prove the damaging attacker. The other four attempts have no damage within half a second, which is also not proof of a successful block. Known-cue skips include 22 out-of-range, two Counter cooldown, one F-already-down and one attacker-facing rejection. All six known hit-3 cues were out of range, so this session supplies no PvP hit-3 input-timing result. The 702 unmapped-animation skips include ordinary overlapping animations; they are not 702 demonstrated missed attacks or sufficient evidence to expand the attack catalog.

**Diagnostics v6 verified live:** all 25 successful F-down API events have an after_input snapshot, and all 33 negative-health observations have a damage snapshot. There are 58 contexts and 632 state fields (8–12 per snapshot), omitted=0 throughout, no capture errors, no dropped lines and no frame-gap failures. Fields include Health/MaxHealth, AttackerTags, Fatigue, Ki, PowerLevel, Combo, CounterCooldown, PBlock, LastAttacker and BaseMaxHealth where exposed. One damage snapshot also has Ragdolled=false. No stun or busy field appears in these snapshots; absence only describes this recorder's client-visible attribute/direct-value coverage. It does not prove the character is free to parry. LastAttacker is a string observation, not a guaranteed attribution for each damage event; its value remains private.

PBlock is false at the three damage-associated PvP presses and their damage snapshots, but is also false at nine isolated NPC counter-associated presses; it is absent from the remaining hit-4 snapshot. PvP attempt 6 instead has PBlock=true at input, without a following self-counter. Thus the available field values do not provide a demonstrated eligibility classifier. CounterCooldown is an opaque absolute numeric value; do not compare it directly with client os.clock or infer enforcement from its name. No useful replicated stun/busy discriminator emerged from this batch.

**Input cleanup:** all 25 automatic F-up calls report success and all 25 Defend=false observations have valid matching cycleAttempt pairs. Every final report has held=false. API holds range 130.8–138.4 ms; observed request spans 116.5–124.2 ms; input-to-Defend delay 14.3–18.1 ms. This verifies logging, request correlation and release behavior in these runs, not server acceptance or calibrated defense timing.

**Decision and handoff:** preserve v13 baseline timing, all five hits, filters, observed cooldown gates and automatic recording. No code deployment or reload is required for this review. Event-state capture has met its live acceptance checks, but it did not resolve eligibility. Keep the Shadow findings separate from the player encounter and earlier Thug failures. Do not ask for another unchanged generic capture, claim success from zero damage, add every unmapped animation, invent a stun gate or make a global timing shift. A future timing/catalog change needs a concrete discriminating comparison or a verified game-specific state/attack mapping; this batch does not select one. Public notes contain only sanitized findings; raw captures and player identifiers stay local. No compile or mock suite was rerun because only documentation changed; prior implementation checks remain recorded in SP-T034.


## SP-029 / SP-T036 — bounded PvP range-entry recheck (2026-11-19)

**Existing evidence audited:** continued analysis of SP-T035's completed PvP run (Unix 1795076420, place 135537125271116). Of 22 out-of-range mapped-cue rejections, 13 concern the player binding targeted by the automatic attempts, while nine concern other nearby players. Ten cues start within the configured 8-stud radius plus 3 studs; nine of these also pass the recorded facing thresholds, all from the main opponent. Eight of those nine precede local negative health within 300 ms. Examples: hit 2 at 3.5156 s, distance 9.879, damage about 250 ms later; hit 4 at 4.3663 s, distance 9.998, damage about 83 ms later; hit 3 at 103.8282 s, distance 8.777, damage about 117 ms later. Several other rejections have substantial vertical separation or involve bystanders, so increasing range globally is not supported.

**Confirmed source behavior versus hypothesis:** the old observe path permanently discards a cue on its first out-of-range result. It cannot recover that swing even if the opponent subsequently enters range. The recording has no continuous root-position trace for those discarded cues, so it cannot prove that any particular opponent crossed the boundary before impact or that the observed damage came from that cue. Recovery is a bounded implementation improvement for that demonstrated code path, not a retrospectively measured missed-parry count or a proven live fix.

**Three damage-associated presses:** the hit-5 damage at 4.9837 s occurs about 4.9 ms before API release. The hit-1 and hit-2 damage at 58.8461 / 59.2963 s follows their API releases by approximately 32 ms. These mixed observations do not determine a shared hold correction; existing request timing and state snapshots also do not establish server acceptance. Preserve baseline seeds and hold preference instead of changing both coverage and timing together.

**Implemented / untested live — Combat page v14, diagnostics v7:** a fresh mapped player animation just outside the user's configured range can enter a brief approach queue. Admission requires current allowed player ownership, playing forward animation with finite positive speed and phase 0–50 ms, distance no greater than Range+3 studs, normal facing and line of sight. Only two approach cues may wait concurrently, within the existing eight-cue total limit. NPC cues and players beyond the margin retain their existing immediate rejection behavior.

An approach cue checks at 20 ms intervals, at most four times and no later than 80 ms from observation. If it enters the actual configured range, it can proceed through the normal gates; it never advances the original per-hit seed or additive user offset. Entry before the seed waits until the original deadline and revalidates geometry again. Expired cues are discarded even if a delayed frame subsequently sees an in-range target. Rechecks retain manual-F, held-key, current/live ownership, playing-track, optional knocked/off-screen, facing, wall, cooldown, typing/menu, stop and cleanup checks. The actual parry range does not increase. All five mapped hits remain enabled; no new attack IDs, cooldown bypass, direct remote actuation, NPC timing change or hold change is introduced.

**Recording:** header/config declares pvpRangeRecheckMs=80, rangeMargin=3, rangeStepMs=20 and rangeSlots=2. RANGE_WAIT marks admission, RANGE_CHECK retains per-check client distance/vertical/facing and track context, and RANGE_ENTERED marks observed entry before normal execution. Entry alone does not mean an input or parry succeeded. Normal CUE_ACCEPTED/F_DOWN/request/release/outcome logs remain the evidence for later stages; range_wait_expired and range_queue_full explain bounded rejections. Report remains subject to existing limits. This closes the missing client-geometry observation for future approach cues without claiming to reconstruct old movement. Combat status identifies PvP range recheck v14. Existing Recorders controls and state snapshots remain.

**Validation:** 231 mocked assertions pass: the existing 192 plus 39 approach scenarios covering movement across the actual boundary, seed/offset preservation, zero-delay hit recovery, expiry and delayed frames, unchanged NPC rejection, distant players, two-slot bound with room for a normal cue, duplicate/invalid tracks, walls, ownership/death, manual input, cooldown, typing/menu, PvP toggle, stop and release cleanup. Updated module and both staged builds compile; both regenerated build variants are also compile-checked before installation. Tests validate local scheduling and gating, not improved live PvP defense. Exact module replacement and hash-checked installation preserve unrelated modules and private/public separation. The shared bounded decision/input/result methodology follows the existing Sparkling/Beyond Nen notes; no foreign combat timings were imported.

**Handoff:** reload and enable the installed v14 build with the user's existing settings. A useful comparison now has a specific question: do RANGE_WAIT cues actually enter the configured radius within the bound, and do those inputs produce isolated counters or damage? Compare RANGE_CHECK geometry, RANGE_ENTERED, subsequent execution/release and outcomes; retain NPC/player separation and ambiguous-input flags. If there are no entries, do not call the change effective or enlarge the margin automatically. Hold timing and server eligibility remain unresolved. No unchanged generic recording is required to justify this implementation; future live validation concerns this explicit coverage change.
